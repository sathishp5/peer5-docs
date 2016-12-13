# Use NGINX As Warm Wowza Cache

Wowza server can both transcode and serve you HLS/DASH stream,
but sometimes serving the files is done through other servers  
in order increase the amount of users that your servers can serve.
So instead of transcoding the stream multiple times, one server can generate the stream and the content can be served through caching servers.

In this guide we will use Ubuntu server with nginx as a warm cache for wowza.


## Example setup

A standard setup might be compiled from the following servers:

- 1x Wowza transcoder
- 3x NGINX caching server (Ubuntu)
<br>
<br>
![](images/wowza-nginx.png)
<br>
<br>

In this example the servers are connected to the same local network,
though they can also be remote and IP address should be changed respectively.

## Install nginx

```
apt-get update
apt-get install nginx -y
```

## Server tuning

In order to allow the cache servers to fully utilize their resources,
several adjustments should be made. 

### tune global limits

add to `/etc/sysctl.conf`
```
fs.inotify.max_user_instances=1048576
fs.inotify.max_user_watches=1048576
fs.nr_open=1048576
net.core.netdev_max_backlog=1048576
net.core.rmem_max=16777216
net.core.somaxconn=65535
net.core.wmem_max=16777216
net.ipv4.ip_local_port_range=1024 65535
net.ipv4.netfilter.ip_conntrack_max=1048576
net.ipv4.tcp_fin_timeout=5
net.ipv4.tcp_max_orphans=1048576
net.ipv4.tcp_max_syn_backlog=20480
net.ipv4.tcp_max_tw_buckets=400000
net.ipv4.tcp_no_metrics_save=1
net.ipv4.tcp_rmem=4096 87380 16777216
net.ipv4.tcp_synack_retries=2
net.ipv4.tcp_syn_retries=2
net.ipv4.tcp_tw_reuse=1
net.ipv4.tcp_wmem=4096 65535 16777216
net.nf_conntrack_max=1048576
```

### tune user limits

add to `/etc/security/limits.conf`
```
*               soft    nofile          1048576
*               hard    nofile          1048576
*               soft    nproc          1048576
*               hard    nproc          1048576
*               soft    memlock         unlimited
*               hard    memlock         unlimited
```

## NGINX tuning

Here is a complete nginx config tuned for caching.  
Be sure to edit the upstream section to match your wowza server ip and port,
multiple servers can be added - one per line (don't forget the semi-colon)

replace `/etc/nginx.conf` completely with this
```
user nginx;
worker_processes auto;
pid /var/run/nginx.pid;
worker_rlimit_nofile 1048576;

events {
    worker_connections 1048576;
    multi_accept on;
    use epoll;
}

http {

    # upstream
    upstream wowza {
      ip_hash;
      # ADD YOUR SERVERS HERE - ONE PER LINE
      #server 192.168.1.2:1935;
    }

    # basic
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    server_tokens off;
    keepalive_timeout 300s;
    types_hash_max_size 2048;
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    # ssl
    ssl_ciphers "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:50m;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    # ssl_dhparam /etc/ssl/certs/dhparam.pem; # need to generate the .pem certifiate before using this
    resolver 8.8.4.4 8.8.8.8 valid=300s ipv6=off;
    resolver_timeout 10s;

    # logs
    access_log off;
    error_log /var/log/nginx/error.log;

    # gzip
    gzip on;
    gzip_disable "msie6";
    gzip_http_version 1.1;
    gzip_comp_level 6;
    gzip_types text/plain text/css application/json application/javascript text/javascript application/x-javascript text/xml application/xml application/xml+rss application/vnd.ms-fontobject application/x-font-ttf font/opentype font/x-woff image/svg+xml image/x-icon;

    # proxy
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_read_timeout 10s;
    proxy_send_timeout 10s;
    proxy_connect_timeout 10s;
    proxy_cache_path /var/cache/nginx/wowza_cache_temp use_temp_path=off keys_zone=wowza_cache_temp:10m max_size=20g inactive=10m;
    proxy_cache wowza_cache_temp;
    proxy_cache_methods GET HEAD;
    proxy_cache_key $uri;
    proxy_cache_valid 200 302 5m;
    proxy_cache_valid 404 3s;
    proxy_cache_lock on;
    proxy_cache_lock_age 5s;
    proxy_cache_lock_timeout 1h;

    # default route
    server {
        listen 80 default_server;

        #listen 443 ssl default_server;
        #ssl_certificate /path/to/cert.crt;
        #ssl_certificate_key /path/to/cert.key;

        add_header X-Cache-Status $upstream_cache_status;

        location ~ \.(m3u8|mpd)$ {
            proxy_cache_valid 200 302 5s;
            proxy_pass http://wowza;
        }

        location / {
            proxy_pass http://wowza;
        }
    }
}
```

**NOTE: Did you update you wowza ip address? if not go back and update it**
<br>
<br>

after setting up the config we check that it's valid
```
service nginx configtest
```
should output something like:
```
* Testing nginx configuration                   \[ OK ]
```


Then we restart the server to apply the new configuration 
```
service nginx restart
```

## Load Balancing

The last step is to point you users to request the content from nginx as if its wowza.
This can be done using DNS or via randomly assigning one of the servers.

`http://wowza.example.com/live/smil:channel1.smil/playlist.m3u8`

now becomes  
`http://origin1.example.com/live/smil:channel1.smil/playlist.m3u8`
`http://origin2.example.com/live/smil:channel1.smil/playlist.m3u8`
`http://origin3.example.com/live/smil:channel1.smil/playlist.m3u8`

Or  
`http://origin.example.com/live/smil:channel1.smil/playlist.m3u8`

that resolves to either using [round-robin](https://en.wikipedia.org/wiki/Round-robin_DNS) or other logic

Happy caching!

# Setting up HLS live streaming server using NGINX + nginx-rtmp-module on Ubuntu

This guide will explain how to setup your own streaming server on ubuntu.

## 1. Compile nginx with rtmp module

Firstly, we'll need to compile nginx with the nginx-rtmp-module.

We recommend using [this](https://github.com/sergey-dryabzhinsky/nginx-rtmp-module) forked module. it's being actively worked on and contains more fixes and improvements over the [original one](https://github.com/arut/nginx-rtmp-module)

__Clone nginx-rtmp-module__

    git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git

__Install nginx dependencies__

    sudo apt-get install build-essential libpcre3 libpcre3-dev libssl-dev

__Download nginx__  
Latest nginx can be downloaded from [this page](http://nginx.org/en/download.html).  
for example `nginx-1.10.1` can be downloaded from this link: [http://nginx.org/download/nginx-1.10.1.tar.gz](http://nginx.org/download/nginx-1.10.1.tar.gz) 
  
    wget http://nginx.org/download/nginx-1.10.1.tar.gz
    tar -xf nginx-1.10.1.tar.gz
    cd nginx-1.10.1

__Compile nginx__

    ./configure --with-http_ssl_module --add-module=../nginx-rtmp-module
    make -j 1
    sudo make install
    
- Notice the `--add-module=../nginx-rtmp-module` argument, the path must point correctly to the cloned module
- (Optional) replace -j 1 with the amount of cpu's on your computer to accelerate the compilation
  


## 2. Create nginx configuration file

__rtmp module config__

An application in nginx means an rtmp endpoint  
Basic uri syntax: `rtmp://nginx_host[:nginx_port]/app_name/stream_name`  
We will be using `stream` as our stream name so our endpoint will be: `rtmp://localhost/show/stream`
Which will later be available as `http://localhost:8080/hls/stream.m3u8`

For good HLS experience we recommend using 3 seconds fragments with 60 seconds playlist.  

```
rtmp {
    server {
        listen 1935; # Listen on standard RTMP port
        chunk_size 4000;

        application show {
            live on;
            # Turn on HLS
            hls on;
            hls_path /mnt/hls/;
            hls_fragment 3;
            hls_playlist_length 60;
            # disable consuming the stream from nginx as rtmp
            deny play all;
        }
    }
}
```

Note that the example points `/mnt/hls/` as the target path for the hls playlist and video files.  
You can change this to a different directory but make sure that nginx have **write permissions**.  


__http server config__

Since hls consists of static files, a simple http server can be set up with two additions, correct MIME types and CORS headers.

```
server {
    listen 8080;

    location /hls {
        # Disable cache
        add_header Cache-Control no-cache;

        # CORS setup
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
        add_header 'Access-Control-Allow-Headers' 'Range';

        # allow CORS preflight requests
        if ($request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Headers' 'Range';
            add_header 'Access-Control-Max-Age' 1728000;
            add_header 'Content-Type' 'text/plain charset=UTF-8';
            add_header 'Content-Length' 0;
            return 204;
        }

        types {
            application/vnd.apple.mpegurl m3u8;
            video/mp2t ts;
        }

        root /mnt/;
    }
}
```


__the complete nginx.conf__

The default location for nginx conf is `/usr/local/nginx/conf/nginx.conf` or `/etc/nginx/nginx.conf`

```
worker_processes  auto;
events {
    worker_connections  1024;
}

# RTMP configuration
rtmp {
    server {
        listen 1935; # Listen on standard RTMP port
        chunk_size 4000;

        application show {
            live on;
            # Turn on HLS
            hls on;
            hls_path /mnt/hls/;
            hls_fragment 3;
            hls_playlist_length 60;
            # disable consuming the stream from nginx as rtmp
            deny play all;
        }
    }
}

http {
    sendfile off;
    tcp_nopush on;
    aio on;
    directio 512;
    default_type application/octet-stream;

    server {
        listen 8080;

        location / {
            # Disable cache
            add_header 'Cache-Control' 'no-cache';

            # CORS setup
            add_header 'Access-Control-Allow-Origin' '*' always;
            add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
            add_header 'Access-Control-Allow-Headers' 'Range';

            # allow CORS preflight requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Headers' 'Range';
                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain charset=UTF-8';
                add_header 'Content-Length' 0;
                return 204;
            }

            types {
                application/dash+xml mpd;
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }

            root /mnt/;
        }
    }
}

```

## 3. Start nginx

Test the configuration file

    nginx -t

Start nginx in the background

    nginx
    
Start nginx in the foreground

    nginx -g 'daemon off;'
    
Reload the config on the go

    nginx -t && nginx -s reload

Kill nginx
    
    nginx -s stop


## 4. Pushing live stream to nginx using rtmp

nginx accepts rtmp stream as input.
For a proper HLS stream the video codec should be [`x264`](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) and audio codec `aac`/`mp3`/`ac3` most commonly being `aac`.

### Options 1: From existing rtmp stream already in h264
 
 if you have an existing rtmp stream in the correct codec, you can skip ffmpeg and tell nginx to pull the stream directly.
 In order to do so add a `pull` directive under `application` section in nginx.conf like so:
 
```
application show {
    live on;
    pull rtmp://example.com:4567/sports/channel3 live=1;
    # to change the local stream name use this syntax: ... live=1 name=ch3; 

    # other directives...
    # hls_...
}
            
```
Read on more available options [here](https://github.com/arut/nginx-rtmp-module/wiki/Directives#pull)
 
### Options 2: From local webcam/different rtmp/file

To achieve the stream encoding and muxing we will use the almighty [ffmpeg](http://ffmpeg.org/).

To install ffmpeg using PPA run these commands

    sudo add-apt-repository ppa:mc3man/trusty-media
    sudo apt-get update
    sudo apt-get install ffmpeg

There are several source from which you can produce an rtmp stream. here are couple examples:
Update `localhost` to your nginx server ip/domain

Capture webcam on `/dev/video0` and stream it to nginx

    ffmpeg -re -f video4linux2 -i /dev/video0 -vcodec libx264 -vprofile baseline -acodec aac -strict -2 -f flv rtmp://localhost/show/stream
  
-  `-re` - consume stream on media's native bitrate (and not as fast as possible)
-  `-f` - use video4linux2 plugin
-  `-i` - select physical device to capture from
-  `-vcodec` - specify video codec to output
-  `-vprofile` - use x264 baseline profile
-  `-acodec` - use aac audio codec
-  `-strict` - allow using the experimental aac codec
-  `-f` - specify format to output
-  `rtmp://localhost/show/stream` - rtmp endpoint to stream to. if the target port is not `1935` is should be included in the uri.  
 The last path component is the stream name - that means that multiple channels can be pushed using different names

Stream file example-vid.mp4

     ffmpeg -re -i example-vid.mp4 -vcodec libx264 -vprofile baseline -g 30 -acodec aac -strict -2 -f flv rtmp://localhost/show/stream

Stream another rtmp stream

     ffmpeg -i rtmp://example.com/appname/streamname -vcodec libx264 -vprofile baseline -acodec aac -strict -2 -f flv rtmp://localhost/show/stream

## 5. Take the server for a test run!

Now that we are pushing our stream into nginx, a manifest file in the format `stream-name.m3u8` is created in the target folder along with the video fragments.

For our example, the manifest is available at: `http://localhost:8080/hls/stream.m3u8`.

For testing our new HLS live stream we will use [videojs5](http://videojs.com/).

player.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Live Streaming</title>
    <link href="//vjs.zencdn.net/5.8/video-js.min.css" rel="stylesheet">
    <script src="//vjs.zencdn.net/5.8/video.min.js"></script>
</head>
<body>
<video id="player" class="video-js vjs-default-skin" height="360" width="640" controls preload="none">
    <source src="http://localhost:8080/hls/stream.m3u8" type="application/x-mpegURL" />
</video>
<script>
    var player = videojs('#player');
</script>
</body>
</html>
```

With Peer5 plugin (get your API key [here](https://app.peer5.com/register)):

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JW7 Player test</title>
    <!-- peer5 client library -->
    <script src="//api.peer5.com/peer5.js?id=YOUR_PEER5_API_KEY"></script>

    <!-- videojs5 scripts - You can change to your self hosted scripts -->
    <link rel="stylesheet" href="//api.peer5.com/videojs5/assets/video-js.min.css">
    <script src="//api.peer5.com/videojs5.js"></script>

    <!-- peer5 plugin for videojs5 -->
    <script src="//api.peer5.com/peer5.videojs5.hlsjs.plugin.js"></script>
</head>
<body>

<video id="player" class="video-js vjs-default-skin" height="360" width="640" controls preload="none">
    <source src="http://localhost:8080/hls/stream.m3u8" type="application/x-mpegURL" />
</video>
<script>
    var player = videojs('#player');
</script>
</body>
</html>
```

It's alive!

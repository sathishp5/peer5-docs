## Cross Origin Resource Sharing (CORS)
 
[CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a security mechanism embedded in modern browsers that prevents hijacking resources from servers that have different domains.  
 
Peer5 performs requests from your video page to your video server,  
for best performance it is required that your video server will respond with the following headers per request type:

`GET`

- `'Access-Control-Allow-Origin'`: `'*'`
- `'Access-Control-Expose-Headers'`: `'Content-Length, Content-Range'`
---
`HEAD`

- `'Access-Control-Allow-Origin'`: `'*'`
- `'Access-Control-Expose-Headers'`: `'Content-Length'`
---

`OPTIONS`  
<small>options requests (sometime called preflight) are performed in many common browsers like Chrome and Firefox.</small>
  
- `'Access-Control-Allow-Origin'`: `'*'`
- `'Access-Control-Allow-Methods'`: `'GET, HEAD, OPTIONS'`
- `'Access-Control-Allow-Headers'`: `'Range'`
- `'Content-Length'`: `'0'`  
status: `200` | `204`
---

### Common servers examples
 
- [nginx](https://github.com/Peer5/peer5-cors-config/blob/master/nginx/nginx.conf)
- [nimble](https://github.com/Peer5/peer5-cors-config/blob/master/nimble/nimble.conf)
- [wowza](https://github.com/Peer5/peer5-cors-config/tree/master/wowza)
- [varnish](https://github.com/Peer5/peer5-cors-config/tree/master/varnish)


<br>
**Having problems?**  
<a href="javascript:Intercom('show')">Chat with us</a> or [send us an email](mailto:info@peer5.com)
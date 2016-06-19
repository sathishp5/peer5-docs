# F.A.Q

Need extra help? <a href="javascript:Intercom('show')">Chat with us</a> or [send us an email](mailto:info@peer5.com)

## Is it possible to stream video from one peer to another without using a server?
no. In order to use peer5, a proper HLS/DASH stream is needed - we recommend using [nimble](https://wmspanel.com/nimble)  
as server software

---

## Why is my stream is not accepted/is invalid?
In order to use peer5 you will need either an HLS stream or an MPEG-DASH stream.  
 unlike regular video files, these streams have a manifest file as their entry point.
  
- HLS - `.m3u8` manifest file.  
- DASH - `.mpd` manifest file.

If you are trying to integrate peer5 with any of these `.mp4` `.flv` `.mp3` `.ogg` `.webm` `.jpg` etc...  
it will not work since these are not valid streams.  

---

## How do I setup an HLS/DASH stream
In order to create an HLS/DASH stream you need to convert your `rtmp` or video file to the correct format.

You can either use a hosted service to convert and host the files for you such as:  

- [akamai](https://www.akamai.com/)
- [bitmovin](https://bitmovin.com/)
- [level3](http://www.level3.com/)
- [wowza streaming cloud](https://www.wowza.com/)

or use self hosted media servers such as:

- [wowza](https://www.wowza.com/)
- [nimble](https://wmspanel.com/nimble)
- [nginx-rtmp-module](https://github.com/arut/nginx-rtmp-module)
 
 ---
 
 Need extra help? <a href="javascript:Intercom('show')">Chat with us</a> or [send us an email](mailto:info@peer5.com)
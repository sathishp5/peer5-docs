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

## How do I disable Peer5 on a specific page?
In order to disable the Peer5 technology for specific pages,<br>
you can use our Javascript API to [disable](https://docs.peer5.com/guides/configuring-peer5/#disabling-p2p) and [enable](https://docs.peer5.com/guides/configuring-peer5/#enabling-p2p) Peer5.
 
---

## What are the supported Web Players?
We support Jwplayer, Clappr, Flowplayer, Videojs, Kaltura. We do not support OSMF player and other flash oriented players. 

---

## How to play HLS in HTML5?
The introduction of the video element in HTML5 means that it’s now possible to play video on the web without having to install plugins. However, not all browsers support the same video formats. The Media Source Extensions allows JavaScript applications to generate media streams dynamically for playback. 

---

## How do I add streams dynamically to Peer5 from my web application?
You can simply add streams with CORS enabled to one of the Peer5 supported players and add Peer5 api javascript into the player code. That's all. 

---

Need extra help? <a href="javascript:Intercom('show')">Chat with us</a> or [send us an email](mailto:info@peer5.com)

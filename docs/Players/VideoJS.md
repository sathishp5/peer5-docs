# Boosting VideoJS with Peer5

<br>
![](./images/videojs.png)
<br><br>
[VideoJS](http://videojs.com/) is a free and open source HTML5 video player that supports HLS.

The integration with Peer5 plugin is as easy as it can get.

In addition to the player script, include Peer5 client and VideoJS plugin.
 
Peer5 client and plugins scripts

     <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
     <script src="//api.peer5.com/peer5.videojs5.hlsjs.plugin.js">
    
**Complete Example** 
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.m3u8` file
  
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>VideoJS Player test</title>
        <!-- peer5 client library -->
        <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
        
        <!-- videojs5 scripts - You can change to your self hosted scripts -->
        <link rel="stylesheet" href="//api.peer5.com/videojs5/assets/video-js.min.css">
        <script src="//api.peer5.com/videojs5.js"></script>
        
        <!-- peer5 plugin for videojs5 -->
        <script src="//api.peer5.com/peer5.videojs5.hlsjs.plugin.js">
    </head>
    <body>
        <video id="player" class="video-js vjs-default-skin" height="360" width="640" controls preload="none">
            <source src="MANIFEST_FILE" type="application/x-mpegURL" />
        </video>
        <script>
            var player = videojs('#player');
        </script>
    </body>
    </html>
```
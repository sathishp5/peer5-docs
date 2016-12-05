# Shaka Player with Peer5 for DASH integration

![](./images/shaka-player.png)

[Shaka Player](https://github.com/google/shaka-player) is a free and open source HTML5 video player that supports playing [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) without any plugin.

The integration with Peer5 plugin is as easy as it can get.
 
## Peer5 client and player scripts

    <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
    <script src="//api.peer5.com/peer5.shakaplayer.plugin.js"></script>
    
## Complete Example 
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.mpd` file
  
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Peer5 Demo</title>
    <!-- peer5 client & plugin-->
    <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
    <script src="//api.peer5.com/peer5.shakaplayer.plugin.js"></script>
    <!-- shakaplayer script-->
    <script src="//cdnjs.cloudflare.com/ajax/libs/shaka-player/2.0.1/shaka-player.compiled.js"></script>
  </head>
  <body>
    <video id="video" width="640" controls autoplay></video>
    <script>
      var video = document.getElementById('video');
      window.shaka.polyfill.installAll();

      if (!shaka.Player.isBrowserSupported()) {
          parentEl.innerHTML = 'Shaka player is not supported in this browser';
      }
      else {
          var player = new shaka.Player(video);
          player.load("MANIFEST_FILE");
      }
    </script>
  </body>
</html>
```

visit [here](https://github.com/google/shaka-player) for the full Shaka Player docs
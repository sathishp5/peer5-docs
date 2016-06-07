# Shaka Player with Peer5 for DASH integration

![](./images/shaka-player.png)

[Shaka Player](https://github.com/google/shaka-player) is a free and open source HTML5 video player that supports playing [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) without any plugin.

The integration with Peer5 plugin is as easy as it can get.
 
## Peer5 client and player scripts

    <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
    <script src="//api.peer5.com/peer5.shakaplayer.js"></script>
    
## Complete Example 
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.mpd` file
  
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Shaka Player Player test</title>
        <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
        <script src="//api.peer5.com/peer5.shakaplayer.js"></script>
    </head>
    <body>
        <video id="player" width="640" height="360" crossorigin="anonymous" controls autoplay>
            Your browser does not support HTML5 video.
        </video>
        <script>
            var stream = 'MANIFEST_FILE';
            shaka.polyfill.installAll();
            var videoEl = document.getElementById('player');
            var player = new shaka.player.Player(videoEl);
            var estimator = new shaka.util.EWMABandwidthEstimator();
            var source = new shaka.player.DashVideoSource(stream, null, estimator);
            player.load(source);
        </script>
    </body>
    </html>
```

visit [here](https://github.com/google/shaka-player) for the full Shaka Player docs
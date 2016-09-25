## Flowplayer with Peer5 for HLS integration
<br>
![](./images/flowplayer.png)
<br><br>
[Flowplayer](http://flowplayer.org/) is a feature rich video player that supports HLS.

The integration with Peer5 plugin is easy and involves two lines of code.
In addition to the player script, include Peer5 client and the flowplayer plugin.
 
## Peer5 client and plugins scripts
add these two scripts to the `head` of your player's page

    <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
    <script src="//api.peer5.com/peer5.flowplayer.plugin.js"></script>

### Complete Example
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.m3u8` file
  
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Flowplayer Player test</title>
        <!-- peer5 client & plugin -->
        <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
        <script src="//api.peer5.com/peer5.flowplayer.plugin.js"></script>

        <!-- Flowplayer scripts & styles -->
        <script src="//releases.flowplayer.org/6.0.5/flowplayer.min.js"></script>
        <link href="//releases.flowplayer.org/6.0.5/skin/minimalist.css" rel="stylesheet" />
    </head>
    <body>
        <div id="player"></div>
        <script>
            var player = flowplayer("#player", {
                clip: {
                    sources: [{
                        type: "application/x-mpegurl",
                        src:  "MANIFEST_FILE"
                    }]
                }
            });
        </script>
    </body>
    </html>
```

visit [here](https://flowplayer.org/docs/api.html) for the full Flowplayer docs 
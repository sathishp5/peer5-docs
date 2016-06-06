# JWPlayer 7 with Peer5 for HLS integration

![](./images/jwplayer.jpg)

[JWPlayer 7](https://www.jwplayer.com/) is a feature rich video player that supports HLS.

The integration with Peer5 plugin is as easy as it can get.

In addition to the player script, include Peer5 client and JWPlayer plugin.
 
## Peer5 client and plugins scripts

     <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
     <script src="//api.peer5.com/peer5.jwplayer7.plugin.js"></script>
    
## Complete Example 
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `JWPLAYER_KEY`  &nbsp;&nbsp;&nbsp;&nbsp;your [JWPlayer API key](https://dashboard.jwplayer.com/#/account/properties)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.m3u8` file
  
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>JW7 Player test</title>
        <!-- peer5 client library -->
        <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
        
        <!-- jwplayer7 - You can change to your self hosted script -->
        <script src="//api.peer5.com/jwplayer7.js"></script>
        
        <!-- peer5 plugin for jwplayer7 -->
        <script src="//api.peer5.com/peer5.jwplayer7.plugin.js"></script>
        <!-- jwplayer7 license key -->
        <script>jwplayer.key = 'JWPLAYER_KEY';</script>
    </head>
    <body>
        <div id="player"></div>
        <script>
            var player = jwplayer('player').setup({
                playlist: [{
                    file: 'MANIFEST_FILE',
                    provider: '//api.peer5.com/jwplayer7/assets/flashls.provider.swf'
                }]
            });
        </script>
    </body>
    </html>
```

visit [here](https://developer.jwplayer.com/jw-player/) for the full JWPlayer docs
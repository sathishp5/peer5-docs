# Boosting Clappr with Peer5
<br>
![](./images/clappr.png)
<br><br>
[Clappr](http://clappr.io/) is a free and open source HTML5 video player that supports HLS.

The integration with Peer5 plugin is as easy as it can get.

In addition to the player script, include Peer5 client and Clappr plugin.
 
Peer5 client and plugins scripts

     <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
     <script src="//api.peer5.com/clappr.js"></script>
    
**Complete Example** 
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.m3u8` file
  
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Clappr Player test</title>
        <!-- peer5 client library -->
        <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
        
        <!-- clappr - can be changed to your self hosted script -->
        <script src="//api.peer5.com/clappr.js"></script>
        
        <!-- peer5 plugin for clappr -->
        <script src="//api.peer5.com/peer5.clappr.plugin.js"></script>
    </head>
    <body>
        <div id="player"></div>
        <script>
            var player = new Clappr.Player({
                source: 'MANIFEST_FILE',
                parentId: '#player'
            });
        </script>
    </body>
    </html>
```
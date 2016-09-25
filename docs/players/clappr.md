# Clappr with Peer5 for HLS integration
<br>
![](./images/clappr.png)
<br><br>
[Clappr](http://clappr.io/) is a free and open source HTML5 video player that supports HLS.

The integration with Peer5 plugin is easy and involves two lines of code.
In addition to the player script, include Peer5 client and the Clappr plugin.
 
## Peer5 client and plugins scripts
add these two scripts to the `head` of your player's page

     <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
     <script src="//api.peer5.com/peer5.clappr.plugin.js"></script>

## Complete Example
 
The following information needs to be filled according to your actual data:
 
- `PEER5_API_KEY` &nbsp;&nbsp;your [Peer5 API key](https://app.peer5.com/integration)
- `MANIFEST_FILE` &nbsp;&nbsp;url to your `.m3u8` file
  
```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Clappr Player test</title>
        <!-- peer5 client & plugin -->
        <script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
        <script src="//api.peer5.com/peer5.clappr.plugin.js"></script>
        
        <!-- clappr script -->
        <script src="//cdn.clappr.io/latest/clappr.js"></script>
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

visit [here](https://github.com/clappr/clappr) for the full Clappr docs 
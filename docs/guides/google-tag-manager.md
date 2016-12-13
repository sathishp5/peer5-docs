# Google Tag Manager

If you use Google Tag Manager (GTM) follow one of these methods to integrate Peer5 correctly.

## Background

Since scripts are loaded asynchronously its important to initiate player only after peer5 scripts have been loaded.
jwplayer is used through out the examples, but the methods apply to all players

## 1. Create player in external script

player.js
```
jwplayer.key = 'JWPLAYER_KEY';

var player = jwplayer('player').setup({
    file: 'MANIFEST_FILE'
});
```


Custom HTML tag (in GTM dashboard)
```
<script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
<script src="//api.peer5.com/peer5.jwplayer7.plugin.js"></script>
<script src="//ssl.p.jwpcdn.com/player/v/7.8.1/jwplayer.js"></script>
<script src="/player.js"></script>
```

## 2. Initiate player inline

index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JW7 Player test</title>

    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer','GTM-XXXXX');</script>
    <!-- End Google Tag Manager -->
</head>
<body>
    <div id="player"></div>
    <script>
        // create global function that will be called by the tag manager script
        window.onScriptsReady = function () {
            jwplayer.key = 'JWPLAYER_KEY';
            var player = jwplayer('player').setup({
                file: 'MANIFEST_FILE'
            });
        }
    </script>
</body>
</html>
```


Custom HTML tag (in GTM dashboard)
```
<script src="//api.peer5.com/peer5.js?id=PEER5_API_KEY"></script>
<script src="//api.peer5.com/peer5.jwplayer7.plugin.js"></script>
<script src="//ssl.p.jwpcdn.com/player/v/7.8.1/jwplayer.js"></script>
<script>
    // scripts are loaded - safe to init player
    if (window.onScriptsReady) {
        window.onScriptsReady()
    }
</script>
```

# Migrate From Flash

Starting version 55 of Chrome (release in Dec 2016) browser flash is becoming an opt-in plugin meaning that user will need to explicitly activate flash plugin.
If you are using flash based HLS media player its a good idea to migrate to HTML5 based one.
This guide will explain the easiest solutions for most of the common players.



## Do I Use Flash?

If you are using RTMP you are using Flash.

If you are using HLS/DASH read ahead
To check if you are using flash and if flash is the default playback method follow these steps.

1. Navigate to your page that contains the video player and start playing
2. Right click on the playing video area

if you see something like `Flash Version x.x.x` or `Adobe x.x.x`

if you see a regular browser menu and you can 'inspect element' in chrome that means you are using HTML5 

## HLS Migration Table

players compatibility with HLS playback

Player              | Flash playback    | Flash Is Default? | HTML5 playback    | need migration?   | More information
:-:                 | :-:               | :-:               | :-:               | :-:               | :--
Clappr              | Yes(Plugin)       | no                | Yes               | **no**            | [Clappr Formats](https://github.com/clappr/clappr/blob/master/doc/SUPPORTED_FORMATS.md)
JWPlayer v6.x       | Yes               | Yes               | No                | **yes**           | Upgrade to JWPlayer v7
JWPlayer v7.x       | Yes               | Yes               | Yes               | **maybe**         | see [JWPlayer7 tuning](#jwplayer7-tuning)
VideoJS v4          | Yes               | Yes               | No                | **yes**           | Upgrade to VideoJS5
VideoJS v5          | Yes               | no                | Yes               | **no**            | see [VideoJS5 tuning](#videojs5-tuning)
Flowplayer(html5)   | Yes               | Yes               | Yes(plugin)       | **yes**           | see [Flowplayer tuning](#flowplayer-tuning)
Kaltura             | Yes               | no                | Yes(plugin)       | **yes**           | see [Kaltua tuning](#kaltura-tuning)
MediaElement.js     | Yes               | Yes               | No                | **yes**           | html5 supported in yet-released [v3](https://github.com/johndyer/mediaelement/tree/3.x-dev)


### JWPlayer7 Tuning

Starting v7.4.2 its possible to enable html5 rendering for HLS - hence v7.4.2 is the minimal version for html5 HLS playback.

To tune JWPlayer v7 to use html5 by default add the following configs to your player initiation

```
{
  primary: 'html5',
  hlshtml: true
}
```

### VideoJS5 Tuning

VideoJS v5 needs a plugin to play hls, if you are using either one of these - then you are already using html5 based playback
- https://github.com/Peer5/videojs-contrib-hls
- https://github.com/videojs/videojs-contrib-hls

### Flowplayer Tuning

Flowplayer v6 supports HLS playback using flash or through native HLS support such as in safari.

To use Flowplayer with html5 HLS playback you must add the [hlsjs plugin](https://flowplayer.org/docs/plugins.html#hlsjs) or to use [Peer5](/players/flowplayer/)

### Kaltura Tuning

Html5 HLS playback in kaltura is supported through [hlsjs plugin](https://github.com/kaltura/mwEmbed/tree/master/modules/Hlsjs).
The [integration with peer5](/players/kaltura/) enables the hlsjs plugin as well.

To use Flowplayer with html5 HLS playback you must add the [hlsjs plugin](https://flowplayer.org/docs/plugins.html#hlsjs) or to use [Peer5](/players/flowplayer/)

## RTMP Migration

Since RTMP is a proprietary protocol of Adobe its not possible to play RTMP without flash plugin.
There are some attempts to replace RTMP with WebRTC as described [here](http://stackoverflow.com/questions/37457972/low-latency-2s-live-video-streaming-html5-solutions)
yet not mature solution has been displayed yet.
The most viable solution to replace RTMP streaming is to use HLS.

HLS is supported in many media servers - [read more](/faq/#how-do-i-setup-an-hlsdash-stream)



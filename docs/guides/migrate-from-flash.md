# Migrate From Flash To An HTML5 Media Player

Starting with version 55 of Chrome (expected release in Dec 2016), Flash will be blocked by default and users will have to explicitly activate the Flash plugin via an opt-in mechanism. If you are using a Flash-based HLS media player, now is a good time to migrate to an HTML5-based one. This guide will explain the easiest migration path for the most popular players.

## Do I Use Flash?

If you are using RTMP, you are using Flash.

If you are using HLS, follow these steps to check if you're using Flash.

1. Navigate to your page that contains the video player and start playing
2. Right click anywhere on the video player

If you see something like `Flash Version x.x.x` or `Adobe x.x.x` it means you are using Flash.

If you see a regular browser menu and you can 'inspect element' in Chrome that means you are using HTML5. 

## HLS Migration Table

A word of caution - even if your player supports HTML5, you should actually test that your stream works with your player's HTML5 implementation.
Check it by [disabling flash in your browser](http://www.howtogeek.com/222275/how-to-uninstall-and-disable-flash-in-every-web-browser/)

Player              | Flash playback    | Flash Is Default? | HTML5 playback    | need migration?   | More information
:-:                 | :-:               | :-:               | :-:               | :-:               | :--
Clappr              | Yes(Plugin)       | no                | Yes               | **no**            | [Clappr Formats](https://github.com/clappr/clappr/blob/master/doc/SUPPORTED_FORMATS.md)
JWPlayer v6.x       | Yes               | Yes               | No                | **yes**           | Upgrade to JWPlayer v7
JWPlayer v7.x       | Yes               | Yes               | Yes               | **maybe**         | see [JWPlayer7 tuning](#jwplayer7-configuration)
VideoJS v4          | Yes               | Yes               | No                | **yes**           | Upgrade to VideoJS5
VideoJS v5          | Yes               | no                | Yes               | **no**            | see [VideoJS5 tuning](#videojs5-configuration)
Flowplayer(html5)   | Yes               | Yes               | Yes(plugin)       | **yes**           | see [Flowplayer tuning](#flowplayer-configuration)
Kaltura             | Yes               | no                | Yes               | **no**            |
MediaElement.js     | Yes               | Yes               | No                | **yes**           | html5 supported in not yet released [v3](https://github.com/johndyer/mediaelement/tree/3.x-dev)


### JWPlayer7 Configuration

Starting with v7.4.2 its possible to enable HTML5 rendering for HLS - hence v7.4.2 is the minimal version necessary for HTML5 HLS playback.
To configure JWPlayer v7 to use HTML5 by default, add the following config tags to your player initiation

```
{
  primary: 'html5',
  hlshtml: true
}
```

### VideoJS5 Configuration

VideoJS v5 needs a plugin to play HLS. If you are using a plugin such as the one below, then you are already using HTML5-based playback

- [https://github.com/Peer5/videojs-contrib-hls.js](https://github.com/Peer5/videojs-contrib-hls.js)

### Flowplayer Configuration

Flowplayer v6 supports HLS playback using Flash or through native HLS support such as in Safari.
To use Flowplayer with HTML5 HLS playback you must add the [hlsjs plugin](https://flowplayer.org/docs/plugins.html#hlsjs) or use [Peer5](/players/flowplayer/)

## RTMP Migration

Since RTMP is a proprietary protocol of Adobe, it is not possible to play RTMP without the Flash plugin.
There are some attempts to replace RTMP with WebRTC as described [here](http://stackoverflow.com/questions/37457972/low-latency-2s-live-video-streaming-html5-solutions)
but no mature solution has been presented yet.
The most viable solution right now is to replace RTMP with HLS.

HLS is supported in many media servers - [read more](/faq/#how-do-i-setup-an-hlsdash-stream)

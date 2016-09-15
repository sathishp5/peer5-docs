# Stats API

Peer5 provides live statistics through a javascript SDK.
Using this API you'll be able to receive various metrics relating to the performance of the p2p network, and the video playback.  
This API is especially usable if you'd like to merge peer5's analytics into your own analytics flow.

```javascript
/**
* 
* @returns {{
  load.end: {number[]},
  rebuffer.end: {number[]},
  seek.end: {number[]},
  numOfPeers: {number},
  totalHttpDownloaded: {number},
  totalP2PDownloaded: {number}
}}
*/
peer5.getStats();
```

## Video loading time

The amount of time a video took to start playing first frame since the player initiated playback logic.

```javascript
/**
*
* @returns {number[]} - array of durations in miliseconds
*/
peer5.getStats()['load.end'];
```

Usually the array will have 1 item. But if there were several videos loading at the same page it can be higher than 1.

## Video rebuffering

Get the duration of rebuffer events that took place in the session. 

```javascript
/**
*
* @returns {number[]} - array of durations in miliseconds
*/
peer5.getStats()['rebuffer.end'];
```

Each element in the array represents the duration in milliseconds of a rebuffer event that has ended.

## Video seeks

Get the duration of seek events that took place in the session.

```javascript
/**
*
* @returns {number[]} - array of durations in miliseconds
*/
peer5.getStats()['seek.end']
```

Each element in the array represens the duration in milliseconds that a seek event lasted.

## Number of peers

Get the number of peers currently connected to the client.
 
```javascript
/**
*
* @returns {Number}
*/
peer5.getStats().numOfPeers;
```

Note: this metric is not aggregated, but represents the current state.

## HTTP data

Get the total amount of bytes downloaded in http so far in the session.

```javascript
/**
*
* @returns {Number} - bytes
*/
peer5.getStats().totalHttpDownloaded
```

## P2P data

Get the total amount of bytes downloaded in p2p so far in the session.

```javascript
/**
*
* @returns {Number} - bytes
*/
peer5.getStats().totalP2PDownloaded
```

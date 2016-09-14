# Stats API

Peer5 enables monitoring statistics from the client side, using this API.
You'll be able to receive various metrics relating to the performance of the p2p network, and the video playback.  
This API is especially usable if you'd like to merge peer5's analytics into your own analytics flow.

```javascript
/**
* 
* @returns {{
  load.end:{array},
  rebuffer.end:{array},
  seek.end:{array},
  numOfPeers:{Number},
  totalHttpDownloaded:{Number},
  totalP2PDownloaded: {Number}
}}
*/
peer5.getStats();
```

## Video loading time

The amount of time a video took to start playing first frame since the player was ready.

```javascript
/**
*
* @returns {array} of Number of miliseconds
*/
peer5.getStats()['load.end'];
```

Usually the array will be of length 1, if there were several videos loading at the same page it can be higher than 1.

## Video rebufferings

Get the duration of rebuffer events that took place in the session. 

```javascript
/**
*
* @returns {array} of Number of miliseconds
*/
peer5.getStats()['rebuffer.end'];
```

Each element in the array represents the duration in milliseconds of a rebuffer event that has ended.

## Video seeks

Get the duration of seeks events that took place in the session.

```javascript
/**
*
* @returns {array} of Number Of miliseconds
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

Note: this metric is not aggregative, but represents the current state.

## HTTP data

Get the total amount of bytes downloaded in http so far in the session.

```javascript
/**
*
* @returns {Number}
*/
peer5.getStats().totalHttpDownloaded
```

## P2P data

Get the total amount of bytes downloaded in p2p so far in the session.

```javascript
/**
*
* @returns {Number}
*/
peer5.getStats().totalP2PDownloaded
```

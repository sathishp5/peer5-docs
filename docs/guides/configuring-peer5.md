## Configuring Peer5
 
Peer5 has a javascript API for configuring different parameters of the library.

### Disabling P2P

If, for some reason, you want to disable P2P requests for a given session, you can do it with the following code:

```javascript
peer5.configure({
  p2p: false
});
```

Any requests made using `peer5.Request` after disabling P2P will use native XMLHttpRequest.

### Enabling P2P

When you want to enable P2P requests you can easily do it with the following code:

```javascript
peer5.configure({
  p2p: true
});
```

Any requests made using `peer5.Request` after enabling P2P will fully utilize the Peer5 mesh network.
## Configuring Peer5
 
Peer5 has a client-side Javascript API for configuring different parameters of the library.

### Disabling P2P

When Peer5 is loaded, it has P2P enabled by default.
If, for some reason, you want to disable P2P requests for a given user session, you can do it with the following code:

```javascript
peer5.configure({
  p2p: false
});
```

Any requests made using `peer5.Request` after disabling P2P will use XMLHttpRequest to fetch the file from the HTTP server.

### Enabling P2P

To re-enable P2P requests, you need to call configure with `{p2p:true}`:

```javascript
peer5.configure({
  p2p: true
});
```

Any requests made using `peer5.Request` after enabling P2P will fully utilize the Peer5 mesh network.

# Prism Client

Connect to a [Prism Server](https://github.com/socketstream/prism) from the browser or a remote Node app.


## Example

```js
var prismClient = require('prism-client');
var rtt = require('rtt-engineio')();

var transport = rtt.client({port: 3001, host: 'localhost'});
var app = prismClient();

app.connect(function(err, info){

  console.log("Connected to Prism Server %s with Session ID %s", info.version, info.sessionId);
  
  app.discover(function(){
    console.log('Available services are', app.api);
  });

});
```

## API

[View API](/API.md)


## Connecting from another Node process

Please see the REPL client example in `examples/repl.js`.

Note: Not all transports support Node.js clients. `rtt-engineio` and `rtt-ws` do.


## Connecting from a PhoneGap app

I've not tried this yet, but in theory it should be possible. You may need to use [PersistJS](https://github.com/jeremydurham/persist-js). Please get in touch to let me know how you get on.


## Providing Services

A [Prism Server](https://github.com/socketstream/prism) will be running one or more [Realtime Services](https://github.com/socketstream/realtime-service). Each Service will probably (but not necessarily) contain code to be run on the client (accessible via the `app.api` object).

Client's can either be told what services are available in advance (using `app.provide()`), or discover them upon connection (using `app.discover()`).

We recommending providing service info in advance where possible as this minimizes the traffic over the WebSocket. It also allows for the client-side code to be minimized and served by CDNs.

Prism Server can make this easy for you by building a custom client module containing all the code you need for the transport and services. Just call `server.buildClient();`.

However it's not always possible to know what Services are provided by a server until you connect. For example, you may want to run a query over the REPL against a remote server. In this case, you'll need to call `app.discover()` upon initial connection.


## License

MIT
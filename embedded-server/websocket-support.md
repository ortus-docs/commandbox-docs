---
metaLinks:
  alternates:
    - >-
      https://app.gitbook.com/s/Bw6M3PI3e5HgLVcKZz0G/embedded-server/websocket-support
---

# WebSocket Support

CommandBox supports WebSockets natively via our [SocketBox library](https://forgebox.io/view/socketbox).  The WebSocket server in CommandBox is not really a separate "server" per se, since it’s on the same port. It’s just an upgrade listener which will upgrade any WS requests.

This websocket integration will work for Lucee, Adobe, and BoxLang alike as it passes incoming messages to the app via an "internal" HTTP request to `/WebSocket.cfc?method=onProcess` where the CF/BL code can handle it. The incoming request will have all cookies, headers, hostname, etc that the original websocket connection was started with, so normal CGI variables and session scopes should work fine.

You need to create a custom `/WebSocket.cfc` class should extend one of the classes described below in this library which provides the base functionality.

## Configure

CommandBox allows for the following config:

* Whether or not the websocket handler is active. Toggle for all sites under `web.websocket.enable` and for an individual site under `sites.mySite.websocket.enable`. `true`/`false`
* The URI the websockets listener will listen on. Default is `/ws` so if your site is `foobar.com` then your JS code in the browser will connect to `foobar.com/ws`. Configure under `web.websocket.uri` for all sites and for an individual site under `sites.mySite.websocket.uri`. You can use the root URI of `/` but you may have issues setting up websocket proxying if you have a web server in front.
* The publicly accessible remote class (cfc or bx) to respond to incoming websocket messages. Default is `/WebSocket.cfc` in the web root, Configure for all sites with `web.websocket.listener` and for an individual site under `sites.mySite.websocket.listener`.

## Using SocketBox

SocketBox has two usage modes. Out of the box, there is just basic websocket support that is very simple for you to build whatever you want on top of.\
There is also a STOMP broker built on top of the core functionality you can tap into by extending a different CFC.

### WebSocket Core

Core WebSocket support is VERY simple. It has:

* low level connect/disconnect management
* incoming message handling
* abiliy to send a message to one or all connected clients

Core WebSocket support does NOT provide

* authentication
* authorization
* message routing semantics
* topic subscriptions, etc
* conventions for what is inside of a message (they're all just plain text)

To use core WebSocket support, you'll want to extend the `modules.socketbox.models.WebSocketCore` class.

#### Usage

Methods you can override in your custom `/WebSocket.cfc` are:

* `onConnect( required channel )` - called for every new remote connection
* `onClose( required channel )` - called every time a connection is closed
* `onMessage( required message, required channel )` - called every time an incoming message is received. The message will be a string.

Other methods inherited from the the `WebSocketCore.cfc` you can use.

* `sendMessage( required message, required channel )` - Send a string message to a specific channel
* `broadcastMessage( required message )` - Send a string message to every connected channel
* `getAllConnections()` - Returns an array of all channels representing every remote connection to the server.

None of these methods return any values. If you, for instance, want to reply back to a websocket message with another message, you can re-use the channel like so:

```js
function onMessage( required message, required channel ) {
    if( message EQ "Ping" ) {
        sendMessage( "Pong", arguments.channel );
    }
}
```

You can build whatever you want on top of this basic functionality, but it's up to you to decide what each message will contain, how it should be parsed, and to track your own list of channels via the onConnect() and onClose() methods if you wish.

#### Browser/Client code example

You can use a JS WebSocket library, but modern browsers have everything you need baked in. This is what some JS code could look like that connects to a simple websocket.

```html
<script language="javascript">

    // Use wss:// for HTTPS
    socket = new WebSocket('ws://localhost/ws');

    socket.onopen = function() {
        console.log('Connected to WebSocket server');
        socket.send("Hello from the browser!");
    };

    socket.onmessage = function(event) {
        console.log('Message from server:', event.data);
    };

    socket.onclose = function(e) {
        console.log('Socket is closed.');
    };

    socket.onerror = function(err) {
        console.error('Socket encountered error: ', err.message );
    };

</script>
```

### STOMP Broker

If you want more power, you can use our STOMP broker. STOMP stands for Simple Text Oriented Messaging Protocol and provides semantics for

* message format/serialization
* authentication/authorizing
* topics and message routing
* subscriptions to a topic
* sending messages to a destination
* automatic reconnect
* heartbeat messages to detect dead connections

To use STOMP WebSocket support, you'll want to extend the `modules.socketbox.models.WebSocketSTOMP` class. This class provides all the same functionality as the `WebSocketCore` class, but with much more.

#### Usage

It adds the following methods you can override in your custom `WebSocket.cfc`.

* `configure()` - You can configure the STOMP broker via a struct returned from this method (see below)
* `authenticate( required string login, required string passcode, string host, required channel )` - Allows custom authentication logic to accept or deny a STOMP connect request. Note a STOMP connect message is different from a low level WebSocket connection.
* `authorize( required string login, required string exchange, required string destination, required string access, required channel )` - Allows custom authorization logic to allow or deny a client from being able to subscribe to a destination or publish to a destination.
* `onSTOMPConnect( required message, required channel )` - Called when a STOMP client connects. Typically you don't need to override this
* `onSTOMPDisconnect( required message, required channel )` - Called when a STOMP client disconnects. Typically you don't need to override this

Additionally, the following public methods are available to you to call anywhere in your app. Simply create an instance of your WebSocket class and call them.

* `send( required string destination, required any messageData, struct headers={} )` - Send a STOMP message to all subscribers from server side code.
* `getSubscriptions()` - Get all current STOMP subscriptions, which includes browser clients and server side listeners. (You probably only need this if writing a custom exchange or debugging)
* `getExchanges()` - Get all configured exchanges (you probably only need this for debugging)
* `getSTOMPConnections()` - Get all STOMP connections. (you probably only need this for debugging)
* `getConfig()` - Get the current config for the STOMP broker.
* `getConnectionDetails( required channel )` - Get details for a given channel

Most of those methods you'll never use directly. The STOMP broker will generally automatically manage all connections, subscriptions, exchanges, and routing. All you need to do is configure any server-side listeners and exchanges/routing and then let it get to work.

#### Message Object

In STOMP, all communications between client and server are in the form of a message, which has 3 parts

* `command` - The purpose of the message
* `body` - This can be a string, or will be auto serialized to JSON if it's a complex object.
* `headers` - A struct of metadata describing the message

You shouldn't need to create message objects directly, but your server side listeners will receive a message object. Methods you can call are:

* `getCommand()` - Will most likely always be "message" for your server side listeners
* `getBody()` - will auto-deserialize JSON back into complex objects when the `content-type` header is `application/json`.
* `getHeaders()` - Get the full struct of headers
* `getHeader( required string key, string defaultValue)` - Get a single header, with an optional default value
* `getBodyRaw()` - Returns the body contents as a string if you don't want it JSON deserialized.
* `getChannel()` - The channel which sent the original message. THIS WILL BE NULL if sent from server-side code!

#### Configuration

Configure the STOMP broker by supplying a `configure()` method in your `WebSocket.cfc` that returns a struct like so:

```js
/**
 * Here we configure our exchanges, listeners, and settings.
 * This method will only be run once when the server starts UNLESS `debugMode` is set to `true`.
 */
function configure() {
    return {
        // Set to try (dynamically if you wish) to reload the config on EVERY request (for development only!)
        "debugMode" : false,
        // How often to send a heartbeat to the client (in milliseconds)
        "heartBeatMS" : 10000,
        // Here we configure the exchanges and their bindings which will process incoming messages.
        // Remember, all messages are send first to an exchange for routing.
        // The DirectExchange will be configured automatically even if we don't include it below.  It's default use require on special config.
        "exchanges" : {
			 "direct" : {
				 "bindings" : {
					 "destination1" : "destination2",
					"destination3" : "/topic/foo.bar"
				 }
			 },
			 "topic" : {
				 "bindings" : {
					"myTopic.brad.##" : "destination1",
					"anotherTopic.*" : "fanout/myFanout"
				}
			},
			"fanout" : {
				"bindings" : {
					"myFanout" : [
						"destination1",
						"direct/destination2"
					],
					"anotherFanout" : [
						"destination3",
						"topic/destination4"
					]
				}
			},
			"distribution" : {
				"type" : "random", // roundrobin
				"bindings" : {
					"myDistribution" : [
						"destination1",
						"direct/destination2"
					]
				}
			}
        },
        // Our Websocket clients can create subscriptions to destinations, but we can also define server-side listeners
        "subscriptions" : {           
			"destination1" : (message)=>{
				logMessage( message.getBody() );
			},
			"destination2" : ()=>{}
        }
    };
}
```

All configuration is optional and can be ommitted if not in use. In fact, the entire `configure()` method is optional if you just want to have a direct exchange and no server-side listeners.

If `debugMode` is set to true, the entire config will be reloaded every request and additional logging to the console will be enabled.

The `heartBeatMS` setting controls the number of ms between heartbeat "pings" from every client websocket connection. Set to `0` to disable this. Heartbeats are useful so clients can reconnect right away if the connection is interrupted, but can create a lot of network chatter if you have many connections.

**Exchanges**

You can configure exchanges as shown above. The direct exchange will always be configured by default and you only need to specify it if you want to pass custom bindings to it. Exchanges receive the incoming messages and decide what destinations, if any, to route them to. An exchange could route a message to 0 destinations or 1000 destinations. And each destination could have 0 subscribers or 1000 subscribers. The exchange abstracts all that away from you. Publishers simply send messages to an exchange wihtout caring who is subscribed, and subscribers simply receive messages from their subscriptions without caring who sent it.

Here are the exchange types and their config.

* `direct` - Routes message directly to the destination matching their destination header.
  * `bindings` - configure custom bindings between a destination and another destination.
* `topic` - Routes messages to destinations via wildcard subscriptions allowing partial match semantics on destinations named `foo.bar.baz`.
  * `bindings` - Configure destinations using `*` and `#` as wildcards to match a single segment or many segments respectively
* `fanout` - Routes message to ALL bindings at the same time
  * `bindings` - Matches each incoming destination to an array of outgoing destinations
* `distribution` - Routes messages to a SINGLE outgoing destination based on a selection algorithm
  * `type` - The value `random` or `roundrobin`.
  * `bindings` - Same as fanout, but only one destination will be chosen at a time
* `<path.to.custom.CFC>` - Provide the mapping path to a CFC that extends `modules.socketbox.models.STOMP.exchange.BaseExchange` and implement your own `routeMessage( required WebSocketSTOMP STOMPBroker, required string destination, required any message )`. You can decide whatever logic you want to find the matching subscriptions or STOMP connections you wish to send to.
  * Config is entirely chosen by you! Whatever struct of properties you declare in the config will be passed to your CFC's `init()` method. Acess any property inside the exchange via the `getProperty( required string key, any defaultValue )` method.

#### Server Side listners

If you want to listen to messages with server-side code, specify as many `subscriptions` in the config, which consist of a function that will be executed every time a message is routed to your listener. Your function will receive the `message` as its only argument and you can run whatever code you wish. Get the original channel that send the message via `message.getChannel()`, but note, this will be null if the message was send from server-side code and not a WebSocket client.

#### Browser/Client code example

Our STOMP broker should be compatible with any STOMP client, but we've been testing with this one: https://www.npmjs.com/package/@stomp/stompjs

You can create a simple STOMP connection like so:

```html
<script type="importmap">
  {
    "imports": {
      "@stomp/stompjs": "https://ga.jspm.io/npm:@stomp/stompjs@7.0.0/esm6/index.js"
    }
  }
</script>
<script type="module">
    import { Client } from '@stomp/stompjs';
      
    client = new Client({
      // Use wss:// for HTTPS
      brokerURL: 'ws://localhost/ws',
      reconnectDelay: 5000,  // Optional: reconnect after 5 seconds
      heartbeatIncoming: 10000,
      heartbeatOutgoing: 10000,
      connectHeaders: {
        login: 'myuser',
        passcode: 'mypass'
      },
      onConnect: (frame) => {
        console.log( 'Successfully connected to STOMP broker!' );
        
        // We can subscribe to a destination now
        mySubscription = client.subscribe('my-destination', function( message ) {
          // If the content-type message header is application/json, it's up to you to deserialize here with JSON.parse()
          console.log( 'Message received', message.body );
        });

        // unsubscribe like so:
        // mySubscription.unsubscribe();

      },
      onStompError: error => {
        console.log("STOMP error: " + error.headers.message);
      },
      onWebSocketClose: () => {
        console.log("WebSocket connection closed");
      },
      // This puts debugging info in the console.  Remove it in production.
      debug: function (str) {
        console.log(str);
      }
    });
  
    // Activate the client
    client.activate();

</script>
```

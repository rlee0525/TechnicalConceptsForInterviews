WebSockets
===================

Simply put, WebSocket is a full-duplex (simultaneous two-way or bi-directional) communications protocol over a single TCP connection.  This is especially great if we want an web application to update some information without having the user prompt for it.  Applications to this are endless and typically include chat apps, stock tickers, news, and any type of live reports.  But before we start to delve into how it works, let's contrast it to some alternatives.

What it is not (other communication strategies)
-------------
#### Polling
Polling sends regular HTTP requests which are met with immediate responses.  The regular interval poses a problem at times when connections are needlessly opened or when connections aren't opened often enough.

#### Long-Polling

Long-Polling sends a similar request to the server and keeps the request open for a set period of time.  If the server has a response in time before the request is set to close, the message is included in the response, and the process is repeated.  

If not, the server sends a request to terminate the request.  While this may eliminate unneeded requests at set intervals, it could, at its erroneous worst, open too many requests in continuous loops.

#### Streaming
Streaming sends a complete HTTP request that is kept open by the server for a set amount of time.  Any response from the server is updated to include new messages the server has, but the server never terminates or signals to complete the response, keeping the connection open.  As with the previous alternatives, this type of streaming is still encapsulated in HTTP and brings some of its baggage.  

HTTP is often buffered when routed over the internet, increasing latency.  Furthermore, HTTP requests and responses, contain additional header data (such as cookies) which provide too much overhead for this process to scale.  Finally, coordination of two such alternatives are usually needed to provided true full-duplex communication (one up, one down).

HTML 5 WebSocket
-------------
The HTML5 WebSocket specification defines the standard for the web today.  Currently, it is compatible with nearly 94% of web browsers in use today[^caniuse].  

WebSocket supports simultaneous communication, using a single connection, allowing servers to support more concurrent connections.

The WebSocket connection starts with a familiar HTTP request, but includes an **upgrade** request to initiate the WebSocket connection.

    GET /chat HTTP/1.1
    Host: example.com:8000
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Sec-WebSocket-Version: 13

After the handshake is complete it uses a TCP connection over any specified port.  

Much like http and https, WebSocket protocol specification defines **ws** and **wss** as its uniform resource identifier (URI) schemes.


Libraries
-------------
There are a number of libraries that support the WebSocket specification to get you started.  

Note that [Rails 5 comes with Action Cable](http://edgeguides.rubyonrails.org/action_cable_overview.html) that integrates WebSocket!

 - [Node.JS: ws](https://github.com/websockets/ws)
 - [Node.JS: Socket.IO](https://socket.io/)
 - [Ruby: websocket-ruby](https://github.com/imanel/websocket-ruby)
 - [Django: Channels](https://channels.readthedocs.io/en/stable/)
 - [PHP: Ratchet, phpws](http://socketo.me/)

For a simple example, using the popular Socket.IO library, try https://github.com/socketio/chat-example


  [^caniuse]: (http://caniuse.com/#feat=websockets)

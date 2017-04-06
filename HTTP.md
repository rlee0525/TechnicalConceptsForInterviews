# HTTP (Hypertext Transfer Protocol)
- The HTTP client and HTTP server exchange information about resources identified by URLs.

## What are HTTP headers?
- HTTP headers allow the client and the server to pass additional information with the request or the response.

##### HTTP request header
- ex) GET /tutorials/other/top-20-mysql-best-practices/ HTTP/1.1
- method, path, protocol

##### HTTP response header
- ex) HTTP/1.x 200 OK

## What is HTTP Session?
- A sequence of network request-response transactions.
- HTTP client initiates a request -> establishes a TCP connection to a particular port on a host -> wait for a client's request message -> the server receives the request and sends back a status line
- HTTP -> TCP -> server -> status

## What are request methods?
##### HEAD
- similar to GET but w/o the response body.

##### GET
- requests a representation of the specified resource.

##### POST
- submits data to be processed to the identified resource.

##### PATCH
- apply partial modification.

##### DELETE
- deletes the specified resource.

##### PUT
- uploads a representation of the specified resource.

##### TRACE
- echoes back the received request.

##### OPTIONS
- returns the HTTP methods that the server supports for specified URL.

##### CONNECT
- converts the request connection to a transparent TCP/IP tunnel.

## Whats are safe methods?
- methods that are intended only for information retrieval without changing the state of the server.

## What is persistent connections?
- In HTTP/1.1, a keep-alive-mechanism was introduced, where a connection could be reused for more than one request/response pair.

## What is HTTP session state?
- a stateless protocol that does not require the server to retain information or status about each user for the duration of multiple requests. Often solved using HTTP cookies.

# HTTP (Hypertext Transfer Protocol)
- The HTTP client and HTTP server exchange information about resources identified by URLs.

## What are HTTP headers?
- HTTP headers allow the client and the server to pass additional information with the request or the response.

##### HTTP request header
- ex) `GET /tutorials/other/top-20-mysql-best-practices/ HTTP/1.1`
- method, path, protocol

##### HTTP response header
- ex) `HTTP/1.x 200 OK`

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

## Cookies
- An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser, that may store it and send it back together with the next request to the same server. Typically, it's used to know if two requests came from the same browser allowing to keep a user logged-in, for example. It remembers stateful information for the stateless HTTP protocol.
- Cookies are mainly used for these three purposes:
  - Session management (user logins, shopping carts)
  - Personalization (user preferences)
  - Tracking (analyzing user behavior)
- Cookies have also been used for general client-side storage. While this use could have been considered legitimate at a time when there was no other way to store data on the client side, it is no longer the case nowadays where web browsers are capable of using various storage APIs. Since cookies are sent along with every request, it can be an additional performance burden (especially for mobile web). New APIs to consider for local storage are the Web storage API (localStorage and sessionStorage) and IndexedDB.

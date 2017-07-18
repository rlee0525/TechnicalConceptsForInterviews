# Web Systems Basics

### HTTP vs. HTTPS
- HTTPS is the secure version of HTTP, the protocol over which data is sent between your browser and the website that you are connected to.
- S stands for 'Secure' and typically use SSL (Secure Sockets Layer) or TLS (Transport Layer Security) to encyrpt all communications between your browser and the website.
- TLS and SSL both use 'asymmetric' Public Key Infrastructure (PKI) system, which uses a public key and a private key to encrypt communications. Anything encrypted with the public key can only be decrypted by the private key and vice-versa.
- HTTPS is often used to protect highly confidential online transactions like online banking and online shopping order forms.

### XSS vs. CSRF
- CSRF (Cross-Site Request Forgery) attack - the attacker tries to force/trick you into making a request which you did not intend. 
  
    e.g. Sending you a link that makes you involuntarily change your password. 

    ```https://security.stackexchange.com/account?new_password=abc123```

- XSS (Cross-Site Scripting) attack - the attacker makes you involuntarily execute client-side code, most likely JS. 

  e.g. ```https://security.stackexchange.com/search?q="><script>alert(document.cookie)</script>```

- Both attacks have in common that they are client-side attacks and need some form of user activity.
- Unlike RFI (Remote File Inclusion) or SQLi (SQL injection), you are attacking a user rather than the server.
- XSS is generally more powerful because it allows the execution of arbitrary script code while CSRF is restrcted to a particular action.
- XSS attack also effectively bypasses all anti-CSRF measures if conducted successfully.

### Cookie / SessionStorage / LocalStorage
- localStorage and sessionStorage are both WebStorages and features of HTML5.
- localStorage stores information as long as the user does not delete them.
- sessionStorage stores information as long as the session goes. (Typically until the user closes the tab/browser).
- cookies are supported by older browsers (pre HTML5) and usually are a fallback for frameworks
- cookies can store way less information than WebStorages
- WebStorage information is never transferred to the server.

### Web Server vs. Application Server (Often used interchangeably)
- Web server is designed to serve HTTP Content. App server can also serve HTTP content but is not limited to just HTTP. (Other protocol support such as RMI/RPC)
- Web server is designed to serve static content (although plugins are used to generate dynamic HTTP content)
- App server can do whatever web server is capable of. Additionally, it has components and features to support Application level services such as connection pooling, object pooling, transaction support, messaging services, etc.
- Most of the production environments have web server acting as reverse proxy to app server -> web server identifies dynamic content request and transparently forwards to app server.

#### Examples

    Scenario 1: Web server without an application server

    you have an online store with only a web server and no application server. The site will provide a display where you can choose a product from. When you submit a query, the site performs a lookup and returns an HTML result back to its client. The web server sends your query directly to the database server (be patient, I will explain this one in our next nugget) and waits for a response. Once received, the web server formulates the response into an HTML file and sends it to your web browser. This back and forth communication between the server and database server happens every time a query is run.

    Scenario 2: Web server with an application server

    if the query you want to run has already been done previously and no data has changed since then, the server will generate the results without having to send the request to the database server. This allows a real-time query where a second client can access the same info and receive real time, reliable information without sending another duplicate query to the database server. The server basically acts as an intermediate between the database server and the web server. This allows the information pulled to be reusable while in the first scenario, since this info is embedded in a particular and "customized" HTML page, this is not a reusable process. A second client will have to request the info again and receive another HTML embedded page with the info requested -highly inefficient. Not to mention that this type of server is very flexible due to its ability to manage its own resources, including security, transaction processing, messaging and resource pooling.


### Web farm with load balanced application servers

> https://medium.com/on-coding/web-application-architecture-bca09ce0fabe

![Web App Diagram](http://res.cloudinary.com/rlee0525/image/upload/v1499810025/0-l2ZTQR-SfotH8Fnx_ustyky.png)

### Cookies vs Tokens

> https://auth0.com/blog/cookies-vs-tokens-definitive-guide/

#### Cookie Based Authentication
- Has been the default method for handling user authentication for a long time.
- **stateful**
    - Authentication record or session must be kept both server AND client-side.
    - The server needs to keep track of active sessions in a DB
    - On the FE, a cookie is created that holds a session identifier.
- User enters login credentials -> Server verifies and creates a session that is stored in a DB -> a cookie with the session ID is placed in the browser -> subsequent requests, the session ID is verified against the DB and if valid, request processes -> session is destroyed on both sides once a user logs out of the app.

#### Token Based Authentication
- Increased prevalence due to SPA (Single Page Applications), web APIs, and IoT.
- JSON Web Tokens (JWTs) <- standard
- **stateless**
- The server does NOT keep a record of which users are logged in / JWTs.
- Every request to the server is accompanied by a token which the server uses to verify the authenticity of the request.
- e.g. Authorization header: `Bearer { JWT }`
- User enters login credentials -> Server verifies and returns a signed token -> Stored most commonly in local storage but also in session storage / cookie -> subsequent requests, this token is included in the header -> The server decodes the JWT and if the token is valid, processes the request -> token is destroyed client-side without the need to interact with the server once a user logs out.

#### Advantages of Token Based Authentications
- **Stateless, Scalable, and Decoupled**
    - The BE doesn't have to keep a record of tokens
    - Auth0 can provide services to sign tokens
    - The server only needs to verify the validity of the token
- **Cross Doman and CORS**
    - Using JWT and checking with it every call to the BE, CORS enabled makes managing different domains trivial compared to cookies
- **JWT**
    - Unlike cookies where you simply store the session id, JWT allow you to store any type of metadata in JSON.
    - e.g. user id, expiration of the token, email address, who issued the token, permission, etc.
- **Performance**
    - Decoding a token is faster than looking up the BE
    - Additional lookup calls can also be avoided by including permission level to token
- **Mobile Ready**

#### Concerns with Token Based Authentications
- **JWT Size**
    - Much bigger than a session cookie - each request to the server must include the JWT along with it
- **Where to store tokens**
    - Mostly stored in local storage
    - Unlike cookies, local storage is sandboxed to a specific domain and its data cannot be accessed by any other domain including sub-domains
    - Session storage can also be the place
- **XSS and CSRF**
    - Cross Site Scripting, if not properly sanitized, could be executed on your domain making JWT tokens vulnerable
    - Storing JWT in local storage will prevent CSRF attacks
    - To prevent theses, have a short expiration time for tokens
- **Tokens**
    - Comprised of three parts: header, payload, and signature
    - Encoded NOT encrypted
    - No sensitive data should be stored in the payload

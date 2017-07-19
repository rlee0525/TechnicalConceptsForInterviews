# Navigating To A URL
> Source: http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/
> Source: https://superuser.com/questions/420949/how-do-cdn-content-distribution-networks-work

## Step 1
### You enter a URL into the browser.
- facebook.com

## Step 2
### The browser looks up the IP address for the domain name.

**Step 11 could happen on DNS level as well**

- **DNS Lookup** 
  - Domain Name Servers (DNS) are the Internet's equivalent of a phone book. They maintain a directory of domain names and translate them to Internet Protocol (IP) addresses. This is necessary because, although domain names are easy for people to remember, computers or machines, access websites based on IP addresses.

  - **Browser cache** – The browser caches DNS records for about 30 minutes.

  - **OS cache** – If the browser cache does not contain the desired record, the browser makes a system call (gethostbyname in Windows). The OS has its own cache.

  - **Router cache** – The request continues on to your router, which typically has its own DNS cache.

  - **ISP DNS cache** – The next place checked is the cache ISP’s DNS server. With a cache, naturally.

  - **Recursive search** – Your ISP’s DNS server begins a recursive search.
    - Root nameserver => .com top-level nameserver => Facebook’s nameserver.
    - Normally, the DNS server will have names of the .com nameservers in cache, and so a hit to the root nameserver will not be necessary.


    One problem: the entire domain like wikipedia.org or facebook.com maps to a single IP address.

    To mitigate the bottleneck:

    - **Round-robin DNS**: the DNS lookup returns multiple IP addresses, rather than just one. (e.g. facebook.com actually maps to four IP addresses)

    - **Load-balancer**: a piece of hardware that listens on a particular IP address and forwards the requests to other servers. (Major sites will typically use expensive high-performance load balancers.)

    - **Geographic DNS**: improves scalability by mapping a domain name to different IP addresses, depending on the client’s geographic location. (Great for hosting static content so that different servers don’t have to update shared state.)

    - **Anycast**: a routing technique where a single IP address maps to multiple physical servers. (It does not fit well with TCP and is rarely used in that scenario.)
      - Most of the DNS servers themselves use anycast to achieve high availability and low latency of the DNS lookups.

## Step 3
### The browser sends a HTTP request to the web server.
- FB homepage or other dynamic pages expire very quickly from the browser cache and therefore the browser will send GET request to the FB server.
- URL: "http://facebook.com/"

## Step 4
### The Facebook server responds with a permanent redirect.
- 301 status code (permanent redirect)
- Tell the browser to go to "http://www.facebook.com/"
- Search engine rankings between two URLs may be differenct.

## Step 5
### The browser follows the redirect.
- The browser sends another GET request to "http://www.facebook.com/".

## Step 6
### The server ‘handles’ the request.
- The FB server will receive the GET request, process it, and send back a response.
- Request handler reads the request, its parameters, and cookies to generate a HTML response.

## Step 7
### The server sends back a HTML response.
- 200 status code.
- response body is compressed and sent to the browser.

## Step 8
### The browser begins rendering the HTML.
- It begins rendering the website even before the browser has received the entire HTML document.

## Step 9
### The browser sends requests for objects embedded in HTML.
- As the browser renders the HTML, it will notice tags that require fetching of other URLs which will send more API requests.

## Step 10
### The browser sends further asynchronous (AJAX) requests.
- The client continues to communicate with the server even after the page is rendered.

## Step 11
### Utilizing CDN (Initial request)
- The browser parses the HTML file and finds the referenced image in it that is located at CDN.
- It finds the IP address of CDN
- Connects to that IP address -> CDN server -> requests image

## Step 12
### Utilizing CDN (Geo Location - one of the many approaches)
- CDN realizes the person is from Germany. Instead of sending the picture, it sends an HTTP redirect with IP address of CDN geographically closer to you.

## Step 13
### Utilizing Cache
- This redirect step will be cached and all future requests will go to the closest content server
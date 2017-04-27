# Navigating To A URL
> Source: http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/

### Step 1
#### You enter a URL into the browser
- facebook.com

### Step 2
#### The browser looks up the IP address for the domain name.

- **DNS Lookup**
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

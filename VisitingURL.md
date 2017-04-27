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

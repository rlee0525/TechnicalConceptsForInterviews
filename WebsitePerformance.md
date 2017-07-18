# Website Performance
> https://developer.yahoo.com/performance/rules.html#num_http

### Minimize HTTP Requests
- 80% of the end-user response time is spent on the FE. Most of this time is tied up in downloading all the components in the page: images, stylesheets, scripts, Flash, etc.
- **Combine files**
  - Combining all scripts into a single script (e.g. webpack)
  - Combining all CSS into a single stylesheet
  - CSS Sprites
  - Image maps
  - Inline images

### CDN (Content Delivery Network)
- The user's proximity to your web server has an impact on response time
- Improve end--user response time by moving static content off of application web servers to a CDN

### Cache-Control Header
- A first-time visitor to your page may have to make several HTTP requests but by using the Expires header you can make these components cacheable
- Most often used with images but they should be used on all components including scripts, stylesheets, and Flash components - shorter expiration date for more dynamic components

### Gzip Components
- Most websites gzip their HTML documents
- Also worthwhile to gzip your scripts and styleshseets, as well as any text response including XML and JSON.
- Images and PDF files should not be gzipped because they are already compressed

### Put Stylesheets at the Top
- Putting stylesheets in the HEAD allows the page to render progressively
- The browser will display whatever content it has as soon as possible

### Minify JS and CSS
- Remove unnecessary characters from code to reduice its size and therefore improve load times

### Reduce DNS Lookups
- DNS lookups (hostnames to IP addresses) can be cached for better performance

### Avoid Redirects

### Post-load Components
- Code splitting (webpack)
- Determine which content and components can wait

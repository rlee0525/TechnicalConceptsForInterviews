# Architecture (The Basics - High Level Overview)

## Architecture Questions vs. Design Questions (They are DIFFERENT!)

### Design Questions
- How would you design a Scrabble game? (Object-oriented Design!)
- What are some architecture choice you make to create a problem that manages a car park?
- Walk me through how you'd make a Tetris game. How would you model the data?

**classes, instance variables, how they interact, etc.**

### Architecture Questions
- How would you design Twitter?
- Say you need to create a WhatsApp clone. Imagine you have millions of users. How do you build your system? What services do you create?
- Explain to me how you'd architect an app like OpenTable?

**Talk about DB, application layer, caching**

**BAD ANSWER**: I'd make a Rails app. I'd use such and such models and here's my DB schema.

- Running on Heroku WEBrick server could probably service 100~requests per minute.
- Production servers using rails => Unicorn(multi-process server) or Puma(multi-threaded server) => ex) Unicorn: 7 different workers available per request.

## Standard Web App Architecture
### Basic Pieces (In order)
- Browser
- Load balancer
- Application servers
- Caching layer
- CDN(s)
- Database

### Databases - slow
- Relational Database (joining tables)
  - easy to use, easy to query
  - denormalizing can make it faster (duplicate data to avoid constant join tables) => used often to scale using RDBMS
  - suck at scaling
- NoSQL (schema is not enforced)
  - Document-based => ex) MongoDB (Giant Hash map with keys and values as JSON object)
  - K-V stores => ex) Redis (Similar to document-based but with less power - no indexes, joins, etc.)
  - Distributed databases => ex) Cassandra (Cluster of databases - allows failures because other nodes will pick up the slack) - all of the big websites use distributed databases.

#### Failover
- Prevent a single point of failure
- Leader and followers (master-slave relationship)

##### Scaling a Relational Database
- high read load then master slave replication (e.g. Twitter / Facebook)
  - Leader has many followers. When DB requests, one of the followers will read the request. However only leader can write to keep things in sync.

- high write load, then shard (e.g. WhatsApp)
  - Not good for Relational DB.
  - Use data sharding. keep database smaller and organized. sharding becomes really bad when data is shared across sharded tables
    - multiple data centers must be kept consistent
    - within shard, master slave replication exists
- easy to undo: among the reasons why it's popular among startups

### CDNs
- Giant data centers designed to send big files to users and to their ISPs as fast as possible - geographically closer (everywhere)
- If you are front end is running slowly, you can use CDN! (such as JS, jQuery, Bootstrap, or CSS)
- All the JS files will be concatenated together into a giant file to reduce to only one request. (This is why we use Webpack to do it!)
- your server should return only HTML markup (much faster loading)

### Caching Layer (Big chunk of memory) - fast
- Important for preventing database from getting smashed
- Caches are generally stored in memory (e.g. RAM)
- e.g. Redis and Memcached
- All caches are K-V stores
- Store common queries (e.g. someone's tweets <- no need to hit DB), expensive queries (e.g. trending hashtag), or static data
- Anything that hits DB hits caching layer first and is stored
- Cache invalidation such as LRU

**How do I make a website faster? Caching & CDN**

### Application Server

#### Heroku vs. AWS
- Heroku is PaaS (Platform As A Service) - Heroku will figure out all of the settings (PaaS) and therefore more expensive - not suitable for large scale app because it is simply too expensive and not as configurable.
- AWS is IaaS (Infrastructure As A Service) - Just servers without additional services. Provides access to virtual machines that are just fresh linux box. DevOps will make sure these servers have right configurations to run your codes correctly. -> Definitely cheaper than using PaaS and generally cheaper than having and managing your own servers (unless you are a giant company that needs a lot of data centers).

#### Services vs. Monolith
- SOA (service-oriented architecture) AKA "microservices" (Modularity on the application level)
  - e.g. Multiple rails app (one for user, one for tweets, etc) -> tweet syntax error won't break user authentication app.
- Monoliths
  - e.g. Rails app -> one syntax error breaks everything.

**Example of Services (Uber -> Big organizations love microservices as it's essential that everything always appear working and is instant)**
- Routing service (How do I get from point A to point B)
- Dispatch service (Connect riders to drivers)
- Payment processing service
- Reviewing service
- User authentication service

**Pros and cons of SOA**
- Failures can be isolated to particular services without taking down the entire system
- Easy to divide among teams, a team can keep their codebase small and understandable
  - Microservices can be written in various languages unlike monolith
  - Easier to do small refactorings
  - Harder to do big refactoring across many services
- A little bit of overhead in messages between apps unlike monolith

**Client vs. Server**
- Use HTTP via TCP or use UDP
- TCP: this data must get to the end user. send request to server to build connection then sends packets in order. Lots of overhead. Slower but more secure as delivery is guaranteed and in order.
  - WhatsApp -> Use TCP to make sure message is sent to a friend even if s/he is offline at the moment.
- UDP: similar but not in order and no guarantee in delivery. Very low latency compared to TCP but meaning it's very fast.
  - Uber -> driver sends their location to Uber every 5 seconds and Uber app sends most recent version of this information to users when they open the app. => for this, UDP is preferred because this has to happen very fast, the order doesn't matter as much, and guaranteed delivery is not as important as they are constantly sending this information.
- What info should live on the client rather than the server?
  - What needs to be persistent and what's okay to lose?
  - Scalability tradeoffs
  - What if client and server become inconsistent?

**Asynchronous jobs**
- Does it need to be done right this second?
- If not, just queue it up in a background process and tell your user you'll get it done
- Many things can be done asynchronously

### Load Balancer (Distribute the load to different servers)
- Reduces load
- What if this goes down?
  - More load balancers / fallover
- Round Robin DNS
- Put one into the database layer as well so cache can talk to load balancer rather than directly to the database.

**Any big website, when you get IP address, it's not actually application server. It's load balancer that will send you to the server**

## How much do your most common operations cost?

### Facebook Design
- Giant NoSQL graph DB of users.
- Each user has list of friends array. (exists in caching layer)
- Triggers read for every single one of your friends every time the user wants newsfeed (which is dynamic so cannot just store). (Writes are really cheap, reads are not)
- 5000 friends limit because this is unscalable beyond that point because read is performed every single time.

### Twitter Design
- Each user has a list of feed that contains array of tweet ids. (exists in caching layer)
- Every time you write in Twitter, it writes to every one of your followers and put the id to end of their feed array. (Writes are really expensive, Read are really cheap because feeds are already there)

**There are always tradeoffs**

**How would you design Twitter?**
- NoSQL
- read-heavy, expensive writes but tradeoffs (why not Facebook way)
- store in caching layers
- load balancers in front
- distributed database over data centers
- IaaS

### WhatsApp Design
- store more relationally (Can use RDBMS up to a million users maybe)
- Use TCP
- Client server application (out of sync problems)
- Do we want clients to talk directly to each other without going through servers?

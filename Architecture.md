# Architecture (The Basics - High Level Overview)

## Architecture Questions vs. Design Questions (They are DIFFERENT!)

### Design Questions
- How would you design a Scrabble game? (Object-oriented Design!)
- What are some architecture choice you make to create a problem that manages a car park?
- Walk me through how you'd make a Tetris game. How would you model the data?

###### classes, instance variables, how they interact, etc.

### Architecture Questions
- How would you design Twitter?
- Say you need to create a WhatsApp clone. Imagine you have millions of users. How do you build your system? What services do you create?
- Explain to me how you'd architect an app like OpenTable?

###### Talk about DB, application layer, caching

###### BAD ANSWER: I'd make a Rails app. I'd use such and such models and here's my DB schema.

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

### Databases
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

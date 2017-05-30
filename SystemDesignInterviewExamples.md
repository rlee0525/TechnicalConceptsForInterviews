# System Design Interview Examples

> Source: http://blog.gainlo.co/index.php/2016/02/17/system-design-interview-question-how-to-design-twitter-part-1/

## Design Twitter

#### Define the problem
- Compress Twitter to its MVP
  - Follow each other and view others' feeds

#### High-level solution
- Divide the whole system into several core components
  - Data modeling
    - Using RD: user object and feed object. User can follow each other, each feed has a user owner.

  - How to serve feeds
    - fetch feeds from all the people you follow and render them by time

#### Detail questions
- When users followed a lot of people, fetching and rendering all their feeds can be costly. How to improve this?
  - Infinite scroll feature to fetch the most recent N feeds instead of all of them.
  - Caching results

- How to detect fake users?
  - Use ML algorithms to identify related features such as registration date, # of followers, # of feeds, etc to detect if a user is fake.

- Can we order feed by other algorithms? (e.g. based on users' interests)
  - Questions

    1) How to measure the algorithm?
      - Maybe by the average time users spend on Twitter or users interaction like favorite/retweet.

    2) What signals to use to evaluate how likely the user will like the feed?
      - Users relation with the author, # of replies/retweets of this feed, # of followers of the author

- How to implement the @ feature and retweet feature?
  - @ feature: store a list of user IDs inside each feed. When rendering your feeds, you should also include feeds that have your ID in its @ list.
  - retweet feature: inside each feed, a feed ID is stored, which indicates the original post if there's any.

#### Trending topics
- Questions

  1) How to get trending topic candidates?
    - Get most frequent hashtags over the last N hours
    - Get the hottest search queries
    - Fetch the recent most popular feeds and extract some common words or phrases

  2) How to rank those candidates?
    - Rank based on frequency
    - Integrate signals like reply/retweet/favorite numbers/freshness
    - Personalized signals such as # of follows/followers

#### Other features
 - Who to follow
 - Moments
 - Search

## Design Photo Sharing App

#### High-level solution
- Two major objects: user and picture
- Relational database
  - User table: name, email, registration date
  - Picture table
  - user, follow relation
  - user, picture relation

#### Potential scale issues
- Response time
  - Rendering users feed - the server has to go over everyone the user follows, fetch all the pictures from them, and rank them based on particular algorithms.
  - Accelerate picture fetching and ranking by implementing infinite scroll feature, use offline pipelines to precompute signals that can speed up the ranking, etc.

- Scale architecture
  - With millions of users, due to storage, memory, CPU bound issues, and etc, divide the whole system into small components by service and separate each component. Use **load balancers**. Service-oriented architecture > monolithic application.

- Scale database
  - Vertical splitting (partitioning) by splitting the database into sub-databases by features
  - Horizontal splitting (sharding) by splitting based on attributes like US users, European users.


## The Twitter Problem

> Source: https://www.hiredintech.com/classrooms/system-design/lesson/67

#### Question
- “Design a simplified version of Twitter where people can post tweets, follow other people and favorite* tweets.”

#### Clarifying Questions
- How many users do we expect this system to handle?
  - Let's say 10 million users generating around 100 million requests per day.
- How many connections will these users each have? (Users will form a graph with the users being the nodes and the edges will represent who follows whom)
  - We expect that each user will be following 200 other users on average, but expect some extraordinary users with tens of thousands of followers.
- Tweets -> How many requests will posting new tweets and favoriting them generate?
  - We expect there will be a maximum of 10 million tweets per day and favorited twice on average per post.


      To summarize, here are some more things we now know:

        - 10 million users
        - 10 million tweets per day
        - 20 million tweet favorites per day
        - 100 million HTTP requests to the site
        - 2 billion “follow” relations
        - Some users and tweets could generate an extraordinary amount of traffic


#### High-level Design
1) The logic o handle all incoming requests to the application
2) The data storage to store all the data that needs to be persisted

- Handling user requests
  - the complexity of the requests that the application receives
  - the technologies used to implement the application
    - suggest using a load balancer, which handles initial traffic and sends requests to a set of servers running one or more instances of the application. Also **increased resilience** by using more than one server in case of failure.


- Storing the Data
  - What do we store?
    - User profile with data
    - Each user's set of tweets over time
    - Following users
    - User <-> tweets Favorite

  - Size of the data?
    - 10 million tweets per day -> ~3.65 billion tweets per year -> 140 characters -> 2.6 terabytes of data (2 bytes per character)
    - ~2 billion connections -> 16 gigabytes of data (4 byte integer * 2 IDs)
    - ~7 billion favorites per year -> ~240 gigabytes of data

  - Using a RD allow us to model relations easily for users, tweets, and the connections between them.

  **Utilize caching such as redis or memcached** to minimize direct reads from the DB by storing the popular bits of data in memory.
    - DB stores data on disk and its much slower to read than from memory. We could also store more complex pieces of data like the results from popular queries.
    - DB Indexing => vital for executing quick queries joining tables.
    - Partitioning DB to improve read and write speeds.

#### Database Schema
- **User** -> id, username, full name, password digest, timestamp, description
- **Tweets** -> id, content, timestamp, user_id
- **Connections** -> follower_id, followee_id
- **Favorites** -> user_id, tweet_id, created_at

- Implementing queries to our data filtering users by their username => add_index over this build to optimize query time.
- add_index from user_id and tweets as they are most often called together.
  - consider also including column created_at to allow users to search filtered results and pull only relevant posts
  - **Order Matters!** <user_id, created_at> <- user_id will take precedence

#### Additional Considerations
**Increased # of read requests**
- Database is the first bottleneck as it become overwhelmed with all the read requests.
  - a) use replication (denormalization)
  - b) sharding database and spread the data across different machines
- Web application itself - it could lead to slow response time
  - a) scaling out by adding more machines running the application
- Web deployment: AWS > Heroku with scaling
- load balancing solution: get rid of single point of failure.

**Scaling the database**
- Add an in-memory cache solution in front of the database. (key-value store)
- Partitioning the database

## Design Instagram
> Source 1: https://engineering.instagram.com/what-powers-instagram-hundreds-of-instances-dozens-of-technologies-adf2e22da2ad

> Source 2:
https://engineering.instagram.com/sharding-ids-at-instagram-1cf5a71e5a5c

Sharding & IDs at Instagram
- Make sure all of our important data into memory for quick read
- Application servers run Django with PostgreSQL as back-end database

**Essential Features**

1) Generated IDs should be sortable by time (so a list of photo IDs, for example, could be sorted without fetching more information about the photos)

2) IDs should ideally be 64 bits (for smaller indexes, and better storage in systems like Redis)

3) The system should introduce as few new ‘moving parts’ as possible — a large part of how we’ve been able to scale Instagram with very few engineers is by choosing simple, easy-to-understand solutions that we trust.

**Storing Images (CDN)**
- Storing the images in an Amazon S3 bucket and retrieving from Amazon's server instead of your own. You keep the images text details on your server including its url path to its location in the S3 cloud server.

# Tips and Concepts

## It's an open-ended conversation
For the most part, you’ll be steering the conversation. It’s up to you to understand the problem. That might mean asking questions, sketching diagrams on the board, and bouncing ideas off your interviewer. Do you know the constraints? What kind of inputs does your system need to handle? You have to get a sense for the scope of the problem before you start exploring the space of possible solutions. And remember, there is no single right answer to a real-world problem. Everything is a tradeoff.

## Topics

### Concurrency 
  - Threads, deadlock, and starvation 
  - How to parallelize algorithms
  - Consistency and coherence

### Networking
  - IPC and TCP/IP? 
  - Throughput vs. latency, and when each is the relevant factor

### Abstraction
  - How do OS, file system, and database work? 
  - Do you know about the various levels of caching in a modern OS?

### Real-World Performance
  - Relative performance of RAM, disk, SSD and your network.

### Estimation
  - Estimation, especially in the form of a back-of-the-envelope calculation, is important because it helps you narrow down the list of possible solutions to only the ones that are feasible. Then you have only a few prototypes or micro-benchmarks to write.

### Availability and Reliability
  - Are you thinking about how things can fail, especially in a distributed environment? 
  - Do know how to design a system to cope with network failures?
  - Do you understand durability?
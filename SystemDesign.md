# System Design and Scalability

## Key Concepts
#### Horizontal vs. Vertical Scaling
- Vertical scaling: increasing the resources of a specific node. (e.g. adding additional memory to a server to improve handling load changes)
- Horizontal scaling: increasing the number of nodes. (e.g. adding additional servers to decrease load on any one server)
**Vertical scaling is generally easier but more limited**

#### Load Balancer
- Frontend parts of a scalable website will be thrown behind a load balancer.
- A system can distribute the load evenly so that one server doesn't crash and take down the whole system.
- To do so, build out a network of cloned servers that all have essentially the same code and access to the same data.

#### Database Denormalization and NoSQL
- Joins in a RDB such as SQL can get very slow as the system grows bigger.
- Denormalization: adding redundant information into a database to speed up reads. (e.g. Rails includes to solve N + 1 queries)
- Or simply use NoSQL DB.

#### Database Partitioning (Sharding)
- Sharding: splitting the data across multiple machines while ensuring you have a way of figuring out which data is on which machine.
  - Vertical Partitioning: partitioning by feature (e.g. profiles, messages, etc.)
    - Drawback: if one of these tables gets very large, that DB need to be repartitioned using a different scheme.
  - Key-Based Partitioning: partitioning by part of the data, most commonly id. (e.g. allocate N servers and put the data on key % n)
    - Drawback: the number of servers is effectively fixed. Adding additional servers means reallocating all the data which is a very expensive task.
  - Directory-Based Partitioning: maintaining a lookup table for where the data can be found.
    - Drawback: the lookup table can be a single point of failure and constantly accessing this table impacts performance.
**Many architectures actually end up using multiple partitioning schemes**

#### Caching
- A simple key-value pairing that typically sits between application layer and data store.
- When an application requests a piece of information, it first tried the cache then it'll look up the data in the data store if the cache does not contain the key.
- Can cache a query and its results or specific objects (e.g. rendered version of a part of the website)

#### Asynchronous Processing & Queues
- Slow operations should be done asynchronously.

#### Networking Metrics
- Bandwidth: maximum amount of data that can be transferred in a unit of time. (bits per second)
- Throughput: actual amount of data that is transferred.
- Latency: how long it takes data to go from one end to the other. (delay between sending and receiving)

#### MapReduce
- Used to process large amounts of data - by allowing parallel processing
- Map takes in some data and emits a <key, value> pair
- Reduce takes a key and a set of associated values and "reduces" them in some way, emitting a new key and value.

## Considerations
- Failures
- Availability and Readability
- Read-heavy vs. Write-heavy
- Security

## CAP Theorem
- It is impossible for a distributed computer system to simultaneously provide more than two out of three of the following guarantees: Consistency, Availability, and	Partition tolerance.
- Consistency: every read receives the most recent write or an error.
- Availability: every request receives a (non-error) response – without guarantee that it contains the most recent write.
- Partition tolerance: the system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.
- In other words, the CAP Theorem states that in the presence of a network partition, one has to choose between consistency and availability.

## ACID
- Atomicity, Consistency, Isolation, Durability - a set of properties of database transactions.
- Atomicity: requires that each transaction be "all or nothing"
  - if one part of the transaction fails, then the entire transaction fails, and the database state is left unchanged. An atomic system must guarantee atomicity in each and every situation, including power failures, errors and crashes.
- Consistency: ensures that any transaction will bring the database from one valid state to another.
  - Any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof. Any programming errors cannot result in the violation of any defined rules.
- Isolation: ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed sequentially, i.e., one after the other.
  - Providing isolation is the main goal of concurrency control. Depending on the concurrency control method (i.e., if it uses strict - as opposed to relaxed - serializability), the effects of an incomplete transaction might not even be visible to another transaction.
- Durability: ensures that once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors.
  - For instance, once a group of SQL statements execute, the results need to be stored permanently (even if the database crashes immediately thereafter). To defend against power loss, transactions (or their effects) must be recorded in a non-volatile memory.

## Examples
### Designing a cache
##### For a single system (HASHMAP!)
- How would you create a data structure that enables you to easily purge old data and also efficiently look up a value based on a key?
- A linked list where a node is moved to the front every time it's accessed and remove the last element of the linked list when the list exceeds a certain size. (e.g. LRU Cache)
  - Easily purge old data.
- A hash table that maps from a query to the corresponding node in the linked list. This allows us to not only efficiently return the cached results, but also to move the appropriate node to the front of the list.
  - Efficient lookups of data.

##### For many machines
- Several options to consider in deciding to what extend the cache is shared across machines.
  - 1) Each machines has its own cache - relatively quick as no machine-to-machine calls but less effective
  - 2) Each machine has a copy of the cache - common queries would nearly always be in the cache. However, too much time and space.
  - 3) Each machine stores a segment of the cache - Assign queries based on the formula hash(query) % N to find out which machine holds certain part of the hash table. Increased # of machine-to-machine calls but with advantages.

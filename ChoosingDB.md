# RDBMS or NoSQL

## Quick Comparison
Reasons for **SQL**:

* Structured data
* Strict schema
* Relational data
* Need for complex joins
* Transactions
* Clear patterns for scaling
* More established: developers, community, code, tools, etc
* Lookups by index are very fast

Reasons for **NoSQL**:

* Semi-structured data
* Dynamic or flexible schema
* Non-relational data
* No need for complex joins
* Store many TB (or PB) of data
* Very data intensive workload
* Very high throughput for IOPS

## Nature of Data
- RDBMS: simple tabular structure
- NoSQL: complicated data with multiple level of nesting (e.g. geo-spatial, engineering parts), easily represented as JSON

## Data Volatility
- RDBMS: rigid schema - correct design the first time is important as it is slow to update (low flexibility)
- NoSQL: popular among Web-centric businesses that require dynamic schema (high flexibility)

**As DB grows in size or # of users multiply, RDBMS often has performance issues**

## Operational Issues (scale, performance, high availability)
Usual Steps for companies with operational issues:
- Vertical scaling (more power with added CPU and RAM but high cost) => processors are added (linear scaling) until bottleneck
- Horizontal scaling (clustering) - adding more machines often provided RDBMS => expensive and complex

- Consider NoSQL (Essential especially in Big Data environment) => built to host distributed DB for online systems, high availability but with possible consistency issues

**RDBMS => multi-join tables => high latency**
**RDBMS => prioritize reliability (via ACID) and easier maintenance over performance**
**ACID (atomicity, consistency, isolation, durability) - guaranteed for RDBMS but not noSQL**

## Data Warehousing and Analytics
- RDBMS: ideally suited for complex query and analysis (even Hadoop data is sometimes loaded back to an RDBMS for reporting purposes)
- NoSQL: real-time analytics for operational data

**NoSQL taxonomy supports key-value stores, document store, BigTable, and graph databases**
**NoSQL => non-relational, distributed, open-source and horizontally scalable**

## Reasons To Use A NoSQL DB

#### Storing large volumes of data that often have little to no structure.
- A NoSQL database sets no limits on the types of data you can store together, and allows you to add different new types as your needs change. With document-based databases, you can store data in one place without having to define what “types” of data those are in advance.

#### Making the most of cloud computing and storage.
- Cloud-based storage is an excellent cost-saving solution, but requires data to be easily spread across multiple servers to scale up. Using commodity (affordable, smaller) hardware on-site or in the cloud saves you the hassle of additional software, and NoSQL databases like Cassandra are designed to be scaled across multiple data centers out of the box without a lot of headaches.

#### Rapid development.
- If you’re developing within two-week Agile sprints, cranking out quick iterations, or needing to make frequent updates to the data structure without a lot of downtime between versions, a relational database will slow you down. NoSQL data doesn’t need to be prepped ahead of time.

# Row Store and Column Store Databases

## Basic
- Row: great for transaction processing
- Column: great for highly analytical query models

- Row: writes very quickly
- Column: writes slowly but reads very quickly

- Row: standard traditional DB
- Column: each field from each table is stored in its own file or set of files, minimize i/o by only accessing the files that contained data from the requested fields

## Normalization Versus Denormalization

- Column: many columnar databases prefer a denormalized data structure => no joins would need to be processed and thus the query will likely run much faster

- Row: normalized data => allows data to be written to the database in a highly efficient manner - need to record just the relevant details and thus writes much faster. Updating is also more efficient as it affects only one record in the table. In the columnar DB, many records might need updating.

## Conclusion

- Often, mixture of the two. The initial write is to a row-based system. Then, write the data (or the relevant parts of the data) to a column based database to allow for fast analytic queries.

# Hadoop vs. NoSQL

- Both came out as a way to handle Big Data
- Incremental, horizontal scaling (scaling out)
- Varying, changing data formats

### Hadoop
- batch-oriented
- large-scale processing
- massive compute power
- big processing tasks on large volumes of data => works spread across many servers in parallel. Hadoop manages the process by using divide and conquer method known as map reduce. Process close to data so you are not accessing data across the network and thus slowing down the network.
- ex) predictive analytics, fraud detection, and recommendation

### NoSQL
- real-time
- interactive
- fast reads / writes
- ex) user transactions, sensor data, customer profiles (all the information that may be updated rapidly)

**Fast read / write using NoSQL on one cluster and using Hadoop for large scale analytics.**

# Common Types of NoSQL DB
#### Key-value model
- the least complex NoSQL option, which stores data in a schema-less way that consists of indexed keys and values.
**Examples**: Cassandra, Azure, LevelDB, and Riak.

#### Column store
- wide-column store, which stores data tables as columns rather than rows. It’s more than just an inverted table—sectioning out columns allows for excellent scalability and high performance.
**Examples**: HBase, BigTable, HyperTable.

#### Document database
- taking the key-value concept and adding more complexity, each document in this type of database has its own data, and its own unique key, which is used to retrieve it. It’s a great option for storing, retrieving and managing data that’s document-oriented but still somewhat structured.
**Examples**: MongoDB, CouchDB.

#### Graph database
- have data that’s interconnected and best represented as a graph? This method is capable of lots of complexity.
**Examples**: Polyglot, Neo4J.

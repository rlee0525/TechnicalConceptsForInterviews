# Hash Table Collision Resolution

> Contributor: Will Ashley


## Background – What is A Hash Table?

A hash table is, essentially, a data structure that maps keys to values. It does this by taking each input into the table and processing it with a hashing function, which is used to determine the index in a bucket the value will rest. 

Hash tables are useful because insertion, deletion, and lookup of elements can possibly be achieved in constant time. If you have a hash function that behaves consistently, it will tell you where to find an element in a hash table each time. But what if, in the process of trying to store more information in your hash table, your hashing function tells you to store something at an index that is already occupied? 

## Hash Collisions

### What is a hash collision? 

A hash collision is when two different inputs to a hashing function result in the same index in a bucket. Hash collisions are unavoidable if you have a sufficiently large data set, so knowing how to handle them is important – doing it poorly will make the time complexity of lookup in the hash table increase substantially. 
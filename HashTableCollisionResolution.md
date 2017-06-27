# Hash Table Collision Resolution

> Contributor: Will Ashley


## Background – What is A Hash Table?

A hash table is, essentially, a data structure that maps keys to values. It does this by taking each input into the table and processing it with a hashing function, which is used to determine the index in a bucket the value will rest. 

Hash tables are useful because insertion, deletion, and lookup of elements can possibly be achieved in constant time. If you have a hash function that behaves consistently, it will tell you where to find an element in a hash table each time. But what if, in the process of trying to store more information in your hash table, your hashing function tells you to store something at an index that is already occupied? 

## Hash Collisions

### What is a hash collision? 

A hash collision is when two different inputs to a hashing function result in the same index in a bucket. Hash collisions are unavoidable if you have a sufficiently large data set, so knowing how to handle them is important – doing it poorly will make the time complexity of lookup in the hash table increase substantially. 

Let's take a look at a few ways we can approach this problem.

## Resolutions

### Linear Probing

Linear probing should be considered the naive solution to a hash collision. Let's look at an example:


`["apple", "bat", "car"]`

Let's take this array as a really simple hash table. Our hashing function will take an input, such as "dog", and use the first letter's place in the alphabet to determine the index. Since "dog" starts with "d", it will get hashed to the 3rd index in the array. 

Simple enough, and we can see the advantages of speedy lookup and insertion right here: if we use the hash function to check if "dog" exists in the table, the lookup takes place in constant time. 

But what if we try to add "ant" to the table? 

Since "ant" would normally get hashed to the 0th index, we would try to insert it there, but for us, "apple" already occupies that space. **collision!**

*Linear Probing* says "let's just put it at the next available index!"

So our hash table would look like this:

`["apple", "bat", "car", "ant"]`

But doing this requires us to first check the 1st index, then the 2nd, and finally, we arrive at the 3rd index, where we find free space! As you can see, insertion, deletion, and lookup have devolved to  `O(n)` in the worst case! Bleh!

### Separate Chaining

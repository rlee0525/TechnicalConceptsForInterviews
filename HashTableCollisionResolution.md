# Hash Table Collision Resolution

> Contributor: Will Ashley


## Background – What is A Hash Table?

A hash table is, essentially, a data structure that maps keys to values. It does this by taking each input into the table and processing it with a hashing function, which is used to determine the index in a bucket where the value will reside. Hash tables appear in interviews all the time, especially at companies like Microsoft and Google. Know them! 

Hash tables are useful because insertion, deletion, and lookup of elements can possibly be achieved in constant time. If you have a hash function that behaves consistently, it will tell you where to find an element in a hash table each time. But what if, in the process of trying to store more information in your hash table, your hashing function tells you to store something at an index that is already occupied? 

## Hash Collisions

### What is a hash collision? 

A hash collision is when two different inputs to a hashing function result in the same index in a bucket. Hash collisions are unavoidable if you have a sufficiently large data set, so knowing how to handle them is important – doing it poorly will have a dramatic impact on performance.

Let's take a look at a few ways we can approach this problem.

## Resolutions

### Open Addressing with Linear Probing

Let's look at an example:

`["apple", "bat", "car"]`

Let's take this array as a really simple hash table. Our hashing function will take an input, such as "dog", and use the first letter's place in the alphabet to determine the index. Since "dog" starts with "d", it will get hashed to the 3rd index in the array. 

Simple enough, and we can see the advantages of speedy lookup and insertion right here: if we use the hash function to check if "dog" exists in the table, the lookup takes place in constant time. 

But what if we try to add "ant" to the table? 

Since "ant" would normally get hashed to the 0th index, we would try to insert it there, but for us, "apple" already occupies that space. **collision!**

*Linear Probing* says "let's just put it at the next available index!"

So our hash table would look like this:

`["apple", "bat", "car", "ant"]`

But doing this requires us to first check the 1st index, then the 2nd, and finally, we arrive at the 3rd index, where we find free space! As you can see, insertion, deletion, and lookup have devolved to  `O(n)` in the worst case.

And what happens when we want to insert "dog" into the list now? It would hash to index 3, but since "ant" now occupies that space, it must go to index 4. This is called **clustering.**

#### Quadratic Probing 

As you might have noticed, when we *probe* the array for an empty index, we are probing by a *linear* factor, in this case, 1. We simply take the hash index and add 1 to it repeatedly until we find an empty index. But we could also probe by a quadratic factor, but adding to the hashed index the successive outputs of a quadratic polynomial IE:

`H + 1^2, H + 2^2, H + 3^2`

Where `H` is the result originally given by the hashing function. This results in less clustering than linear probing. This only really matters though if your data set is sufficiently large.

### Separate Chaining

*Separate chaining,* most commonly uses linked lists at each index:

`[{"ant" => "apple"}, {"bat"}, {"car"}]`

Where `=>` represents a pointer to the next node in the list. Using this method, we can simply insert new elements at the head of each list, should there be a collision. 

In the worst case, insertion, lookup, and deletion become `O(n/k)` where `k` is the size of the hash table. 

If `k` is a constant, then `O(n/k)` is really just `O(n)`, but in the real world,  `O(n/k)` is a huge improvement over  `O(n)`

### Robin Hood hashing

*Robin Hood hashing* says that in the event of a collision, the new value may displace the value currently residing in the bucket if its probe count is larger than the key at the current position. Since both the worst case and the variation in the number of probes is reduced dramatically, an interesting variation is to probe the table starting at the expected successful probe value and then expand from that position in both directions.

### Cuckoo hashing

*Cuckoo Hashing* utilizes two or more hashing functions to resolve collisions. When a collision occurs, the value is rehashed using the second hash function, if that still causes a collision, then it will rehash with the third hashing function, and so on, until there it either finds an empty bucket for the value or runs out of hashing functions. If it runs out of hashing functions, it will use the last hashing function to place the value in its corresponding bucket, and remove whatever value is currently there, and rehash it using the first hashing function. This process continues over and over until a place for all values is found. This does mean that any value could possibly be in more than one location, but still ensures constant time lookup in the worst case, and amortized constant time for insertion and deletion.  

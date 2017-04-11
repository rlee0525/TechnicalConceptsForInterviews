# Pointers and Memory Allocation

## Basics (Array)
### Methods
- indexing []
- setting []=
- push
- pop
- shift
- unshift

### Static vs. Dynamic Arrays
- Static - only indexing and setting
- Dynamic - built on static array

### Random Access Memory (RAM)
- Indifferent to how you access the memory. (any address can be accessed equally as fast)
- Think of it as contiguous blocks of cells
  - Each cell can only store 8 bytes of information (for 64 bit architecture)
  - Each cell has memory address associated with it. (e.g. 800 for first cell then 808 for next and so on)
  - Stores binary data
- Loading information from RAM is very slow. Try to load it from Registers. (fastest it can be) - Even having cache is slower than having information in Registers.

### CPU
**CPU is used to access, store, or manipulate information using RAM.**

#### CPU Methods to manipulate data
- Load
- Store
- Add, Subtract, Multiply, Divide, Mod
- Greater / Less / or equal
- Logical operations (and, or)
- Jump, If

## Pointers
- In computer science, a pointer is a programming language object, whose value refers to (or "points to") another value stored elsewhere in the computer memory using its memory address.
- A pointer references a location in memory, and obtaining the value stored at that location is known as dereferencing the pointer.
- Two pointers can equal each other, such that changing one's value also changes the other's value (since they, in fact, point to the same address)

## References
- An alias for a pre-existing object and it does not have memory of its own.
- One cannot create a reference without specifying where in memory it refers to.
- Unlike pointers, references cannot be null and cannot be reassigned to another piece of memory.

## Malloc (Intro to C Programming)

### int a;
- The compiler sorts out how to set aside some memory to store the integer.

### int a[50];
- Sets aside enough storage for 50 ints and sets the name a to point to the first element.
- Static storage - the storage is allocated by the compiler before the program is run.

**Use pointers and the malloc function for dynamic storage**
### ptr=malloc(size);
- Reserves size bytes of storage and sets the pointer ptr to point to the start of it.
- This sounds excessively primitive - who wants a few bytes of storage and a pointer to it?
- Use the sizeof function to allocate storage in multiples of a given type.
- e.g. sizeof(int) returns a number that specifies the number of bytes needed to store an int.
  - Using sizeof you can allocate storage using malloc as: ptr = malloc(sizeof(int) * N) where N is the number of ints you want to create. The only problem is what does ptr point at? The compiler needs to know what the pointer points at so that it can do pointer arithmetic correctly. In other words, the compiler can only interpret ptr++ as an instruction to move on to the next int if it knows that the ptr is a pointer to an int. This works as long as you define the ptr to be a pointer to the type of variable that you want to work with. Unfortunately this raises the question of how malloc knows what the type of the pointer variable is, which it doesn't.
  - To solve this problem you can use a TYPE cast. This C play on words is a mechanism to force a value to a specific type. All you have to do is write the TYPE specifier in brackets before the value. So: ptr = (* int) malloc(sizeof(int) * N) forces the value returned by malloc to be a pointer to int. Now you can see how a simple idea ends up looking complicated. OK, so now we can acquire some memory while the program is running, but how can we use it? There are some simple ways of using it and some very subtle mistakes that you can make in trying to use it! For example, suppose during a program you suddenly decide that you need an int array with 50 elements. You didn't know this before the program started, perhaps because the information has just been typed in by the user. The easiest solution is to use: int * ptr; and then later on: ptr = (* int) malloc(sizeof(int) * N) where N is the number of elements that you need. After this definition you can use ptr as if it was a conventional array. For example: ptr[i] is the ith element of the array. The trap waiting for you to make a mistake is when you need a few more elements of the array. You can't simply use malloc again to get the extra elements because the block of memory that the next malloc allocates isn't necessarily next to the last lot. In other words, it might not simply tag on to the end of the first array and any assumption that it does might end in the program simply overwriting areas of memory that it doesn't own.
  - Another fun error that you are not protected against is losing an area of memory. If you use malloc to reserve memory it is vital that you don't lose the pointer to it. If you do then that particular chunk of memory isn't available for your program to use until it is restarted. This is also known as "memory leak". To prevent memory leaks in your program, you should free the reserved memory using free(ptr) once you're done using that chunk of memory.

## Differences between Stack and Heap
### Stack
- Stack is used for static memory allocation.
- Stored in the computer's RAM.
- Variables allocated on the stack are stored directly to the memory and access to this memory is very fast, and it's allocation is dealt with when the program is compiled. When a function or a method calls another function which in turns calls another function etc., the execution of all those functions remains suspended until the very last function returns its value. The stack is always reserved in a LIFO(Last in first out) order, the most recently reserved block is always the next block to be freed. This makes it really simple to keep track of the stack, freeing a block from the stack is nothing more than adjusting one pointer.
- You can use the stack if you know exactly how much data you need to allocate before compile time and it is not too big.

### Heap
- Heap is used for dynamic memory allocation.
- Stored in the computer's RAM.
- Variables allocated on the heap have their memory allocated at run time and accessing this memory is a bit slower, but the heap size is only limited by the size of virtual memory. Element of the heap have no dependencies with each other and can always be accessed randomly at any time. You can allocate a block at any time and free it at any time. This makes it much more complex to keep track of which parts of the heap are allocated or free at any given time.
- You can use heap if you don't know exactly how much data you will need at run-time or if you need to allocate a lot of data.

**In a multi-threaded situation each thread will have its own completely independent stack but they will share the heap. Stack is thread specific and Heap is application specific. The stack is important to consider in exception handling and thread executions.**

#### Short Summary
- In the context of Operating Systems, stack and heap are the two sections of the memory layout of a process. The stack is used to keep track of variables/parameters local to a function in a program. Whenever you call a new function, a new stack frame is pushed to the stack with parameters and variables local to that function. When that function returns, the stack frame is popped out and the context switches back to the previous function (the caller).

- A heap is a kind of a global memory pool. A function can allocate memory on the heap if it wants the data to live longer than the function itself. Objects allocated on the heap are accessible to all the functions, given they have the reference/address of the object to access it. In C, you can allocate memory on the heap using the malloc(3) family of functions.

## Threads
- Thread: a minimum CPU resources needed for executing a program.
- Threads cannot speed up execution of code. They do not make the computer run faster. All they can do is increase the efficiency of the computer by using time that would otherwise be wasted. In certain types of processing this optimization can increase efficiency and decrease running time.

## Lock, Mutex, and Semaphore
### Simplified
- A **lock** allows only one thread to enter the part that's locked and the lock is not shared with any other processes.
- A **mutex** is the same as a lock but it can be system wide (shared by multiple processes).
- A **semaphore** does the same as a mutex but allows x number of threads to enter.
**You also have read/write locks that allows either unlimited number of readers or 1 writer at any given time.**

### In-depth
- Critical Section: User object used for allowing the execution of just one active thread from many others within one process. The other non selected threads are put to sleep. [No interprocess capability, very primitive object].
- Mutex Semaphore (aka **Mutex**): Kernel object used for allowing the execution of just one active thread from many others, among different processes. The other non selected threads are put to sleep. This object supports thread ownership, thread termination notification, recursion (multiple 'acquire' calls from same thread) and 'priority inversion avoidance'. [Interprocess capability, very safe to use, a kind of 'high level' synchronization object].
- Counting Semaphore (aka **Semaphore**): Kernel object used for allowing the execution of a group of active threads from many others. The other non selected threads are put to sleep.[Interprocess capability however not very safe to use because it lacks following 'mutex' attributes: thread termination notification, recursion?, 'priority inversion avoidance'?, etc].
- **Spinlocks**: A lock which uses busy waiting. (The acquiring of the lock is made by xchg or similar atomic operations). [No thread sleeping, mostly used at kernel level only. Inefficient for User level code].
  - Critical Region: A region of memory shared by 2 or more processes.
  - Lock: A variable whose value allows or denies the entrance to a 'critical region'. (It could be implemented as a simple 'boolean flag').
  - Busy waiting: Continuously testing of a variable until some value appears.

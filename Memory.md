# Pointers and Memory Allocation

## Pointers
- In computer science, a pointer is a programming language object, whose value refers to (or "points to") another value stored elsewhere in the computer memory using its memory address.
- A pointer references a location in memory, and obtaining the value stored at that location is known as dereferencing the pointer.

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
  - Another fun error that you are not protected against is losing an area of memory. If you use malloc to reserve memory it is vital that you don't lose the pointer to it. If you do then that particular chunk of memory isn't available for your program to use until it is restarted.

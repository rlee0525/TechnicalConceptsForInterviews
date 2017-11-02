# Object-Oriented Programming Basics

## What is OOP?
- **Object-oriented programming** is a programming language model organized around objects rather than actions and data rather than logic.

## Why OOP?
- Allows inheritance, reducing development time, easier development experience, and more accurate coding and robustness with reusability. Also data hiding prevents instance of certain class from accidentally accessing other data - safe development. The encapsulation allows programmer to define the class with many functions and characteristics and only few functions are exposed to the user.

## Key Concepts
### Abstraction
- Representing a complicated system in simplified model through information hiding and providing only the essential details to the users. (e.g. Media player abstracts the concepts of playing, pausing, fast-forwarding and so on. As a user, you can use this to operate the device.)

### Encapsulation
- Binding of data and related functions into a unit such as class (collection of objects with characteristics in common). It makes the concept of data hiding possible and protects from the “side-effects” of unwanted changes. It prevents clients from seeing its inside view, where the behavior of the abstraction is implemented. (e.g. VCR implemented this interface and encapsulated the details of the mechanical drives and tapes.)
- When a new implementation of a media player arrives (DVD player for example), it can replace the implementation encapsulated in the media player and users can continue to use it just as they did with their VCR. (same operations such as play, pause, etc.)
- Concept of information hiding through abstraction. It allows for implementation details to change without the users having to know and promotes low coupling of code.

### Inheritance
- When a class of objects is defined, any subclass that is defined can inherit the definitions of one or more general classes. An object in a subclass does not need to carry its own definition of data and methods that are generic to its superclass. Faster development process codes are reusable. Can be overridden by the use of the public, private, and protected keywords.
- **Inheritance** represent an “is-a” relationship. (e.g. a dog or a cat “is-a” pet)
- **Interface** represent “is” relationship, an addition features of a class. (e.g. Foo “is” disposable)

### Polymorphism
- “Many forms”. Allows us to have the same method take on different meanings depending on which class is instantiated. Shape class with circle and square. Calling area will return the correct results for both subclasses. **Important because it allows objects of different classes to share the same external interface while giving flexibility to change it for itself.**  (Shadowing, or overriding inheritance, is an example as this is done by implementing same method in its subclasses to override superclass’ same method)

### Dynamic Binding
- Binding means connecting one program to another program that is to be executed in reply to the call, AKA late binding. Makes polymorphism possible by executing the code under the present reference of object.

### Delegation (Composition) 
- “has a” relationship, meaning the use of instance variables that are references to other objects.

### Genericity
- Allows declaration of variables without specifying exact data type and the compiler will identify them at run time.

### Cardinality
- The degree of relationship. The number of occurrences in one associated to the number of occurrences in another.
  - One-to-one
  - One-to-many
  - Many-to-many

### Multiplicity
- Interval of positive integers to specify allowable cardinalities.

### Modularity
- Decomposing a program into a set of modules to reduce the overall complexity of the program. (Set of cohesive and loosely coupled modules.)

### Coupling 
- A measure of how closely connected software modules are. Loosely coupled system comprises of components that have little or no knowledge of other separate components.

### Cohesion
- A measure of the strength of relationship between the class’ methods and data. High cohesion is associated with reusability, understandability, and robustness.
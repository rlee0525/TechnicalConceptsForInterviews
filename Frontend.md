# Frontend Interview Questions

## Basic Terms
### What is a DOM?
- Document Object Model <- browser makes this when page loads
- HTMl -> DOM by browser
- It is an API for valid HTML and XML
- We can use it to access, change, delete or add elements.
- It can be represented as a tree

### What is Ajax?
- Asynchronous Javascript and XML.
- Technique used to create better faster and more interactive web applications.
- Uses XHTML for content, CSS for presentation, !!DOM and JS!! for dynamic content display
- Used to make requests to server and updates the screen. User would not know anything was transmitted.
- Asynchronous. User can continue to use application during ongoing request.
- Data driven instead of page driven.
- Clicking not required, mouse movement is sufficient to trigger event.

### What is a closure?
- A closure is an inner function that has access to the outer function's variables.
- It is great for making private variables!
- Has access to variables in its own scope, to outer function's variables and global variables.
- They still have access to the outer function's variables even if the outer function returns.
- They store references to the outer function's variables, not the actual value.

### What is prototype in JavaScript?
- A prototype is an object and all JS object has a prototype.
- Everything is an object in JS and everything inherits from the Object prototype.
- Use hasOwnProperty so it doesn't check all the way up the prototype chain.
- All JS objects inherit properties and methods from their prototype.
- The standard way of creating an object prototype is using a constructor function. (Class)
- Changes to an object's prototype can be seen by all objects through prototype chaining.

### What is jQuery?
- It is a fast and concise JavaScript Library.
- It simplifies HTML document traversing, event handling, animating and Ajax.
- Lightweight. Write less to do the same thing in Vanilla JavaScript.
- JavaScript is a programming language; jQuery is a javascript library of convenience functions that simplify interaction with the DOM.

### What is a module in JS?
- A cluster of code. It is highly self-contained.
- They are easy to maintain because they are (ideally) not dependent with the other code.
- They are also used to help with name spacing. Another perk is that they can be reused.
- Modules are comparable to Classes in other languages.

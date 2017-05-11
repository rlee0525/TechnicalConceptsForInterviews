# Frontend Interview Questions - The Basics

> Contributor: Michael Mach

## Basic Terms

### What is a DOM?

- Document Object Model <- browser makes this when page loads
- HTML -> DOM by browser
- It is an API for valid HTML and XML
- We can use it to access, change, delete or add elements.
- It can be represented as a tree

### What is Ajax?

- Asynchronous Javascript and XML.
- Technique used to create better faster and more interactive web applications.
- Uses XHTML for content, CSS for presentation, **DOM and JS** for dynamic content display
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

### What are IIFEs?

- Immediately Invoked Function Expressions.
- An IIFE is an anonymous function that is created and then immediately invoked. It's not called from anywhere else (hence why it's anonymous), but runs just after being created.

## Basic Concepts

### Explain how 'this' works in JavaScript

- This refers to an object and it is usually used inside a function or a method.
- example: instead of saying Person.firstName, you can say this.firstName.
- Use bind, apply or call to fix 'this' when it is out of context.

### Explain why the following doesn't work as an IIFE: function foo(){ }();

- Surround it with parenthesis. `(foo(){})()`

### What's the difference between a variable that is: null, undefined or undeclared?

- undeclared variables don't even exist
- undefined variables exist, but don't have anything assigned to them (typeof)
- null variables exist and have null assigned to them (===)
- null is a primitive value, not an object. even though `typeof null` returns `object`

### What's a typical use case for anonymous functions?

- Typically used as callbacks.

### What's the difference between .call and .apply?

- `theFunction.apply(valueForThis, arrayOfArgs)` apply -> array
- `theFunction.call(valueForThis, arg1, arg2, ...)`

### Explain Function.prototype.bind

- `theFunction.bind(valueForThis)(arg1, arg2, ...)` (does not invoke function. params passed outside)
- Returns function with the modified `this`

### What is document.write

- The write() method is mostly used for testing: If it is used after an HTML document is fully loaded, it will delete all existing HTML.

### Explain "hoisting"

- Variables are defined at the top of the function.
- In JavaScript, a variable can be declared after it has been used.
- In other words; a variable can be used before it has been declared.
- `var` is hoisted. `const` and `let` are not.

### Describe event bubbling.

- Event that happens in the inner most element would cause a chain reaction moving upward.
- Events above the target element would also fire.
- The target would stay the same but `this` will change to reflect the firing element.

### Explain event delegation.

- Using event propagation (bubbling) to handle events at a higher level in the DOM.
- Adding a single event handler to the parent to avoid adding event handlers to every children.

### What's the difference between an "attribute" and a "property"?

- JS DOM objects have properties.
- Attributes are in the HTML itself. Similar to Props but not as good.
- Attr is always a string while props can be anything.
- Attr always returns the default value even though the value was changed.

### Difference between document load event and document DOMContentLoaded event?

- The DOMContentLoaded event will fire as soon as the DOM hierarchy has been fully constructed, the load event will do it when all the images and sub-frames have finished loading.

### What is the difference between == and ===?

- `==` will try to convert one side to the same type as the other.
- `===` will strictly compare the two without any conversions.

### Make this work: `duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]`

```javascript
let duplicate = function(arr) {
  return arr.concat(arr);
}
```

### Why is it called a Ternary expression, what does the word "Ternary" indicate?

- ex: `(1 === 1) ? true : false`
- So a ternary operand accepts three parameters.

```javascript
if(conditional) {     // one
  // truethy_block    // two
} else {
  // falsey_block     // three
}
```

### What is `"use strict"`? what are the advantages and disadvantages to using it?

- Used to restrict functions or program.
- This strict context prevents certain actions from being taken and throws more exceptions.
- It catches some common coding bloopers, throwing exceptions.
- It prevents, or throws errors, when relatively "unsafe" actions are taken (such as gaining access to the global object).
- It disables features that are confusing or poorly thought out.

### make this work

```javascript
add(2, 5); // 7
add(2)(5); // 7
```

```javascript
let add = function(x, ...xArgs) {
  let total = x;
  if (xArgs.length > 0) {
    xArgs.forEach((el) => {
      total += el;
    });
    return total;
  } else {
    return function(y) {
      return total += y;
    };
  }
};
```

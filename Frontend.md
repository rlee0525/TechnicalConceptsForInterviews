# Frontend Interview Questions

> Contributor: Michael Mach

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

### What are IIFEs?
- Immediately Invoked Function Expressions.
- An IIFE is an anonymous function that is created and then immediately invoked. It's not called from anywhere else (hence why it's anonymous), but runs just after being created.

## Basic Concepts
### Explain how 'this' works in JavaScript
- This refers to an object and it is usually used inside a function or a method.
- example: instead of saying Person.firstName, you can say this.firstName.
- Use bind, apply or call to fix 'this' when it is out of context.

### Explain why the following doesn't work as an IIFE: function foo(){ }();
- Surround it with parenthesis.

### What's the difference between a variable that is: null, undefined or undeclared?
- undeclared variables don't even exist
- undefined variables exist, but don't have anything assigned to them (typeof)
- null variables exist and have null assigned to them (===)

### What's a typical use case for anonymous functions?
- Typically used as callbacks.

### What's the difference between .call and .apply?
- theFunction.apply(valueForThis, arrayOfArgs) apply -> array
- theFunction.call(valueForThis, arg1, arg2, ...)

### Explain Function.prototype.bind
- theFunction.bind(valueForThis)(arg1, arg2, ...) (does not invoke function. params passed outside)
- Returns function with the modified 'this'

### What is document.write
- The write() method is mostly used for testing: If it is used after an HTML document is fully loaded, it will delete all existing HTML.

### Explain "hoisting"
- Variables are defined at the top of the function.
- In JavaScript, a variable can be declared after it has been used.
- In other words; a variable can be used before it has been declared.

### Describe event bubbling.
- Event that happen in the inner most element would cause a chain reaction moving upward.
- Events above the target element would also fire.
- The target would stay the same but 'this' will change to reflect the firing element.

### Explain event delegation.
- Adding a single event handler to the parent to avoid adding event handlers to every children.

### What's the difference between an "attribute" and a "property"?
- JS DOM objects have properties.
- Attributes are in the HTML itself. Similar to Props but not as good.
- Attr is always a string while props can be anything.
- Attr always returns the default value even though the value was changed.

### Difference between document load event and document DOMContentLoaded event?
- The DOMContentLoaded event will fire as soon as the DOM hierarchy has been fully constructed, the load event will do it when all the images and sub-frames have finished loading.

### What is the difference between == and ===?
- == will try to convert one side to the same type as the other.
- === will strictly compare the two without any conversions.

### Make this work: duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```JavaScript
let duplicate = function(arr) {
  return arr.concat(arr);
}
```

### Why is it called a Ternary expression, what does the word "Ternary" indicate?
- ex: (1 === 1) ? true : false
- So a ternary operand accepts three parameters.

```JavaScript
if(conditional) { // one
  truethy_block // two
} else {
  falsey_block // three
}
```

### What is "use strict"? what are the advantages and disadvantages to using it?
- Used to restrict functions or program.
- This strict context prevents certain actions from being taken and throws more exceptions.
- It catches some common coding bloopers, throwing exceptions.
- It prevents, or throws errors, when relatively "unsafe" actions are taken (such as gaining access to the global object).
- It disables features that are confusing or poorly thought out.

### make this work
#### add(2, 5); // 7
#### add(2)(5); // 7

```Javascript
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

### What does a doctype do?
- It tells the browser what version the language the page is written in.
- It tells your browser how to render your document.

### What's the difference between full standards mode, almost standards mode and quirks mode?
- Quirks mode - for older browsers.
- Full standards - the behavior of the website is exactly as described by HTML/CSS specifications.
- Almost standards is in between.

### What's the difference between HTML and XHTML?
- XHTML is identical to HTML but more strict. Does not allow for mistakes.
Stricter error handling.

### What are data- attributes good for?
- The data-* attributes is used to store custom data private to the page or application.
- The data-* attributes gives us the ability to embed custom data attributes on all HTML elements.
- <p data-name="myName">Hi</p>

### Consider HTML5 as an open web platform. What are the building blocks of HTML5?
- more semantic text markup
- new form elements
- video and audio
- new javascript API
- canvas and SVG
- new communication API
- geolocation API
- web worker API
- new data storage

### Describe the difference between a cookie, sessionStorage and localStorage.
- localStorage - stores data clientside. data persists until user clears browser cache/local stored data. (window)
- sessionStorage - data is also stored clientside but is destroyed when browser closes.
- cookie - stored data that is usually accessed server-side. It can expire.

### What is the difference between classes and IDs in CSS?
- ID and Classes can both be used to select an element to modify using CSS.
- ID is more specific.
- Ideally you would want 1 ID and multiple classes.

### What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
- CSS resets wipes out all styling. Normalizing tries to make the styling more consistent across all browsers.
- Using css resets and normalizing gives you more control over how items are displayed.
- Prevents unexpected behaviors.

### Describe Floats and how they work.
- It pushes the element to the side you've chosen.
- All other elements will take up the space (if float left, other elements will take up the right side)

### Describe z-index and how stacking context is formed.
- Higher z-index will put item in the front.
- Lower z-index will put the item further behind.

### Describe BFC(Block Formatting Context) and how it works.
- Grouping elements in boxes.
- Block formatting contexts:
- Stop margins from collapsing
- Restrain floats
- Contain floats

### What are the various clearing techniques and which is appropriate for what context?
- Clear (none, left, right or both)
- Float
- Flex

### Explain CSS sprites, and how you would implement them on a page or site.
- Sprite is one big image containing several frames (smaller images).
- You write code to cycle through the frames. (change the position)

### What are your favorite image replacement techniques and which do you use when?
- I like to simply change background image.

### What are the different ways to visually hide content (and make it available only for screen readers)?
- Display: none; (stuff shifts)
- visibility: hidden; (stuff doesn't shift)

### Have you ever used a grid system, and if so, what do you prefer?
- I like flexbox;
- I can also manually create the grid system using float.
- Bootstrap? also has one that you can use.

### Have you used or implemented media queries or mobile specific layouts/CSS?
- I use min-width and max-width.
- Use dev tools to set the screen small.

### Any familiarity with styling SVG?
- Scalable vector graphics.

### What are the advantages/disadvantages of using CSS preprocessors? (SASS, Compass, Stylus, LESS)
- SASS lets you store values in variables. This makes it easier for others to read and to modify later.

### How would you implement a web design comp that uses non-standard fonts?
- One way is to use a font generator.
- Another way would be to find a similar font.

### Explain how a browser determines what elements match a CSS selector?
- There is specificity
- (Most specificity) inline style -> ID -> class -> elements (Least specificity)

### List as many values for the display property that you can remember.
- display: block, inline, inline-block, list-item, none, table, table-cell, table-row, flex

### What's the difference between inline and inline-block?
- block - takes up the entire width=
- inline - takes up only as much as necessary. Cannot have a width and height set
- inline-block - takes up the entire width. Will adjust to height. Can wrap.

### What's the difference between a relative, fixed, absolute and statically positioned element?
- relative - top: 10px will shift down. Allows using z-index.
- fixed - Stays with the view. Follows you around.
- absolute - place any page element exactly where you want it. Not affected by other elements. (not flexible)
- static - Default

### Explain the box model.
- The CSS box model is essentially a box that wraps around every HTML element. It consists of: margins, borders, padding, and the actual content.
- Content - The content of the box, where text and images appear
- Padding - Clears an area around the content. The padding is transparent
- Border - A border that goes around the padding and content
- Margin - Clears an area outside the border. The margin is transparent
- The box model allows us to add a border around elements, and to define space between elements.^https://o.quizlet.com/i/-qntesLUKZF3Rdq4Bjn0ug_m.jpg

### Explain the CSS position property.
- The position property specifies the type of positioning method used for an element (static, relative, absolute or fixed).
- Static: Default value. Elements render in order, as they appear in the document flow
- Relative: The element is positioned relative to its normal position, so "left:20px" adds 20 pixels to the element's LEFT position
- Absolute: The element is positioned relative to its first positioned (not static) ancestor element
- Fixed: The element is positioned relative to the browser window
- Initial: Sets this property to its default value.
- Inherit: Inherits this property from its parent element.

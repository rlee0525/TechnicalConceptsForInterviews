# Frontend Interview Questions - HTML / CSS

> Contributor: Daniel Chang

## HTML Questions

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

### Describe the difference between `<script>`, `<script async>` and `<script defer>`.
- `<script>`: HTML file will be parsed until the script file is hit. Parsing will stop to fetch the file and the script will be executed before parsing continues.
- `<script async>`: Downloads the file during HMTL parsing. HTML parsing will pause to execute file when finished downloading.
- `<script defer>`: Downloads file during HTML parsing. Will only execute after parsing is completed. Also guaranteed to execute in order of document appearance.
- Read more here: http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html

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

### Explain the box model.
- The CSS box model is essentially a box that wraps around every HTML element. It consists of: margins, borders, padding, and the actual content.
- Content - The content of the box, where text and images appear
- Padding - Clears an area around the content. The padding is transparent
- Border - A border that goes around the padding and content
- Margin - Clears an area outside the border. The margin is transparent
- The box model allows us to add a border around elements, and to define space between elements.^https://o.quizlet.com/i/-qntesLUKZF3Rdq4Bjn0ug_m.jpg

### Explain the CSS position property.
- The position property specifies the type of positioning method used for an element.
- Static: Default value. Elements render in order, as they appear in the document flow
- Relative: The element is positioned relative to its normal position, so "left:20px" adds 20 pixels to the element's LEFT position
- Absolute: The element is positioned relative to its first positioned (not static) ancestor element
- Fixed: The element is positioned relative to the browser window
- Initial: Sets this property to its default value.
- Inherit: Inherits this property from its parent element.

## Packaging
Packaging is important because we pretty much always want to have the fastest loading time. In order to do that we initially load the bare minimum and continue on demand. Also allows for development of new features without slowing down load time.
- Angular: Very limited ability to do so (mostly html templates)
- React: Doesn't care. Plain JS allows use of requirejs and lazy load code. Webpack is also another viable solution.
`Winner: React`

## Learning Curve
- React: Easy to learn. Takes some time to get used to one-way flow, but is very clear once understood. Only has a few lifecycle methods and are self explanatory.
- Angular: Lifecycle is complex. Compile and link are not intuitive, and specific cases can be confusing (recursion in compile, collisions between directives).
`Winner: React`

## Abstraction

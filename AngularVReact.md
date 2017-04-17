# Introduction
Comparison points on Angular vs. React. Angular is full MVC while React is simply V.

## Learning Curve
**React:** Easy to learn. Takes some time to get used to one-way flow, but is very clear once understood. Only has a few lifecycle methods and are self explanatory.

**Angular:** Lifecycle is complex. Compile and link are not intuitive, and specific cases can be confusing (recursion in compile, collisions between directives).

**Winner: React**

## Packaging
Packaging is important because we pretty much always want to have the fastest loading time. In order to do that we initially load the bare minimum and continue on demand. Also allows for development of new features without slowing down load time.

**React:** Doesn't care. Plain JS allows use of requirejs and lazy load code. Webpack is also another viable solution.

**Angular:** Very limited ability to do so (mostly html templates)

**Winner: React**

## Abstraction
Abstraction allows for fast development and hides unnecessary details for developers using the library.

**React:** Less flexible in parts (not being able to add attributes to HTML tags, or a single tag for component). However, it is solved by React's implementation of mixins (that allow overlap only on lifecycle methods, and have a predictable execution order) and the fact that it doesn't leak.

**Angular:** Leaky. Need to actually understand the underlying model to work with it. Often times need to debug internals of Angular when debugging code.

**Winner: React**

## Model Complexity
How data model is structured that is later represented in the view.

**React:** Gives freedom to choose without penalizing performance. Outcome depends on coding skill.

**Angular:** Performance is sensitive when dealing with scope due to copy-n-compare. Thus, large models are not able to be used. Pros: Code becomes simpler and easier to test. Cons: Forced to break down stuff you'd normally use and rebuild it back up (EX: server requests)

**Winner: Tie**

## Debugging
**React:** Two main scenarios: 1) Updating model/doing other actions as a result of user events. 2) One-way rendering flow which is always the same. This means that there are fewer places to look for bugs and the stack traces have clear distinctions between React code and your own code. React has less magic as well and is concentrated in one place - vDOM.

In regards to HTML, it is hard to back trace your code. Even when using jsx it is hard to compare as `if` and `repeat` control flows break the HTML fragments into small chunks of templates.

**Angular:** Event driven. Easy to write, harder to debug. Angular provides constructs that are logical (called `services`) and makes code easier to test and debug. But since there is so much magic behind Angular's directives, the two main options are to rewrite the code in a different way or debug Angular's code.

HTML-wise, Angular's results closely resemble the HTML template.

**Winner JS: React**

**Winner HTML: Angular**

## Binding
**React:** Only provides syntactic sugar for binding, `valueLink`: a single attribute for both `value` and `onChange` properties. If you understand it well it can solve all your binding needs.

**Angular:** Can only bind to scope. So for complex scenarios, will need to have an intermediate model in the way and will also need to deal with digest cycles and explicit watches.

**Winner: React**

## Performance Tweaking
**React:** Simple to control performance. If you implement shouldComponentUpdate, you are allowed to choose which comparison - model or presentation. If you have a small model, leave the comparison to React on the vDOM. If model is complex, or you are creating a lot of DOM, you can optimize it by a custom implementation of this function, where you can devise your own mechanisms for dirty-checking that can be very efficient.

**Angular:** In Angular, you need to start counting scopes, and in some cases, you just have to implement the internals of a component in pure js and wrap it in Angular for convenience. The evidence of this is the amount of articles you can find about Angular performance tweaking...

**Winner: React**

## Code re-use
**React:** Allows freedom to manage the way you like.

**Angular:** Already has a lot of stuff out there. However, not trivial to use Angular libraries for more than one provider due to namespace and priority collisions.

**Winner: Tie**

## Templating
Most important as 80% of writing an online service is UI. When writing a repeater often times it looks backwards.
**React:**

```JavaScript
var createItem = function(itemText) {
    return <li>{itemText}</li>;
};
return <ul>{this.props.items.map(createItem)}</ul>;
```

**Angular:**

```JavaScript
<ul>
    <li ng-repeat="item in items">{{item}}</li>
</ul>
```
**Winner: Angular**

# Conclusion
Obviously both frameworks have their pros and cons.

Angular is more opinionated and can be great if it matches your development scenario. React is more simple and has more freedom, but lacks the declarative power of Angular. So when using react-templates, it achieves a lot of capabilities as Angular, but not as messy.

For developers with complex projects, strong in vanillaJS, care about file packaging or want to use a framework with other libraries, React & react-templates is a clear winner.

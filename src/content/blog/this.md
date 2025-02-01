---
title: this
description: The value of `this` is determined by how a function is called, not where it's defined. This is what the ECMAScript spec calls "runtime binding." Think of it like a game of hot potato whoever's holding the potato (calling the function) determines what `this` becomes.
pubDate: "Feb 01 2025"
---

The value of `this` is determined by how a function is called, not where it's defined. This is what the ECMAScript spec calls "runtime binding." Think of it like a game of hot potato whoever's holding the potato (calling the function) determines what `this` becomes.

Here's a tricky example that often confuses us developers.

```javascript
const user = {
  name: "John",
  greet() {
    const sayHi = () => {
      console.log(`Hi, ${this.name}!`);
    };
    setTimeout(sayHi, 1000);
  },
};

user.greet(); // After 1 second: "Hi, John!"
```

In this case, the arrow function `sayHi` captures `this` from its surrounding scope. If we had used a regular function instead, `this` would've been the global object (or undefined in strict mode), and `this.name` would've been undefined. aha! another thing to look out for when using `this`, welcome to js!

### Arrow Functions and "this"

Arrow functions are special. Unlike regular functions, they don't have their own `this`. Instead, they inherit `this` from their parent scope. It's like they're saying "whatever `this` meant where I was created, that's what I'll use."

Here's something that might surprise you.

```javascript
const obj = {
  value: 42,
  getValue: () => {
    console.log(this.value);
  },
};

obj.getValue(); // undefined
```

Even though `getValue` is a method on `obj`, the arrow function doesn't get `this` from the object. Instead, it inherits `this` from where the arrow function was created (probably the global scope or module scope), againg welcome to js!

### The Spec Says...

According to the ECMAScript specification, when a function is called, an execution context is created. This context includes a `ThisBinding` component that determines what `this` will be inside the function. The rules for setting `ThisBinding` are quite specific:

1. For constructor functions (called with `new`), `this` is the newly created object
2. For methods, `this` is the object that owns the method
3. For arrow functions, `this` is inherited from the enclosing scope
4. For plain function calls, `this` is either the global object or undefined (in strict mode)

Understanding these rules helps you predict what `this` will be in any situation.

### A Real World Example... i think

Here's a pattern you might see in real code:

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
    // Using an arrow function to preserve 'this'
    this.emit = (event, ...args) => {
      const handlers = this.events[event] || [];
      handlers.forEach((handler) => handler.apply(this, args));
    };
  }

  on(event, handler) {
    this.events[event] = this.events[event] || [];
    this.events[event].push(handler);
  }
}

const emitter = new EventEmitter();
emitter.on("test", function () {
  console.log(this === emitter); // true
});
emitter.emit("test");
```

In this example, we use both arrow functions and regular functions strategically. The arrow function for `emit` ensures `this` always refers to the EventEmitter instance, while the handler function gets its `this` bound to the emitter through `apply`.

Remember, `this` isn't magic it's just a parameter that JavaScript passes to functions in a special way.

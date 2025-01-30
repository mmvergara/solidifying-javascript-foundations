---
title: IIFE
description: Immediately Invoked Function Expressions (IIFE) are functions that run as soon as they're defined. They're a great way to create isolated scopes in JavaScript and avoid polluting the global scope.
pubDate: "Jan 31 2025"
---

Have you ever seen code like this?

```javascript
(function () {
  console.log("Hello from an IIFE!");
})();
```

That’s an IIFE, or **Immediately Invoked Function Expression**. It’s a function that runs as soon as it’s defined. They're a great way to create isolated scopes in JavaScript and avoid polluting the global scope.

The syntax might look a little weird at first, but it’s actually pretty simple. You define a function, wrap it in parentheses, and then add `()` at the end to call it immediately:

```javascript
(function () {
  // Your code here
})();
```

The outer parentheses are crucial—they turn the function into an expression instead of a declaration. This is important because JavaScript doesn’t let you immediately invoke a function declaration. For example, this would throw an error:

```javascript
function() {
  console.log("This won't work!");
}();
```

By wrapping the function in parentheses, you’re telling JavaScript, “Hey, treat this as an expression, not a declaration.”

---

### Why?

IIFEs were super popular back in the day (think pre-ES6) because they helped avoid **polluting the global scope**. IIFEs let you create a temporary scope where variables could live without leaking into the global space.

Here’s an example:

```javascript
(function () {
  var secret = "I'm hidden!";
  console.log(secret); // "I'm hidden!"
})();

console.log(secret); // ReferenceError: secret is not defined
```

In this example, `secret` is only accessible inside the IIFE. Once the IIFE runs, `secret` is gone. This is super useful for keeping your code clean and avoiding conflicts with other scripts.

Before block-scoped variables (`let` and `const`), the only way to create a new scope was with a function. To think that IIFE's are only popular because we didn't have block-scoped variables is kinda sad lol.

---

Let’s say you’re building a module that needs to initialize some data but doesn’t want to expose it to the outside world. You could use an IIFE to encapsulate that logic:

```javascript
const counter = (function () {
  let count = 0;

  return {
    increment: function () {
      count++;
      console.log(count);
    },
    reset: function () {
      count = 0;
      console.log("Counter reset!");
    },
  };
})();

counter.increment(); // 1
counter.increment(); // 2
counter.reset(); // "Counter reset!"
```

Here, the `count` variable is private. The only way to interact with it is through the `increment` and `reset` methods returned by the IIFE.

---

The ECMAScript spec doesn’t explicitly mention IIFEs, but it does explain how function expressions work. According to the spec, a function expression is evaluated as part of the larger expression it’s contained in. When you add `()` at the end, you’re invoking that function expression immediately.

With the introduction of block-scoped variables (`let` and `const`) and modules in ES6, IIFEs aren’t as necessary as they used to be. But they’re still handy in certain situations, like when you need to create a quick, isolated scope or when you’re working in an older codebase.

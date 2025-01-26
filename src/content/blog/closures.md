---
title: Closures
description: Closures can feel like a complex topic, but they’re actually quite straightforward once you get the hang of them. Imagine closures as a backpack that a function carries with it, containing all the variables it had access to when it was created. Let's unpack this concept with a simple example.
pubDate: "Jun 26 2025"
---

Closures can feel like a complex topic, but they’re actually quite straightforward once you get the hang of them. Imagine closures as a backpack that a function carries with it, containing all the variables it had access to when it was created. Let's unpack this concept with a simple example.

#### The Backpack in Action

Here’s some code:

```javascript
function outerFunction() {
  let count = 0; // This is the backpack

  return function innerFunction() {
    count++;
    console.log(count);
  };
}

const counter = outerFunction();
counter(); // Logs: 1
counter(); // Logs: 2
```

What’s happening here?

When `outerFunction` runs, it creates `count` and then returns `innerFunction`. But here’s the magic: `innerFunction` remembers `count`, even after `outerFunction` has finished running. This “memory” is the closure.

Think of closures as a vending machine. When you deposit a coin (call `outerFunction`), the machine remembers your balance (closure) even if you walk away. When you return, the machine still knows how much you’ve deposited.

The ECMAScript specification describes closures as a combination of a function and its **lexical environment** (the variables available when the function was defined). This environment travels with the function wherever it’s used.

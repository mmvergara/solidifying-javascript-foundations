---
title: Currying
description: Currying is a functional programming technique that transforms a function with multiple arguments into a sequence of functions, each taking a single argument. Let’s explore how it works, why it’s useful, and how to apply it in real-world scenarios.
pubDate: "January 29 2025"
---

> before tackling this topic make sure you have a good understanding about closures and higher order functions in general.

Currying is the process of transforming a function that takes multiple arguments into a sequence of functions, each taking **one argument at a time**. This allows you to create more specialized and reusable functions.

Think of currying like ordering a custom pizza: instead of specifying all your toppings at once, you add them one by one, building up to the final result.

Take a look at this simple example:

```javascript
// Non-curried function
function multiply(a, b, c) {
  return a * b * c;
}

console.log(multiply(2, 3, 4)); // 24

// Curried version
function curriedMultiply(a) {
  return function (b) {
    return function (c) {
      return a * b * c;
    };
  };
}

console.log(curriedMultiply(2)(3)(4)); // 24
```

In the curried version, `curriedMultiply` takes one argument at a time, returning a new function until all arguments are provided. This might seem like extra work, but it opens up a world of flexibility.

---

### What about the use case?

Currying is like having a toolkit where each tool does one thing really well. It allows you to create **partially applied functions**—functions that are pre-loaded with some arguments and ready to take the rest later. This is especially useful for reusability and composition.

take a look:

```javascript
// Curried function to calculate shipping costs
function calculateShippingCost(baseCost) {
  return function (weight) {
    return function (distance) {
      return baseCost + weight * 2 + distance * 0.5;
    };
  };
}

const standardShipping = calculateShippingCost(10); // Base cost of $10
const heavyItemShipping = standardShipping(20); // Weight of 20kg

console.log(heavyItemShipping(100)); // Shipping cost for 100km: 10 + (20 * 2) + (100 * 0.5) = 80
```

`calculateShippingCost` is a curried function that lets you create specialized shipping calculators. You can reuse `standardShipping` and `heavyItemShipping` for different scenarios.

---

Another example to solidify stuff.

```javascript
// Curried function to create a logger
function createLogger(prefix) {
  return function (message) {
    return function (timestamp) {
      return `[${timestamp}] ${prefix}: ${message}`;
    };
  };
}

const errorLogger = createLogger("ERROR");
const warnLogger = createLogger("WARN");

console.log(errorLogger("File not found")(new Date().toISOString()));
// [2025-10-15T12:00:00Z] ERROR: File not found

console.log(warnLogger("Low disk space")(new Date().toISOString()));
// [2025-10-15T12:00:00Z] WARN: Low disk space
```

In this example, `createLogger` is a curried function that generates specialized loggers. You can create `errorLogger` and `warnLogger` with different prefixes, making your logging system more flexible and reusable.

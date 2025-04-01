---
title: Strict Equality (===)
description: The strict equality operator (===) compares two values for equality without performing type coercion. It returns true if the values are equal and false if they are not.
pubDate: "Jun 26 2025"
---

The behavior of the strict equality operator (`===`) is defined in the **ECMAScript specification** under the section [**Strict Equality Comparison**](https://tc39.es/ecma262/#sec-strict-equality-comparison). If you wan't to understand how `===` works i suggest you first understand how the [**Abstract Equality Comparison**](https://tc39.es/ecma262/#sec-abstract-equality-comparison) works first. It's actually just a `==` but without type coercion.

### The Algorithm for `===`

When you use `===`, JavaScript follows this algorithm to determine if two values are equal.

1. **Check if the types are the same:**

   - If the types of the two values are different, return `false`.
   - If the types are the same, proceed to the next step.

2. **Compare the values based on their type:**

   - **Numbers:**

     - If both values are `NaN`, return `false` (yes, `NaN === NaN` is `false`!).
     - If the numbers have the same numeric value, return `true`.
     - If one value is `+0` and the other is `-0`, return `true` (they are considered equal).

   - **Strings:**

     - If both strings have the same sequence of characters, return `true`.
     - Otherwise, return `false`.

   - **Booleans:**

     - If both values are `true` or both are `false`, return `true`.
     - Otherwise, return `false`.

   - **Objects (including arrays and functions):**

     - If both values reference the **same object in memory**, return `true`.
     - Otherwise, return `false`.

   - **`null` and `undefined`:**
     - `null === null` is `true`.
     - `undefined === undefined` is `true`.
     - `null === undefined` is `false` (they are different types).

---

### Why `NaN === NaN` is `false`

This is a common point of confusion. According to the spec, `NaN` (Not-a-Number) is defined as not equal to itself. This is because `NaN` represents an **invalid or undefined numeric result**, and it doesn't make sense to compare two invalid results as equal.

For example:

```javascript
NaN === NaN; // false
```

If you need to check if a value is `NaN`, use `Number.isNaN()` or `Object.is()`:

```javascript
Number.isNaN(NaN); // true
Object.is(NaN, NaN); // true
```

---

### Why `+0 === -0` is `true`

The spec treats `+0` and `-0` as equal because, in most cases, they behave the same way in mathematical operations. However, there are rare scenarios where they differ (e.g., `1 / +0` is `Infinity`, while `1 / -0` is `-Infinity`). If you need to distinguish between them, use `Object.is()`:

```javascript
Object.is(+0, -0); // false
```

---

### Objects and Reference Equality

When comparing objects (including arrays and functions), `===` checks if they reference the **same object in memory**. This is why two objects with identical contents are not considered equal:

```javascript
const obj1 = { name: "Alice" };
const obj2 = { name: "Alice" };
obj1 === obj2; // false (different objects)
```

But if two variables point to the same object, they are equal:

```javascript
const obj3 = obj1;
obj1 === obj3; // true (same object)
```

This is just my personal opinion but i think this is one way we can measure a developer's understanding of JavaScript by how well they predict the output of comparisons using `===` and `==` in different scenarios.

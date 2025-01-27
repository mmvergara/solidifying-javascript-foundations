---
title: NaN
description: NaN, or "Not-a-Number," is one of those JavaScript quirks that can trip you up if you're not paying attention. But here's the thing NaN isn't just a quirky value—it's a **sentinel value** that represents an _invalid number_. It's not the absence of a number, and it's definitely not zero. It's a specific, intentional signal that something went wrong in a numeric operation. (or i don't know, i didn't create the language)
pubDate: "Jun 27 2025"
---

NaN, or "Not-a-Number," is one of those JavaScript quirks that can trip you up if you're not paying attention. But here's the thing: NaN isn't just a quirky value—it's a **sentinel value** that represents an _invalid number_. It's not the absence of a number, and it's definitely not zero. It's a specific, intentional signal that something went wrong in a numeric operation. (or i don't know, i didn't create the language)

### What is NaN?

NaN comes from the **IEEE 754 spec**, which defines how numbers work in JavaScript. It's not a bug or a mistake—it's a deliberate part of the language. Think of NaN as a red flag that says, "Hey, this operation didn't make sense mathematically."

For example:

```javascript
console.log(Math.sqrt(-1)); // NaN
```

You can't take the square root of a negative number (at least not in real numbers), so JavaScript gives you NaN. It's like asking, "What's the color of happiness?"—it's a question that doesn't have a meaningful answer.

### NaN's Quirky Behavior

Here's where things get weird. NaN is the **only value in JavaScript that is not equal to itself**:

```javascript
console.log(NaN === NaN); // false
```

This isn't a bug—it's by design. According to the ECMAScript spec, NaN is "unordered" with respect to other values, including itself. This means you can't use `===` to check for NaN. Instead, you need to use `Number.isNaN()`:

```javascript
console.log(Number.isNaN(NaN)); // true
console.log(Number.isNaN("Hello")); // false
```

### Why NaN is a Number

Here's the weird thing. NaN is technically a **number type**. Yes, you read that right. The `typeof` NaN is `"number"`. This is because NaN is part of the IEEE 754 spec, which defines numeric operations. When a numeric operation fails, it doesn't make sense to return `undefined`, `null`, or `-1`—those aren't numbers. Instead, JavaScript returns NaN, which is still a number, just an invalid one.

### Example

Let's say you're building a function to calculate the average of an array of numbers. What happens if the array contains non-numeric values?

```javascript
function calculateAverage(arr) {
  const sum = arr.reduce((acc, val) => acc + val, 0);
  return sum / arr.length;
}

console.log(calculateAverage([1, 2, "three", 4])); // NaN
```

Here, the string `"three"` can't be added to the sum, so the result is NaN. This is a great example of how NaN can sneak into your code and cause unexpected behavior.

---

Despite all of this weirdness, NaN is a powerful tool. It's the only value that makes sense to return when a numeric operation fails, we have to learn how to work with the language.

NaN is a unique and important concept in JavaScript. It's not just a value—it's a signal that something went wrong in your calculations.

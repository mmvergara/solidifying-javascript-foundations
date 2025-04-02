---
title: Promise
description: A Promise in JavaScript represents a value that will be available in the future. It's a way to handle asynchronous operations cleanly, without falling into callback hell.
pubDate: "April 2 2025"
---

Think of a promise like ordering food at a restaurant you don’t get the meal immediately, but you **do** get a receipt (the promise) that guarantees your food **will** come (or you’ll get an explanation if it doesn’t).

### The Three States of a Promise

A promise can be in one of three states:

1. **Pending** – Waiting (your order is being cooked).
2. **Fulfilled** – Success (food is served).
3. **Rejected** – Failed (the kitchen ran out of ingredients).

Once a promise is settled (either fulfilled or rejected), it can’t change states. This is defined in the **ECMAScript spec** (ECMA-262, section 25.4).

### Why Promises Beat Callbacks

Before promises, we had **callback hell** nested callbacks that made code unreadable. Promises flatten this:

```javascript
// Callback Hell
getUser(userId, (user) => {
  getPosts(user, (posts) => {
    getComments(posts[0], (comments) => {
      console.log(comments);
    });
  });
});

// With Promises
getUser(userId)
  .then((user) => getPosts(user))
  .then((posts) => getComments(posts[0]))
  .then((comments) => console.log(comments))
  .catch((error) => console.error("Something broke:", error));
```

### The Magic of `.then()` and `.catch()`

- `.then()` handles success (like unwrapping the receipt to get your food).
- `.catch()` handles errors (like the waiter telling you they’re out of steak).

But here’s a **gotcha**: `.then()` **also** returns a **new promise**, which means you can chain them infinitely (or until your code becomes unreadable).

### Async/Await: Promises in Disguise

Under the hood, `async/await` is just syntactic sugar for promises.

```javascript
async function fetchData() {
  try {
    const user = await getUser(userId);
    const posts = await getPosts(user);
    const comments = await getComments(posts[0]);
    console.log(comments);
  } catch (error) {
    console.error("Oops:", error);
  }
}
```

### A Niche Example

Promises run in the **microtask queue**, which has higher priority than the regular callback queue. This means:

```javascript
console.log("Start");

setTimeout(() => console.log("Timeout"), 0);

Promise.resolve().then(() => console.log("Promise"));

console.log("End");

// Output order:
// Start → End → Promise → Timeout
```

Why? Because the microtask queue (promises) gets processed **before** the next event loop tick.

Promises are JavaScript’s way of saying, "I **promise** to handle this async task cleanly." This is one of the most important concepts in modern JavaScript, so make sure you understand it well!

### Extra tip: Promise.all()

When fetching data asynchronously, you might run multiple independent operations. Consider this example:

```js
const user = await getUser(userId);
const posts = await getPosts(userId);
```

Here, `getUser` runs first, and only after it completes does `getPosts` start. However, since `getPosts` only requires userId not the user data itself there's no reason to wait. Running them sequentially wastes time.

A better approach is to execute them concurrently using Promise.all():

```js
const [user, posts] = await Promise.all([getUser(userId), getPosts(userId)]);
```

This allows both getUser and getPosts to run simultaneously, significantly improving performance. Use `Promise.all()` whenever you have multiple **independent asynchronous tasks** to speed up execution!

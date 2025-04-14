---
title: Async/Await
description: Async/await is a cleaner way to handle Promises in JavaScript. It makes your code look synchronous while preserving the asynchronous nature of JavaScript.
pubDate: "April 3 s2025"
---

This is probably this one of the bread and butter of javscript programming. It is a cleaner way to handle Promises in JavaScript. Under the hood, `async/await` is just **syntactic sugar** over Promises (ECMAScript 2017, aka ES8). It doesn’t replace Promises—it just makes them easier to read.

- `async` marks a function as asynchronous (it _always_ returns a Promise).
- `await` pauses execution until a Promise settles (fulfilled _or_ rejected).

### **Why Use It?**

Compare this:

```javascript
// Promises
fetchUser()
  .then((user) => fetchPosts(user.id))
  .then((posts) => console.log(posts))
  .catch((err) => console.error(err));

// Async/Await
async function getUserPosts() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    console.log(posts);
  } catch (err) {
    console.error(err);
  }
}
```

The second version reads like plain English. No nesting, no indentation hell.

### **Await in Loops** → Sequential vs. Parallel execution matters.

```javascript
// Slow: Runs one after another
for (const url of urls) {
  const data = await fetch(url);
}

// Fast: Runs all at once
const promises = urls.map((url) => fetch(url));
const results = await Promise.all(promises);
```

### **Under the Hood: It’s Still Promises**

Async/await doesn’t change how JavaScript works—it just makes Promises _look_ synchronous. The event loop still does its thing, and `await` just pauses the function (not the whole script).

---
title: Debouncing
description: A JavaScript optimization technique that limits the rate at which a function is called.
pubDate: "April 1 2025"
---

Let's just get straight to the point,,, ye?

Imagine a search bar that fetches results as you type. Without debouncing, every keystroke fires an API call flooding your server and slowing things down. Debouncing ensures the search only triggers **after** you stop typing for a set time. This is one of the best use cases for debouncing.

```javascript
function debounce(func, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId); // Reset the timer
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

// Usage:
const search = debounce((query) => {
  console.log(`Searching for: ${query}`);
}, 300);

search("a"); // Ignored
search("ap"); // Ignored
search("app"); // Only this runs after 300ms
```

Each keystroke resets the `delay` timer. The function only runs when typing pauses for 300ms.

Debouncing is everywhere resize events, scroll handlers, button clicks to prevent double submissions.

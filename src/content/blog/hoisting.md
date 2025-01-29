---
title: Hoisting
description: Think of hoisting like setting up a stage before a play. Before the code runs (during the "creation phase"), JavaScript moves all declarations to the top of their scope like stagehands moving props into place before the curtain rises. The only catch is that the declarations are moved, not the initializations.
pubDate: "Jan 30 2025"
---

Think of hoisting like setting up a stage before a play. Before the code runs (during the "creation phase"), JavaScript moves all declarations to the top of their scope like stagehands moving props into place before the curtain rises. The only catch is that the declarations are moved, not the initializations.

According to the ECMAScript specification, this behavior is actually part of how JavaScript creates what's called the "lexical environment" during the creation phase. But let's not get too technical, just remember that JavaScript does a "pre-scan" of your code before running it.

### `var` Hoisting

```javascript
console.log(x); // Outputs: undefined
var x = 5;
console.log(x); // Outputs: 5
```

In this example, the var declaration var x is hoisted to the top of the scope, but the initialization x = 5 is not. This is why the first console.log(x) outputs undefined instead of throwing an error. The variable x exists, but it hasn't been assigned a value yet.

### `let` and `const` in the Temporal Dead Zone (TDZ)

```javascript
console.log(y); // Throws a ReferenceError: Cannot access 'y' before initialization
let y = 10;
console.log(y); // Outputs: 10
```

In this example, the let declaration let y is hoisted, but it is placed in the Temporal Dead Zone (TDZ) until the line where it is initialized. Trying to access y before its initialization results in a ReferenceError. This behavior is different from var, which would simply return undefined in a similar situation.

> this also includes anything that is declared using `let` or `const` including functions ofcourse

### The TDZ (Temporal Dead Zone)

Speaking of the temporal dead zone, this is where `let` and `const` declarations live before they're initialized. Unlike `var` which returns `undefined` when accessed before initialization, these modern declarations will throw an error. The ECMAScript spec calls this behavior "temporal dead zone semantics", but I just call it "JavaScript keeping us honest".

### A slightly harder example

```javascript
function setupEventHandler() {
  handleClick(); // Works!

  const config = {
    debug: true,
  };

  function handleClick() {
    if (config?.debug) {
      // Undefined!
      console.log("Debug mode");
    }
  }
}

setupEventHandler();
```

See what happened there? The function declaration `handleClick` is hoisted, so we can call it early. But that object `config`? It stays right where it is. This is why accessing `config` inside `handleClick` gives us `undefined`, we're trying to read the script before its ready.

### The Class Gotcha

Class hoisting doesn't work quite the same way.

```javascript
const dog = new Animal(); // Throws an error!

class Animal {
  constructor() {
    this.type = "mammal";
  }
}
```

While classes are hoisted, they stay in a "temporal dead zone" until their definition is evaluated. This means you can't access them before they're declared.

---

Just remember declarations are moved to the top, but initializations stay put.

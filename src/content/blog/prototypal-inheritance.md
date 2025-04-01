---
title: Prototypal Inheritance
description: Prototypal inheritance is one of JavaScript's core concepts, but it can feel a bit mysterious at first. Think of it like a family tree objects inherit traits from their ancestors. Let’s break it down in a way that’s easy to understand.
pubDate: "Feb 03 2025"
---

### The Family Tree

In JavaScript, every object has a hidden `[[Prototype]]` property that links it to another object. This is like a child inheriting traits from a parent. When you try to access a property or method on an object, JavaScript will look for it on the object itself. If it doesn’t find it it will check the object’s prototype, and so on, up the chain.

```javascript
const parent = {
  greet() {
    console.log("Hello from the parent!");
  },
};

const child = Object.create(parent);
child.greet(); // Logs: "Hello from the parent!"
```

In this example, `child` doesn’t have its own `greet` method, but it inherits it from `parent`. This is prototypal inheritance in action.

---

### The ECMAScript Spec

According to the ECMAScript specification, the `[[Prototype]]` property is what enables this inheritance. When you create an object using `Object.create()`, you’re explicitly setting its prototype. If you don’t specify a prototype, the object will inherit from `Object.prototype` by default.

Let’s say you’re building a game with different types of characters. You can use prototypal inheritance to share common properties and methods.

```javascript
const character = {
  health: 100,
  attack() {
    console.log(`${this.name} attacks for ${this.damage} damage!`);
  },
};

const warrior = Object.create(character);
warrior.name = "Conan";
warrior.damage = 20;

const mage = Object.create(character);
mage.name = "Gandalf";
mage.damage = 15;

warrior.attack(); // Logs: "Conan attacks for 20 damage!"
mage.attack(); // Logs: "Gandalf attacks for 15 damage!"
```

Here, both `warrior` and `mage` inherit the `attack` method from `character`, but they have their own unique properties like `name` and `damage`.

---

### The Chain

The prototype chain is like a ladder. When you try to access a property or method, JavaScript climbs up the ladder until it finds what it’s looking for or reaches the top (which is `null`). If it doesn’t find the property, it returns `undefined`.

For example:

```javascript
console.log(warrior.health); // 100 (inherited from character)
console.log(warrior.mana); // undefined (not found in the chain)
```

### Why Prototypal Inheritance?

Let's say where going to create a function that returns an object.

```javascript
function createCar(carName) {
  return {
    name: carName,
    getName() {
      return this.name;
    },
  };
}

const car1 = createCar("Tesla");
const car2 = createCar("Ford");
```

At first glance, this approach might seem acceptable. However, if we create multiple car objects, each will have its own copy of the getName method. While the properties are unique to each object, duplicating the method for every instance is inefficient since the method logic remains the same across all objects. Ideally, methods should be shared among instances to avoid unnecessary duplication.

TLDR: every car object created has its own copy of the `getName` method, which wastes memory.

---

To avoid duplication, we can use **prototypal inheritance** to share methods between objects. Here's a cleaner implementation using a **constructor function** and `prototype`:

```javascript
function Car(carName) {
  this.name = carName;
}

Car.prototype.getName = function () {
  return this.name;
};

const car1 = new Car("Tesla");
const car2 = new Car("Ford");

console.log(car1.getName()); // Tesla
console.log(car2.getName()); // Ford
```

Now the `getName` method is shared between all `Car` instances. This way, we avoid duplicating the method for each object, making our code more memory-efficient.

---

Congratulations! Now you know what actually happens when you use the `class` syntax in JavaScript. Under the hood, JavaScript uses prototypal inheritance to link the prototype of the `Car` class to its methods. Here's the same example using the `class` syntax:

```javascript
class Car {
  constructor(carName) {
    this.name = carName;
  }

  getName() {
    return this.name;
  }
}

const car1 = new Car("Tesla");
const car2 = new Car("Ford");

console.log(car1.getName()); // Tesla
console.log(car2.getName()); // Ford
```

---

At first i didn't care but i think that Prototypal inheritance is something a Javascript developer should understand, it's a foundational concept that underpins many advanced JavaScript patterns.

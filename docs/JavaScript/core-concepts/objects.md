---
sidebar_position: 2
---

# Objects
Objects in JavaScript are collections of key-value pairs used to store and organize data. Each key (called a property) is a string or symbol, and each value can be any type. Objects can represent structured data and support methods (functions as values).

### How can you create objects in JS?
Objects in JavaScript can be created using object literals, constructors, classes, or Object.create().
- Object Literal – Most common & concise:
    ```javascript
    const obj = { name: "Oleh", age: 25 };
    ```
- Constructor Function – Traditional OOP style:
    ```javascript
    function User(name) {
        this.name = name;
    }
    const u = new User("Oleh");
    ```
- Class Syntax – Modern OOP approach:
    ```javascript
    class User {
    constructor(name) {
        this.name = name;
    }
    }
    const u = new User("Oleh");
    ```
- Object.create() – Create with a specific prototype:
    ```javascript
    const proto = { greet() { console.log("Hi"); } };
    const obj = Object.create(proto)
    ```
- Factory Function – A function that returns an object:
    ```javascript
    function createUser(name) {
        return { name };
    }
    const u = createUser("Oleh");
    ```
### How do you access object properties (dot vs bracket)?
You can access object properties in JavaScript using dot notation or bracket notation.

#### Dot notation
Dot Notation – Simple, clean. Used when property name is a valid identifier (no spaces, starts with letter)
```javascript
const user = { name: "Oleh" };
console.log(user.name); // "Oleh"
```

#### Bracket Notation – More flexible
Needed for dynamic keys (`user[key]`). Property names with spaces/symbols (`user["first name"]`).
```javascript
const key = "name";
console.log(user["name"]); // "Oleh"
console.log(user[key]);    // "Oleh"
```

#### Destructuring
```javascript
const user = { name: "Oleh", age: 25 };
const { name, age } = user; // name = "Oleh", age = 25
```

#### Object.keys(), Object.values(), Object.entries(): Useful for accessing all properties programmatically:
```javascript
const user = { name: "Oleh", age: 25 };

Object.keys(user);   // ["name", "age"]
Object.values(user); // ["Oleh", 25]
Object.entries(user); // [["name", "Oleh"], ["age", 25]]
for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
```

#### for...in Loop 
```javascript
for (const key in user) {
  console.log(user[key]);
}
```
### What is destructuring?
**Destructuring** is a JavaScript feature that allows you to extract values from arrays or properties from objects into distinct variables.
```javascript
const user = { name: "Oleh", age: 25 };
const colors = ["red", "green", "blue"];

// Basic destructuring
const { name, age } = user;
const [first, second] = colors;

// With default values
const { name, age, country = "Ukraine" } = user;

// Renaming variables
const { name: userName, age: userAge } = user;

// Skip elements
const [first, , third] = colors;
```

### How do you test if a property exists in an object?
To test if a property exists in an object, use the `in` operator, `hasOwnProperty()`, or optional chaining with comparison.

- `in` Operator: Checks if a key exists anywhere in the object (own or inherited)
- `hasOwnProperty()` Checks if a key exists directly on the object
- Optional Chaining + Comparison Safely check if a value exists or is not undefined
- `propertyIsEnumerable()` checks if a property exists directly on an object and is enumerable.

```javascript
const obj = { name: "Oleh" };

"name" in user; // true
user.hasOwnProperty("name"); // true
if (user?.name !== undefined) { ... }
obj.propertyIsEnumerable("name"); // true
```

### What is a property descriptor?
A property descriptor is an object that defines a property’s configuration: whether it's writable, enumerable, or configurable. Every object property has a descriptor behind the scenes. You can access or define it using `Object.getOwnPropertyDescriptor(obj, prop)`, `Object.defineProperty(obj, prop, descriptor)`

```javascript
Object.getOwnPropertyDescriptor(obj, prop)

Object.defineProperty(obj, prop, descriptor)
const user = {};
Object.defineProperty(user, "name", {
  value: "Oleh",
  writable: false,
  enumerable: true,
  configurable: false
});
```
### What flags exist in Object.defineProperty (writable, enumerable, configurable)?
**1. `writable`** - Controls if the property value can be changed
```javascript
const obj = {};
Object.defineProperty(obj, 'name', {
    value: 'John',
    writable: false  // Cannot be changed
});

obj.name = 'Jane';  // Silent failure in strict mode
console.log(obj.name); // "John"
```

**2. `enumerable`** - Controls if the property appears in loops
```javascript
const obj = { a: 1 };
Object.defineProperty(obj, 'b', {
    value: 2,
    enumerable: false  // Hidden from enumeration
});

console.log(Object.keys(obj));     // ["a"]
console.log(obj.propertyIsEnumerable('b')); // false
```

**3. `configurable`** - Controls if the property descriptor can be changed
```javascript
const obj = {};
Object.defineProperty(obj, 'name', {
    value: 'John',
    configurable: false  // Cannot delete or reconfigure
});

delete obj.name;  // Fails silently
Object.defineProperty(obj, 'name', { writable: true }); // TypeError
```

### What is serialization (JSON)? Limitations?
Serializing an object means converting it into a string format (usually JSON) for storage or transmission. In JS, use `JSON.stringify()` and `JSON.parse()`.

```javascript
const user = { name: "Jason", age: 35 };
const json = JSON.stringify(user); // '{"name":"Jason","age":35}'
const parsed = JSON.parse(json); // { name: "Jason", age: 35 }
```

:::danger[Limitations]
Limitations of `JSON.stringify()`:
- Skips undefined, functions, symbols
- Converts dates to strings
- Fails on circular references (throws an error)
:::

:::tip
You can control how an object is serialized using `toJSON()`:
```javascript
const user = {
  name: "Jonh",
  toJSON() {
    return { userName: this.name };
  }
};
JSON.stringify(user); // '{"userName":"Jonh"}'
```
:::

### What are object methods like keys(), values(), entries(), assign()?

1. `Object.keys()`: Returns an array of a given object's own enumerable property names (keys).

```javascript
const person = { name: "Alice", age: 25, city: "Wonderland" };
const keys = Object.keys(person);
console.log(keys); // ["name", "age", "city"]
```

2. `Object.values()`: Returns an array of a given object's own enumerable property values.
```javascript
const person = { name: "Alice", age: 25, city: "Wonderland" };
const values = Object.values(person);
console.log(values); // ["Alice", 25, "Wonderland"]
```
3. `Object.entries()`: Returns an array of a given object's own enumerable property `[key, value]` pairs.
```javascript
const person = { name: "Alice", age: 25, city: "Wonderland" };
const entries = Object.entries(person);
console.log(entries); // [["name", "Alice"], ["age", 25], ["city", "Wonderland"]]
```
4. `Object.assign()`: Copies all enumerable own properties from one or more source objects to a target object. It returns the modified target object.
```javascript
const target = { a: 1 };
const source = { b: 2, c: 3 };
const returnedTarget = Object.assign(target, source);
console.log(returnedTarget); // { a: 1, b: 2, c: 3 }
```

### What is the difference between freeze() and seal()?

- `Object.freeze()`: Freezes an object, preventing new properties from being added and existing properties from being modified or deleted. The object becomes immutable.
```javascript
const person = { name: "Alice", age: 25 };
Object.freeze(person);
person.age = 30; // No effect, object is frozen
console.log(person.age); // 25
```
- `Object.seal()`: Seals an object, preventing new properties from being added, but allows modification of existing properties.
```javascript
const person = { name: "Alice", age: 25 };
Object.seal(person);
person.age = 30; // Allowed, since seal doesn't prevent modifications
person.city = "Wonderland"; // Not allowed, cannot add new properties
console.log(person); // { name: "Alice", age: 30 }
```

### What is getPrototypeOf()?
`Object.getPrototypeOf()`: Returns the prototype (i.e., the internal `[[Prototype]]` property) of the specified object.
```javascript
const person = { name: "Alice" };
const proto = Object.getPrototypeOf(person);
console.log(proto); // { ... } (the default object prototype)
```

### What are getters and setters?
Getters and setters are special methods in objects that act like properties—they allow controlled access to reading (get) or writing (set) values. They are defined using `get` and `set` keywords inside objects or classes. Common for data validation, formatting, or computed properties.
```javascript
const user = {
  firstName: "Jonk",
  lastName: "Pork",
  
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  }
};
console.log(user.fullName);     // "Jonk Pork"
user.fullName = "Ivan Petrov";  // updates firstName & lastName
```

### Why use Symbols as property keys?
Symbols can be used as unique property keys in objects to avoid naming conflicts or hide implementation details. Symbols are created with `Symbol(description)` and are guaranteed to be unique — even with the same description. Symbols are great for building libraries or APIs with hidden internal logic.
```javascript
const id = Symbol("id");
const user = {
  [id]: 123,
  name: "Jonh"
};
```
#### Why Use Symbols as Property Keys?
- To create non-colliding keys (safe from overwriting)
- To store internal/meta data
- For well-known symbols (like Symbol.iterator, Symbol.toPrimitive) to hook into JS behavior

### What is the configurable flag in object properties?
The configurable flag in a property descriptor controls whether a property can be deleted or redefined. By default, properties are `configurable: true`, meaning:
- You can delete the property
- You can change its descriptor (e.g. make it non-enumerable or read-only)
If `configurable: false`, then:
- You cannot delete the property
- You cannot redefine it via `Object.defineProperty()`
- You can still change its value (if `writable: true`)

### What is the prototype attribute in objects?

In JavaScript, every function (when used as a constructor) has a prototype property. When you create an object using new, that object’s internal `[[Prototype]]` links to the constructor’s .prototype.
- `prototype` is a property of functions used to set up inheritance.
- `__proto__` is a property of objects that points to the prototype it inherits from
```javascript
function User(name) {
  this.name = name;
}
User.prototype.sayHi = function () {
  return `Hi, ${this.name}`;
};

const u1 = new User("Jonh");
u1.sayHi(); // "Hi, Jonh"
```

### What is conditional property access (?.) and invocation?

- **Conditional Property Access**: Safely access nested properties:
```javascript
const user = { profile: null };
console.log(user.profile?.name); // undefined (not an error)
```
Without `?.`, this would throw: `Cannot read property 'name' of null.`
- **Conditional Method Invocation**: Safely call a function only if it exists:
```javascript
const user = {};
user.sayHi?.(); // does nothing, no error
```

### What are enumerable objects? 

Enumerable objects in JavaScript are objects that implement the iterable protocol, meaning they can be used in a `for...of loop`. Technically, JS doesn't have a built-in "Enumerable" type like C# — but in JS terms, iterable or enumerable objects are those that implement: `[Symbol.iterator]()`

Each object property has an internal `[[Enumerable]]` flag. If it's true, the property is considered enumerable.

```javascript
const myIterable = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  }
};

for (const val of myIterable) {
  console.log(val); // 1, 2, 3
}
```
#### Examples of Enumerable Objects
- Arrays
- `Map`
- `Set`
- `NodeList`

#### Examples of Enumerable Usage:
- `for...in` → lists all enumerable own + inherited props
- `Object.keys()` → returns own enumerable props
- `JSON.stringify()` → includes only enumerable props

### Object Methods
Methods are functions that are properties of an object. They can be defined using function expressions or shorthand syntax. Inside a method, `this` keyword refers to the object on which the method was called. It allows methods to access and manipulate the object's properties.

```javascript
const person = {
  name: "Alice",
  greet: function() {
    console.log("Hello, " + this.name);
  }
};
person.greet(); // "Hello, Alice"
```

### How to copy Objects?
#### Shallow Copy
**Shallow Copy**: Copies only top-level properties (nested objects remain linked).
- Spread Syntax:
```javascript
const copy = { ...original };
```
- `Object.assign()`:
```javascript
const copy = Object.assign({}, original);
```

#### Deep Copy
**Deep Copy**: Copies entire object structure, including nested objects.
- **JSON trick**:
```javascript
const deepCopy = JSON.parse(JSON.stringify(original));
```
:::danger
⚠️Fails on: Functions, undefined, Symbols, Circular refs
:::

- **Structured Clone (modern & safe)**: `structuredClone()` is a built-in JavaScript function introduced to make deep copying easier and more reliable.
```javascript
const deepCopy = structuredClone(value);
```
- **Manual recursion** or libraries (like `lodash.cloneDeep()`)
```javascript
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") {
    return obj;
  }

  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }

  const copy = {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      copy[key] = deepClone(obj[key]);
    }
  }
  return copy;
}

const original = { name: "Alice", address: { city: "Wonderland", zip: "12345" } };
const deepCopy = deepClone(original);

deepCopy.address.city = "New Wonderland";
console.log(original.address.city); // "Wonderland"
```
### How do you compare two objects for equality?
In JavaScript, objects are compared by reference, not by value. Two objects are only equal if they reference the exact same memory. You need to manually compare properties, or use libraries like: `lodash.isEqual(a, b)` or `JSON.stringify(a) === JSON.stringify(b)` (has limitations: order matters, can't handle functions/symbols)

```javascript
function deepEqual(obj1, obj2) {
  if (obj1 === obj2) {
    return true;
  }

  if (typeof obj1 !== "object" || typeof obj2 !== "object" || obj1 === null || obj2 === null) {
    return false;
  }

  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);

  if (keys1.length !== keys2.length) {
    return false;
  }

  for (let key of keys1) {
    if (!keys2.includes(key) || !deepEqual(obj1[key], obj2[key])) {
      return false;
    }
  }

  return true;
}

const obj1 = { name: "Alice", details: { age: 25, city: "Wonderland" } };
const obj2 = { name: "Alice", details: { age: 25, city: "Wonderland" } };
const obj3 = { name: "Alice", details: { age: 30, city: "Wonderland" } };

console.log(deepEqual(obj1, obj2)); // true
console.log(deepEqual(obj1, obj3)); // false
```

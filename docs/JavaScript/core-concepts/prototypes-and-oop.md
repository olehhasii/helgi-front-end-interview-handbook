---
sidebar_position: 6
---

# Prototypes & OOP

## Prototypes

### What is Prototype in JS?

A prototype is an object from which other objects can inherit properties and methods in JavaScript. It’s the core of JS’s prototype-based inheritance model.

In JavaScript, every object has an internal link to another object called its **prototype**. When you access a property not found on the object, JS looks it up on the prototype, then up the chain (the **prototype chain**) until `null`.

### `__proto__` vs `prototype`

- `obj.__proto__` (or `Object.getPrototypeOf(obj)`) → the prototype of an instance.
- `Func.prototype` → the object used as the prototype for instances created by `new Func()`.

```javascript
function User(name) { this.name = name; }
User.prototype.greet = function() { return `Hi, ${this.name}`; };

const u = new User('Oleh');
console.log(u.greet()); // "Hi, Oleh"
console.log(Object.getPrototypeOf(u) === User.prototype); // true
```

### How to Set a Prototype?
Use these standard ways to set or change an object's prototype:

#### 1) Create with a prototype (preferred)
`Object.create(proto)`: creates a new object with the given prototype.
```javascript
const proto = { greet() { return 'hi'; } };
const obj = Object.create(proto); // obj → proto
```

#### 2) Change an existing object's prototype
`Object.setPrototypeOf(obj, proto)`: changes the prototype of an existing object (less recommended, slower).
```javascript
const proto = { greet() { return 'hi'; } };
const obj = {};
Object.setPrototypeOf(obj, proto); // performance cost
```

#### 3) Via constructor functions (`new`) or Clasess — instances link to `Fn.prototype`
**Constructor functions / classes**: when using `new`, the created object’s prototype is set to the constructor’s .prototype.
```javascript
function User(name) { this.name = name; }
User.prototype.say = function() { return 'hi ' + this.name; };

const u = new User('Oleh'); // u → User.prototype

class User {
  say() { return 'hi'; }
}
const u = new User(); // u → User.prototype
```

### How do you create objects with no prototype? (Object.create(null))
Use `Object.create(null)` to create a “dictionary” object with a null prototype (no inherited properties or methods).
```javascript
const obj = Object.create(null); // obj → proto
```

### Check own vs inherited properties

#### Own property checks (preferred)
```javascript
obj.hasOwnProperty('x');                 // true if own (enumerable or not)
Object.hasOwn(obj, 'x');                 // ES2022, safer (no prototype issues)
Object.prototype.hasOwnProperty.call(obj, 'x'); // Safe on plain objects
```

#### Own + inherited
```javascript
'x' in obj; // true if found on obj or anywhere on its prototype chain
```

#### Iteration tips
```javascript
for (const key in obj) {
  if (Object.hasOwn(obj, key)) { /* own enumerable only */ }
}

Object.keys(obj);                // own enumerable string keys
Object.getOwnPropertyNames(obj); // all own string keys (incl. non-enumerable)
Object.getOwnPropertySymbols(obj); // own symbol keys
Reflect.ownKeys(obj);            // all own keys (strings + symbols)
```
### What is prototypal inheritance?
Prototypal inheritance is how JavaScript objects inherit properties/methods from other objects via the prototype chain. When a property isn’t found on an object, JS looks up its prototype, then its prototype’s prototype, and so on, until `null`.

```javascript
const animal = { eats: true, speak() { return '...'; } };
const dog = Object.create(animal);
dog.barks = true;

dog.eats;   // true (inherited)
dog.speak(); // '...' (inherited)
```

### What is prototype delegation?
Prototype delegation is when an object defers (delegates) property/method lookups to its prototype via the prototype chain. If a property isn’t found on the object itself, JavaScript looks up its `[[Prototype]]` (and continues up the chain) to resolve it.

```javascript
const proto = {
  greet() { return `hi ${this.name}`; }
};

const user = Object.create(proto);
user.name = 'Oleh';

user.greet(); // 'hi Oleh' (method found on prototype, `this` is user)
```

### What are “own” vs inherited properties in serialization (JSON.stringify)?
JSON.stringify includes only an object’s own enumerable string-keyed properties; it ignores inherited, non-enumerable, and symbol keys.

```javascript
const proto = { a: 1 };
const obj = Object.create(proto);
obj.b = 2;
JSON.stringify(obj); // {"b":2}
```
:::tip
`toJSON()` method (own or inherited) can customize what gets serialized.
:::

### How does instanceof work internally? What about isPrototypeOf?
- `instanceof`: Checks whether `constructor.prototype` appears in obj’s prototype chain (or uses `constructor[Symbol.hasInstance]` if defined). In other words, it answers: “Does `Constructor.prototype` appear anywhere in obj’s prototype chain?”
```javascript
obj instanceof Constructor
```
- The method `isPrototypeOf()` performs the same logical check, but from the opposite perspective. It returns true if prototypeObj exists in the prototype chain of object.
```javascript
function Animal() {}
const dog = new Animal();

console.log(Animal.prototype.isPrototypeOf(dog)); // true
console.log(Object.prototype.isPrototypeOf(dog)); // true
console.log(Array.prototype.isPrototypeOf(dog));  // false
```


### What is prototype pollution? How do you avoid it?
**Prototype pollution** is a security vulnerability that allows attackers to **modify the base `Object.prototype`** of JavaScript objects. Because all objects in JavaScript inherit from this prototype, changing it affects **every object** in the application — potentially leading to **unexpected behavior or remote code execution**.

#### ⚙️ How It Happens

When merging or copying objects (e.g., using `Object.assign()`, deep merge utilities, or `JSON.parse()`), unvalidated input can **inject special keys** like `__proto__`, `constructor`, or `prototype` that modify the global prototype chain.

```javascript
const payload = JSON.parse('{"__proto__": {"polluted": true}}');

const obj = {};
Object.assign(obj, payload);

console.log(obj.polluted); // true
console.log({}.polluted);  // true ❗ every object now has polluted = true
```
#### How to Prevent Prototype Pollution
- Validate input keys
- Avoid `__proto__`, constructor, prototype keys
- Use safe libraries
- Freeze critical prototypes

### Should you extend native prototypes (e.g., Array.prototype)? Why/why not?
Extending native prototypes means **adding new methods or properties** to built-in objects like `Array.prototype`, `Object.prototype`, or `String.prototype`.

```javascript
Array.prototype.last = function () {
  return this[this.length - 1];
};

console.log([1, 2, 3].last()); // 3
```
While this works, it’s generally not recommended in production code.

- Name collisions: Future ECMAScript versions may introduce a method with the same name — your version will be overwritten or behave differently.
- Breaking assumption: Third-party libraries may iterate over arrays/objects assuming standard behavior. Added properties can cause unexpected bugs.
- Harder debugging: Modified prototypes change global behavior — bugs appear in unrelated parts of the codebase.

### How to safely check own props without `hasOwnProperty` collisions?
Normally, to check if an object has its **own property** (not inherited from its prototype chain), you’d use: `obj.hasOwnProperty("key");` However, this can fail in edge cases — for example, when:
- The object’s prototype has been replaced or removed.
- The object defines its own property named `hasOwnProperty`, shadowing the built-in method.

Alternative solution:
1) `Object.hasOwn()` (ES2022)

Modern and cleaner — doesn’t rely on prototype chaining.
```javascript
Object.hasOwn(obj, "key"); // ✅ true
```
2) Call `hasOwnProperty` from `Object.prototype` explicitly:
```javascript
Object.prototype.hasOwnProperty.call(obj, "key"); // ✅ true
```

## OOP

### What is OOP?
**Object-Oriented Programming (OOP)** is a programming paradigm where code is organized into objects that combine data (properties) and behavior (methods). JS is **prototype-based**; `class` is syntax sugar over prototypes. Methods are shared via the prototype chain.

### What are OOP principles in JS (encapsulation, inheritance, polymorphism)?
- **Encapsulation**: keep data + methods together; hide internals via public API
```javascript
class Counter {
  #count = 0;                 // private field (encapsulated)
  increment() { this.#count++; }
  get value() { return this.#count; }
}
```

- **Inheritance**: reuse/extend behavior from a parent “type”. create new classes/objects based on existing ones using `extends` or `prototypes`.
```javascript
class Animal { speak() { return '...'; } }
class Dog extends Animal { speak() { return 'bark'; } } // overrides parent
```

- **Polymorphism**: same interface, different implementations. Same method name behaves differently depending on the object. Achieved through method overriding or duck typing.
```javascript
const animals = [new Animal(), new Dog()];
animals.map(a => a.speak()); // ['...', 'bark'] — same method, different results
```

- **Abstraction**: expose only what’s necessary; hide complexity

### What are static, public, private class members?
- **Public fields/methods**: available on every object created from the class. Accessible on instances (default visibility)
- **Private fields/methods**: start with `#`, accessible only inside the class
- **Static fields/methods**: belong to the class (not instances)
#### Example
```javascript
class User {
  // public instance field
  role = 'user';

  // private instance field
  #token = null;

  // static field
  static VERSION = '1.0';

  constructor(name) {
    this.name = name; // public
  }

  // public method
  greet() { return `Hi, ${this.name}`; }

  // private method
  #authorize(token) { this.#token = token; }

  login(token) { this.#authorize(token); }

  // static method (no access to instance via `this`)
  static isUser(obj) { return obj instanceof User; }
}

const u = new User('Jonh');
u.greet();           // "Hi, Jonh"
User.VERSION;        // "1.0"
User.isUser(u);      // true
// u.#token;         // SyntaxError (private)
```

### How do classes work in JS?
`class` is syntactic sugar over prototypes. It defines a constructor, prototype methods (shared by instances), static members (on the class), and supports inheritance with `extends`/`super`. It makes defining constructor functions and prototypes easier to read.

Key points:
- Under the hood: methods go on `ClassName.prototype`; instances link via `[[Prototype]]`.
- `new` creates an object, sets its prototype to the class’s `.prototype`, and runs the constructor.
- Supports inheritance with `extends` and `super`.
- Methods inside classes are non-enumerable by default.

```javascript
class Animal {
  constructor(name) { this.name = name; }
  speak() { console.log(this.name + " makes a sound"); }
}
class Dog extends Animal {
  speak() { console.log(this.name + " barks"); }
}
const d = new Dog("Rex");
d.speak(); // "Rex barks"
```

### What is constructors?
A **constructor** creates and initializes objects.

### Constructor Functions
A constructor function is a regular function intended to create objects with `new`. By convention it’s named in PascalCase and uses `this` to assign instance properties. Methods should go on the function’s `.prototype` to be shared.
```javascript
function User(name) {           // constructor
  this.name = name;             // instance property
}
User.prototype.greet = function() {
  return `Hi, ${this.name}`;    // shared method
};

const u = new User('Oleh');
u.greet(); // "Hi, Oleh"
```

### How does the new keyword work?
The new keyword in JavaScript creates a new object from a constructor function or class. It performs several behind-the-scenes steps.
- Creates a new empty object {}.
- Sets the object’s `[[Prototype]]` to the constructor’s `.prototype`.
- Calls the constructor with this bound to the new object.
- If the constructor explicitly returns an object, that object is returned; otherwise, the new one is used.

### What are getters/setters in classes?
Getters and setters in JS classes are special methods that let you control how a property is read or written.
- Getter (`get`): called when the property is accessed.
- Setter (`set`): called when the property is assigned a value.

They look like properties when used, but run functions behind the scenes.
```javascript
class User {
  constructor(name) { this._name = name; }
  
  get name() { return this._name.toUpperCase(); }   // getter
  set name(value) { this._name = value.trim(); }   // setter
}

const u = new User("Jonh");
console.log(u.name); // "JONH" (getter ran)
u.name = "  Max  ";   // setter ran
console.log(u.name);  // "MAX"
```
### What is subclassing (extends, super)?
Subclassing in JavaScript means creating a class that inherits from another class using extends. The super keyword lets you call the parent’s constructor or methods.
```javascript
class Animal {
  constructor(name) { this.name = name; }
  speak() { return `${this.name} makes a noise`; }
  static kind() { return 'animal'; }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);                // must call before `this`
    this.breed = breed;
  }
  speak() {                     // override + call parent
    return super.speak() + ' and barks';
  }
  static kind() {
    return super.kind() + ' (canine)';
  }
}

const d = new Dog('Rex', 'Collie');
d.speak();       // "Rex makes a noise and barks"
Dog.kind();      // "animal (canine)"
```

### When to use Object.create, function constructors and classes?
- Object.create(proto)
  - Use for: simple objects with a chosen prototype, dictionaries (`Object.create(null)`), mixin-style composition.
  - Pros: no constructor ceremony; precise prototype control; great for “object linking.”
  - Cons: fewer conventions; harder to convey “type”/API at scale.
  - Example:
  ```javascript
  const proto = { greet() { return 'hi'; } };
  const o = Object.create(proto);
  ```

- Function constructors (with .prototype)
  - Use for: legacy codebases, fine-grained control, when classes aren’t available.
  - Pros: explicit control over `.prototype`; works everywhere.
  - Cons: more boilerplate; easier to make mistakes (`new` omission, constructor reset).
  - Example:
  ```javascript
  function User(name){ this.name = name; }
  User.prototype.greet = function(){ return 'hi ' + this.name; };
  const u = new User('Oleh');
  ```

- Classes (class/extends/super)
  - Use for: modern OOP, clear inheritance, static/private fields, tooling/TS friendliness.
  - Pros: concise syntax; built-in `extends`, `super`, `static`, `#private`; widely understood.
  - Cons: still prototype-based under the hood; inheritance can be overused vs composition.
  - Example:
  ```javascript
  class User { constructor(name){ this.name = name; } greet(){ return `hi ${this.name}`; } }
  class Admin extends User { }
  ```

### How do you override a prototype method and call the “super” method?
#### With classes (use `super`)
```javascript
class Animal {
  speak() { return 'noise'; }
}
class Dog extends Animal {
  speak() {
    const base = super.speak();     // call parent
    return base + ' + bark';
  }
}
```

#### With prototypes (call parent method manually)
```javascript
function Animal() {}
Animal.prototype.speak = function() { return 'noise'; };

function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
  const base = Animal.prototype.speak.call(this);  // "super"
  return base + ' + bark';
};
```

### What are factory functions?
A factory function is a regular function that returns a new object without using `class` or `new`. It’s a simple way to create multiple similar objects.
- No new keyword is required.
- Each call creates and returns a new object.
- Can use closures for private data.
```javascript
function createUser(name) {
  return {
    name,
    greet() { console.log("Hi " + name); }
  };
}

const u1 = createUser("Jonh");
```
### What is class composition?
**Class composition** builds objects by combining smaller behaviors (“has-a” relationships) instead of inheriting from a single parent (“is-a”). It follows the principle **"prefer composition over inheritance."**


```javascript
const canFly = Base => class extends Base {
  fly() { console.log("Flying"); }
};

const canBark = Base => class extends Base {
  bark() { console.log("Woof!"); }
};

class Dog {}
class SuperDog extends canBark(canFly(Dog)) {}

const d = new SuperDog();
d.fly(); // Flying
d.bark(); // Woof!
```
### What is dependency injection?
**Dependency Injection** (DI) is a design pattern where an object’s dependencies (other objects or services it needs) are provided (injected) from the outside instead of being created inside.
```javascript
class UserService {
  constructor(db) {
    this.db = db; // injected dependency
  }
}
const db = new Database();
const service = new UserService(db);
```
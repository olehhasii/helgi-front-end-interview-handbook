---
sidebar_position: 3
---

# Arrays
An Array in JavaScript is an ordered, zero-indexed collection of elements that can hold values of any type. Arrays are a special kind of object, optimized for storing sequences of items. They have a numeric index and a dynamic length property.

### How can you create arrays?
Arrays in JavaScript can be created using array literals, Array constructors, factory methods, or spread and from utilities.

1. **Array Literal** (most common)
```javascript
const arr = [1, 2, 3];
```
2. **Array Constructor**: 
```javascript
const arr1 = new Array(3);        // [ <3 empty items> ]
const arr2 = new Array(1, 2, 3);  // [1, 2, 3]
```
:::warning
⚠️ Using `new Array(n)` creates an empty array of length n, not filled with values.
:::
3. **Array.of()**
```javascript
Array.of(3);       // [3]
Array.of(1, 2, 3); // [1, 2, 3]
```
4. **Array.from()**: Creates an array from: Array-like objects (e.g. arguments, NodeList), Iterable objects (e.g. strings, Sets), Can apply a mapping function.
```javascript
Array.from("abc"); // ['a', 'b', 'c']
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```
5. **Spread Operator**
```javascript
const copy = [...arr];          // shallow copy
const chars = [..."hello"];     // ['h', 'e', 'l', 'l', 'o']
```
### What are sparse arrays?
A sparse array is an array with missing elements, meaning some indexes are empty (i.e., they don’t exist at all, not even as undefined). In a sparse array, some indices are skipped, so the array’s `length` is greater than the number of defined elements.
```javascript
const arr = [1, , 3];  // sparse array
console.log(arr[1]);   // undefined
console.log(1 in arr); // false → index 1 doesn't exist
console.log(arr.length); // 3
```
:::tip
Methods like `map`, `forEach`, `reduce` skip holes.
```javascript
const sparse = [1, , 3];
const fixed = sparse.map(x => x); // [1, undefined, 3]
```
:::

### How do you iterate arrays?

- `for loop` – Classic, index-based:
```javascript
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```
- `for...of`:
```javascript
for (const value of arr) {
  console.log(value);
}
```
- `forEach()` – Executes a callback on each item: Skips holes, Does not support `break`/`continue`:
```javascript
arr.forEach((value, index) => {
  console.log(index, value);});
```
-`for...in` – **Not recommended for arrays**:  Iterates over keys, including inherited ones,  Can be unpredictable—use for objects, not arrays
```javascript
for (const key in arr) {
  console.log(key, arr[key]);
}
```
- `map()` – Transforms array, returns new one:
```javascript
const doubled = arr.map(x => x * 2);
```
### What are key array methods (`map`, `forEach`, `reduce`, `filter`)?
#### Common Non-Mutating Methods

| Method | Syntax / Arguments | Returns | Description |
|:-------|:-------------------|:---------|:-------------|
| `map()` | `array.map(callback(element, index, array))` | New array | Transforms each element using a callback |
| `filter()` | `array.filter(callback(element, index, array))` | New array | Keeps elements that pass a test |
| `reduce()` | `array.reduce(callback(acc, cur, index, array), initialValue)` | Single value | Accumulates array values into one result |
| `slice()` | `array.slice(start?, end?)` | New array | Extracts a section without mutating |
| `concat()` | `array.concat(value1, value2, ...)` | New array | Merges arrays or values |
| `includes()` | `array.includes(value, fromIndex?)` | Boolean | Checks if value exists |
| `find()` | `array.find(callback(element, index, array))` | Element or `undefined` | Returns the first match |
| `every()` | `array.every(callback(element, index, array))` | Boolean | `true` if **all** elements satisfy condition |
| `some()` | `array.some(callback(element, index, array))` | Boolean | `true` if **any** element satisfies condition |
| `indexOf()` | `array.indexOf(value, fromIndex?)` | Number | Index of first match, or `-1` if not found |

#### Common Mutating Methods
| Method | Syntax / Arguments | Returns | Description |
|:--------|:-------------------|:---------|:-------------|
| `push()` | `array.push(element1, ..., elementN)` | New length | Adds one or more elements to the end |
| `pop()` | `array.pop()` | Removed element | Removes the last element |
| `shift()` | `array.shift()` | Removed element | Removes the first element |
| `unshift()` | `array.unshift(element1, ..., elementN)` | New length | Adds one or more elements to the start |
| `splice()` | `array.splice(start, deleteCount?, ...items)` | Removed elements | Adds or removes elements at a specific index |
| `reverse()` | `array.reverse()` | The same array (reversed) | Reverses array order in place |
| `sort()` | `array.sort(compareFn?)` | The same array (sorted) | Sorts elements in place (optionally with comparator) |
| `fill()` | `array.fill(value, start?, end?)` | The same array | Overwrites elements with a static value |
| `copyWithin()` | `array.copyWithin(target, start?, end?)` | The same array | Copies a sequence of elements within the array |

### How do arrays differ from objects?
Arrays are specialized objects in JavaScript, but they differ in how they’re used and optimized.
 - **Objects**: store key–value pairs (keys are usually strings/symbols).
 - **Arrays**: use numeric indices (0, 1, 2...) and have a `length` property.

Arrays inherit methods from `Array.prototype` (push, map, filter, etc.), while objects inherit from `Object.prototype`. Engines optimize arrays for sequential numeric access, so they’re generally faster for ordered lists.

### What are array-like objects?
Array-like objects are objects that look like arrays because they have indexed properties and a length property, but they don’t inherit from `Array.prototype`.

### Copying & Cloning Arrays
In JavaScript, arrays are **reference types**. Assigning one array to another variable **does not copy** its contents — it copies the **reference**. To create an actual copy (clone), you can use shallow cloning techniques:
```javascript
const arr = [1, 2, 3];

// Common ways to clone
const copy1 = arr.slice();
const copy2 = [...arr];
const copy3 = Array.from(arr);
const copy4 = arr.concat([]);

console.log(copy1); // [1, 2, 3]
```
:::warning
All of the above return a new array referencing the same primitive values. However, nested objects are still shared.
:::


### Array Utilities (isArray, at)
#### `Array.isArray()`
Checks whether a value is an actual array.

```javascript
Array.isArray([]);        // true
Array.isArray({});        // false
Array.isArray("string");  // false
```
#### `at()` Method
Returns an element at a specific index — supports negative indices.
```javascript
const numbers = [10, 20, 30, 40];
console.log(numbers.at(0));   // 10
console.log(numbers.at(-1));  // 40 (last element)
```
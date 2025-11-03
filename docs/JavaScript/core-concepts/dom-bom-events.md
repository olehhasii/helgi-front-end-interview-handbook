---
sidebar_position: 8
---

# DOM/BOM and Events

## DOM

### What is DOM?
The **DOM (Document Object Model)** is a **programming interface** that represents the structure of a web page as a **tree of objects**. It lets JavaScript access, modify, or delete elements dynamically on a web page. 

The DOM is structured as a hierarchical tree of nodes. Here are the main types of nodes:
| Node Type   | Example         | Description                     |
| :---------- | :-------------- | :------------------------------ |
| `Document`  | `document`      | The root of the DOM tree        |
| `Element`   | `<div>`, `<p>`  | Represents HTML tags            |
| `Text`      | `"Hello"`       | Represents text inside elements |
| `Attribute` | `class="box"`   | Describes element properties    |
| `Comment`   | `<!-- note -->` | Represents comments in HTML     |

### DOM Reflow and Repaint
When you modify the DOM or styles:
- **Reflow**: Browser recalculates element positions and layout.
- **Repaint**: Browser redraws pixels to reflect visual changes.

### What is BOM?
The BOM (Browser Object Model) is everything the browser provides to interact with the browser itself, not just the web page (that’s the DOM).
| Object                       | Purpose                              | Example                |
| :--------------------------- | :----------------------------------- | :--------------------- |
| `window`                     | Represents the browser window or tab | `window.innerWidth`    |
| `navigator`                  | Browser and device information       | `navigator.userAgent`  |
| `screen`                     | Display and resolution info          | `screen.width`         |
| `location`                   | Current URL and navigation control   | `location.href`        |
| `history`                    | Browsing history for the session     | `history.back()`       |
| `alert`, `confirm`, `prompt` | Browser dialogs                      | `alert("Hi!")`         |
| `setTimeout`, `setInterval`  | Timers                               | `setTimeout(fn, 1000)` |

### What is the DOM API?

The DOM API is exposed through the **global `document` object**, which serves as the **entry point** to the page’s content.

#### Core DOM Interfaces
| Interface                     | Purpose                                           | Example                          |
| :---------------------------- | :------------------------------------------------ | :------------------------------- |
| `Document`                    | Root of the DOM tree                              | `document.querySelector()`       |
| `Element`                     | Represents HTML elements                          | `div.textContent = "Hello"`      |
| `Node`                        | Base class for all nodes (text, comment, element) | `node.parentNode`                |
| `NodeList` / `HTMLCollection` | Lists of nodes                                    | Returned by `querySelectorAll()` |
| `Event`                       | Represents user or browser actions                | `click`, `input`, `load`         |

#### Querying Elements
```javascript
// By ID
const heading = document.getElementById("title");

// By class or tag
const items = document.getElementsByClassName("item");
const paragraphs = document.getElementsByTagName("p");

// Using CSS selectors
const first = document.querySelector(".item");
const all = document.querySelectorAll(".item");
```

#### DOM Traversal
```javascript
const list = document.querySelector("ul");

console.log(list.parentElement);   // Parent node
console.log(list.children);        // HTMLCollection of child elements
console.log(list.firstElementChild);
console.log(list.lastElementChild);
console.log(list.nextElementSibling);
console.log(list.previousElementSibling);

for (const child of list.children) {
  console.log(child.tagName);
}
```

#### How to modify element content and attributes?

```javascript
const p = document.querySelector("p");

p.textContent = "Updated text";     // Change text
p.innerHTML = "<strong>Bold text</strong>"; // Parse HTML inside
p.getAttribute("class");            // Read attribute
p.setAttribute("class", "new");     // Modify attribute
p.removeAttribute("hidden");        // Remove attribute
```

### How to create, insert, delete elements?

#### Creating Elements
You can create new elements using `document.createElement()`.
```javascript
const div = document.createElement("div");
div.textContent = "Hello!";
div.className = "greeting";
```
You can also create text nodes manually and append them to elements:
```javascript
const text = document.createTextNode("Hi there");
div.appendChild(text);
```

#### Inserting Elements
Insert elements into the DOM using built-in insertion methods.

```javascript
document.body.appendChild(div);   // Adds as last child
document.body.prepend(div);       // Adds as first child
```

Use `insertBefore()` to insert an element before a specific reference node:

```javascript
const parent = document.querySelector(".container");
const ref = document.querySelector(".item");

parent.insertBefore(div, ref); // Inserts before .item
```

Insert Using `insertAdjacentElement()`

```javascript
const parent = document.querySelector(".container");
const ref = document.querySelector(".item");

ref.insertAdjacentElement("beforebegin", div); // before ref
ref.insertAdjacentElement("afterbegin", div);  // inside ref (top)
ref.insertAdjacentElement("beforeend", div);   // inside ref (bottom)
ref.insertAdjacentElement("afterend", div);    // after ref
```

#### Replacing Elements
Replace an existing element with another using `replaceWith()`.
```javascript
const oldEl = document.querySelector(".old");
const newEl = document.createElement("p");

newEl.textContent = "Replaced!";
oldEl.replaceWith(newEl);
```

#### Deleting Elements
You can remove elements from the DOM using either of these methods:
```javascript
element.remove();               // Removes the element itself
parent.removeChild(child);      // Removes a child node explicitly
```

### How to manipulate CSS via JS?
You can manipulate styles in three main ways:
1. Directly through the element’s `style` property  
2. By toggling CSS classes (`classList`)  
3. By accessing computed styles with `getComputedStyle()`

#### Modify Inline Styles
```javascript
const box = document.querySelector(".box");

// Set styles
box.style.backgroundColor = "lightblue";
box.style.width = "200px";
box.style.border = "2px solid black";

// Read styles
console.log(box.style.backgroundColor); // "lightblue"
```
You can reset a style by setting it to an empty string or using `.removeProperty()`:
```javascript
box.style.backgroundColor = "";
box.style.removeProperty("border");
```

#### Manipulate CSS Classes
The classList API is the preferred modern way to add, remove, and toggle CSS classes.

```javascript
const btn = document.querySelector("button");

btn.classList.add("active");       // Add a class
btn.classList.remove("hidden");    // Remove a class
btn.classList.toggle("dark-mode"); // Toggle on/off
btn.classList.contains("active");  // Check if applied
```

#### Read Computed Styles
Sometimes, you need to know an element’s final computed style (after CSS rules, media queries, and inheritance are applied). Use `window.getComputedStyle()`:
```javascript
const box = document.querySelector(".box");
const styles = getComputedStyle(box);

console.log(styles.width);         // e.g., "200px"
console.log(styles.backgroundColor); // e.g., "rgb(173, 216, 230)"
```

#### Manipulate Stylesheets (Advanced)
You can even access and modify full CSS rules via the CSSStyleSheet API:
```javascript
const sheet = document.styleSheets[0];
sheet.insertRule(".highlight { color: red; }", sheet.cssRules.length);
```

### How to handle scrolling, viewport, coordinates?
JavaScript provides several **DOM and BOM APIs** to handle scrolling, get **viewport sizes**, and read **element coordinates**.

#### 1. Getting Scroll Position
The scroll position represents **how far the user has scrolled** horizontally and vertically.
```javascript
window.scrollX; // Horizontal scroll in pixels
window.scrollY; // Vertical scroll in pixels
```

#### 2. Programmatic Scrolling
You can scroll the page or elements using built-in methods.
```javascript
// Scroll to an absolute position
window.scrollTo({ top: 500, behavior: "smooth" });

// Scroll by a relative offset
window.scrollBy({ top: 100, left: 0, behavior: "smooth" });

// Scroll inside a specific element:
const box = document.querySelector(".scroll-box");
box.scrollTop = 200;  // Scroll down 200px inside the element
```

You can detect when the user scrolls using the scroll event.
```javascript
window.addEventListener("scroll", () => {
  console.log("Scroll position:", window.scrollY);
});
```

:::warning
⚠️ Scroll events fire very frequently — use requestAnimationFrame() or a throttle/debounce function for better performance.
:::

#### 4. Viewport and Page Dimensions
**Viewport size (visible area):**
```javascript
window.innerWidth;   // Width of the viewport
window.innerHeight;  // Height of the viewport
```

**Full document size:**
```javascript
document.documentElement.scrollHeight; // Total height of the page
document.documentElement.scrollWidth;  // Total width of the page
```

#### 5. Getting Element Coordinates
You can get element positions relative to the viewport or document using `getBoundingClientRect()`

`getBoundingClientRect()` Returns an object with coordinates and size relative to the viewport.

```javascript
const box = document.querySelector(".box");
const rect = box.getBoundingClientRect();

console.log(rect.top, rect.left); // Distance from viewport top/left
console.log(rect.width, rect.height); // Element size
```

#### 6. Scrolling an Element into View
To bring an element into the visible area of the viewport:
```javascript
element.scrollIntoView({ behavior: "smooth", block: "center" });
```
#### 7. Checking Element Visibility in Viewport
You can detect if an element is visible on screen using `getBoundingClientRect()`:

```javascript
function isInViewport(el) {
  const rect = el.getBoundingClientRect();
  return (
    rect.top >= 0 &&
    rect.left >= 0 &&
    rect.bottom <= window.innerHeight &&
    rect.right <= window.innerWidth
  );
}

window.addEventListener("scroll", () => {
  if (isInViewport(document.querySelector(".target"))) {
    console.log("Element is visible!");
  }
});
```

### What is Web Components / Shadow DOM?
**Web Components** are a set of **native browser technologies** that let you create **reusable, encapsulated custom elements** — without needing a framework.
They provide a way to build modular UI elements that:
- Have their own **markup**, **styles**, and **behavior**
- Work across projects and frameworks
- Are registered as **custom HTML tags**

#### Custom Elements
Define your own HTML element by extending the `HTMLElement` class.

Custom elements have special lifecycle methods:
| Callback                                             | When It Runs                       |
| :--------------------------------------------------- | :--------------------------------- |
| `connectedCallback()`                                | When inserted into the DOM         |
| `disconnectedCallback()`                             | When removed from the DOM          |
| `attributeChangedCallback(name, oldValue, newValue)` | When an observed attribute changes |
| `adoptedCallback()`                                  | When moved to a new document       |

```javascript
class TimerElement extends HTMLElement {
  connectedCallback() {
    this.start = Date.now();
    this.textContent = "Timer started!";
  }

  disconnectedCallback() {
    console.log("Timer stopped.");
  }
}

customElements.define("my-timer", TimerElement);
```

#### Shadow DOM
The Shadow DOM provides encapsulation for a component’s structure and styles — its internal DOM is hidden from the main document.
```javascript
class MyCard extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: "open" }); // Create shadow root
    this.shadowRoot.innerHTML = `
      <style>
        div { padding: 10px; background: #222; color: white; border-radius: 8px; }
      </style>
      <div>
        <slot></slot>
      </div>
    `;
  }
}

customElements.define("my-card", MyCard);
```
The styles inside the shadow DOM are scoped — they won’t leak out or be affected by global CSS.

##### Shadow DOM Modes
| Mode       | Description                                          |
| :--------- | :--------------------------------------------------- |
| `"open"`   | Accessible via `element.shadowRoot`                  |
| `"closed"` | Hidden from JavaScript (used internally by browsers) |

##### Slots (Content Projection)
The `<slot>` element defines where children passed from outside are rendered.

```javascript
<my-card>
  <h2>Title</h2>
  <p>Description</p>
</my-card>
```
In component
```javascript
<div>
  <slot></slot> <!-- external content inserted here -->
</div>

// You can have named slots too:
<slot name="footer"></slot>

<my-card>
  <span slot="footer">Footer content</span>
</my-card>
```

## Events

### What is the Event Object?
The **Event Object** in JavaScript represents information about an **event** that occurs in the browser — such as a user click, keypress, mouse move, scroll, form submission, or a custom event.

When an event handler is triggered, the browser automatically passes an **Event object** as an argument to the callback function.

#### Common Event Object Properties
| Property           | Description                              | Example                            |
| :----------------- | :--------------------------------------- | :--------------------------------- |
| `type`             | The type of event                        | `"click"`, `"keydown"`, `"submit"` |
| `target`           | The element on which the event occurred  | `event.target`                     |
| `currentTarget`    | The element the handler is attached to   | `event.currentTarget`              |
| `bubbles`          | Whether the event bubbles up through DOM | `true` / `false`                   |
| `cancelable`       | Whether the event can be prevented       | `true` / `false`                   |
| `timeStamp`        | Time when the event occurred (ms)        | `event.timeStamp`                  |
| `defaultPrevented` | Whether `preventDefault()` was called    | `true` / `false`                   |

Mouse-related events have additional properties:
| Property                                   | Description                                                      |
| :----------------------------------------- | :--------------------------------------------------------------- |
| `clientX`, `clientY`                       | Cursor position relative to viewport                             |
| `pageX`, `pageY`                           | Cursor position relative to the full page                        |
| `button`                                   | Which mouse button was pressed (`0=left`, `1=middle`, `2=right`) |
| `ctrlKey`, `shiftKey`, `altKey`, `metaKey` | Modifier key states                                              |

Keyboard events (keydown, keyup, keypress) include key info:
| Property                    | Description                                           |
| :-------------------------- | :---------------------------------------------------- |
| `key`                       | The character or key pressed (e.g., `"a"`, `"Enter"`) |
| `code`                      | Physical key identifier (e.g., `"KeyA"`, `"ArrowUp"`) |
| `repeat`                    | `true` if the key is held down                        |
| `ctrlKey`, `shiftKey`, etc. | Modifier states                                       |

#### Useful Methods
- `preventDefault()`: Stops default browser behavior (e.g., form submit, link navigation)
- `stopPropagation()`: Stops event from bubbling up
- `stopImmediatePropagation()`: Stops bubbling and prevents other listeners on same element
- `composedPath()`: Returns propagation path of event in DOM

### What is Event Bubbling, Capturing, and Event Delegation? 
In the browser’s **DOM event model**, when an event occurs (like a `click`), it doesn’t just stay on the target element — it **travels through the DOM tree**. This process is called **event propagation**, and it happens in **three phases**:
1. **Capturing phase** — the event moves from the document root **down to the target**
2. **Target phase** — the event reaches the actual element that was interacted with
3. **Bubbling phase** — the event travels **back up** from the target to the root

#### 1. Event Bubbling
Event bubbling means the event starts at the target element and then bubbles up through its ancestors until it reaches the document object.

```javascript
<div id="outer">
  <div id="inner">
    <button id="btn">Click Me</button>
  </div>
</div>

document.getElementById("outer").addEventListener("click", () => {
  console.log("Outer DIV clicked");
});

document.getElementById("inner").addEventListener("click", () => {
  console.log("Inner DIV clicked");
});

document.getElementById("btn").addEventListener("click", () => {
  console.log("Button clicked");
});

//Clicking the button logs:
//Button clicked
//Inner DIV clicked
//Outer DIV clicked
```
#### 2. Event Capturing (Trickling)
Event capturing (or trickling) is the opposite — the event travels from top to bottom, before reaching the target. To listen during the capturing phase, pass `true` as the third argument to event initialization:

```javascript
document.getElementById("outer").addEventListener(
  "click",
  () => console.log("Outer (capture)"),
  true // enables capturing phase
);

document.getElementById("btn").addEventListener("click", () =>
  console.log("Button (target)")
);

//Clicking the button logs:
//Outer (capture)
//Button (target)
```

#### Stopping Propagation
You can stop the event from continuing through the propagation chain.
```javascript
btn.addEventListener("click", (e) => {
  e.stopPropagation();
  console.log("Button only!");
});
```

#### Event Delegation
Event delegation is a technique that uses bubbling to handle events efficiently. Instead of attaching a listener to every child element, you attach one listener to a common ancestor and check which child triggered the event.

```javascript
<ul id="menu">
  <li>Home</li>
  <li>About</li>
  <li>Contact</li>
</ul>

const menu = document.getElementById("menu");

menu.addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    console.log("Clicked:", event.target.textContent);
  }
});
```

### How do you set an event?
There are three main ways to set events on elements:
1. **Inline HTML event attributes** (Old School – ❌ Not Recommended)
``javascript
<button onclick="alert('Clicked!')">Click me</button>
```

2. **Element property handlers**  
```javascript
const btn = document.querySelector("button");
btn.onclick = () => console.log("Button clicked!");
```

3. **Modern `addEventListener()` method**
```javascript
const btn = document.querySelector("button");

btn.addEventListener("click", (event) => {
  console.log("Clicked!", event.target);
});
```
| Parameter   | Description                                                                  |
| :---------- | :--------------------------------------------------------------------------- |
| `eventType` | Event name (e.g., `"click"`, `"input"`, `"keydown"`)                         |
| `callback`  | Function to execute when the event occurs                                    |
| `options`   | Optional object or boolean for extra settings (e.g., capture, once, passive) |


### Common Events
| Category      | Examples                                                                             |
| :------------ | :----------------------------------------------------------------------------------- |
| **Mouse**     | `click`, `dblclick`, `mousedown`, `mouseup`, `mousemove`, `mouseenter`, `mouseleave` |
| **Keyboard**  | `keydown`, `keyup`, `keypress`                                                       |
| **Form**      | `submit`, `input`, `change`, `focus`, `blur`                                         |
| **Window**    | `load`, `resize`, `scroll`, `beforeunload`                                           |
| **Clipboard** | `copy`, `paste`, `cut`                                                               |
| **Touch**     | `touchstart`, `touchmove`, `touchend`                                                |

### How to Create Custom Events in JavaScript
Custom events are created using the **`CustomEvent`** constructor and dispatched with **`dispatchEvent()`**.

```javascript
const event = new CustomEvent("userLogin");
document.dispatchEvent(event);

document.addEventListener("userLogin", () => {
  console.log("Custom event fired!");
});
```

You can attach additional data to your custom event using the detail property.
```javascript
const loginEvent = new CustomEvent("userLogin", {
  detail: { username: "Jonh", time: Date.now() },
});

document.addEventListener("userLogin", (e) => {
  console.log("User:", e.detail.username);
  console.log("Time:", new Date(e.detail.time));
});

document.dispatchEvent(loginEvent);
```
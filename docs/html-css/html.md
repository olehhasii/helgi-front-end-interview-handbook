---
sidebar_position: 1
---

# HTML

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

### What are tags and elements?
In HTML, **tags** and **elements** are the basic building blocks of a web page. They define the structure, meaning, and content that browsers use to render pages.

#### Tags
A **tag** is a piece of markup enclosed in angle brackets (`< >`). It tells the browser **how to interpret** or **display** the content inside it.

#### Elements
An element is the complete unit — consisting of:
- Opening tag
- Content (optional)
- Closing tag

### What are the main tags of an HTML page structure?
| Tag               | Description                                                                   |
| ----------------- | ----------------------------------------------------------------------------- |
| `<!DOCTYPE html>` | Declares the document type and HTML version (HTML5).                          |
| `<html>`          | The root element that wraps all content on the page.                          |
| `<head>`          | Contains metadata — information *about* the document (not shown on the page). |
| `<title>`         | Defines the title shown in the browser tab or window.                         |
| `<meta>`          | Provides metadata such as charset, viewport settings, and description.        |
| `<link>`          | Links external resources like CSS files or icons.                             |
| `<body>`          | Contains all **visible content** — text, images, buttons, etc.                |
| `<header>`        | Represents introductory or navigational content.                              |
| `<main>`          | Contains the main, unique content of the document.                            |
| `<footer>`        | Contains footer information — copyright, links, etc.                          |


### Semantic HTML
Semantic HTML means using HTML tags that describe the meaning and purpose of their content, not just how they look. It helps improve **accessibility**, **SEO**, and **code readability** by clearly describing the content’s purpose.

Instead of generic `<div>` or `<span>`, you use meaningful tags like:
| Element                     | Description                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `<header>`                  | Represents introductory content or navigation links.                |
| `<nav>`                     | Defines a block of navigation links.                                |
| `<main>`                    | Contains the central content unique to the page.                    |
| `<article>`                 | Represents a self-contained piece of content (e.g., a blog post).   |
| `<section>`                 | Groups related content within a document.                           |
| `<aside>`                   | Contains tangentially related content (like sidebars).              |
| `<footer>`                  | Contains information about the author, copyright, or related links. |
| `<figure>` / `<figcaption>` | Used for images or illustrations with captions.                     |

### Explain `<head>`, `<script>`, `<style>`, `<link>`, `<meta>`

#### `<head>`
Defines the **head section** of an HTML document. It includes elements like the title, links to stylesheets, scripts, and metadata.
```html
<head>
  <title>My Website</title>
  <meta charset="UTF-8">
  <meta name="description" content="A brief summary of the page.">
  <link rel="stylesheet" href="styles.css">
  <script src="app.js" defer></script>
</head>
```

#### `<script>`
Used to embed or reference JavaScript code.
```html
<!-- Inline script -->
<script>
  console.log("Hello, world!");
</script>

<!-- External file -->
<script src="app.js"></script>

<!-- Load after parsing the document -->
<script src="app.js" defer></script>

<!-- Load asynchronously (does not block parsing) -->
<script src="analytics.js" async></script>
```
- `src` — specifies an external JS file.
- `defer` — executes after HTML is fully parsed.
- `async` — executes as soon as the script is downloaded.

#### `<style>`
Defines internal CSS styles for the document. Usually used for small, page-specific styles. Larger projects should use external stylesheets via `<link>`.
```html
<style>
  body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
  }
</style>
```

#### `<link>`
Links external resources, most commonly stylesheets or icons.
```html
<link rel="stylesheet" href="styles.css">
<link rel="icon" href="/favicon.ico" type="image/x-icon">
```
- `rel` — describes the relationship (e.g., "stylesheet", "icon").
- `href` — specifies the URL of the linked resource.

#### `<meta>`
Provides metadata about the document — such as encoding, viewport settings, and SEO details.
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="Learn front-end development concepts.">
<meta name="author" content="Oleh Hasii">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```
- Define the character encoding (`charset`).
- Set the viewport for responsive design.
- Add SEO keywords and descriptions.
- Provide author or language information.

### Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`?
In a nutshell, such a placement of CSS `<link>` and JavaScript `<script>` allows for faster rendering of the page and better overall performance.

Placing CSS `<link>` in the `<head>` ensures that the stylesheets are loaded and ready for use when the browser starts rendering the page.

`<script>` tags block HTML parsing while they are being downloaded and executed which can slow down the display of your page. Placing the `<script>` at the bottom will allow the HTML to be parsed and displayed to the user first.

### What's the difference between an "attribute" and a "property" in the DOM?
- **Attributes** are defined in the **HTML markup**.  
- **Properties** are defined on the **DOM object** that the browser creates from that markup.

```javascript
<input type="checkbox" checked />

const checkbox = document.querySelector("input");

console.log(checkbox.getAttribute("checked")); // "": attribute exists in HTML
console.log(checkbox.checked);                 // true: DOM property reflects state

checkbox.checked = false;
console.log(checkbox.getAttribute("checked")); // still ""
```

### What are `data-` attributes?
`data-` attributes are custom HTML attributes that allow you to **store extra information** (metadata) directly on DOM elements. They’re useful for keeping small bits of data that don’t affect how the element is displayed or behave by default.

You can access them through the `dataset` property of the element.
```javascript
const user = document.getElementById("user");

console.log(user.dataset.id);     // "42"
console.log(user.dataset.role);   // "admin"
console.log(user.dataset.active); // "true"
```
### What is Shadow DOM?
The **Shadow DOM** is a feature of Web Components that lets you create a hidden, isolated DOM tree inside an element — so its HTML, CSS, and behavior are encapsulated and don’t affect (or get affected by) the rest of the page.

### Difference between document `load` event and document `DOMContentLoaded` event?
The `DOMContentLoaded` event fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading. 

The `load` event, on the other hand, fires when the entire page, including all dependent resources such as stylesheets and images, has finished loading.

### What does a `DOCTYPE` do?
The `<!DOCTYPE>` declaration tells the **browser which version of HTML** the document is written in — it ensures the browser **renders the page correctly** and follows the proper parsing rules. It must be the **very first line** in an HTML document, before the `<html>` tag.

### Explain what a single page app is and how to make one SEO-friendly
A single page application (SPA) is a web application that loads a single HTML page and dynamically updates content as the user interacts with the app. This approach provides a more fluid user experience but can be challenging for **SEO** because search engines may not execute JavaScript to render content.

To make an SPA SEO-friendly, you can use server-side rendering (SSR) or static site generation (SSG) to ensure that search engines can index your content. Tools like Next.js for React or Nuxt.js for Vue.js can help achieve this.

- Use Semantic HTML
- Optimize Meta Tags
- Use Clean URLs
- Improve Performance (Web Vitals)
- Accessibility & ARIA
- Internal Linking & Sitemap

### What can you say about the `<br>` tag?
The `<br>` tag in HTML creates a **line break** — it forces the content that follows to appear on a **new line** within the same block element.
- It’s an inline-level element, not a block element.
- It doesn’t add extra spacing, only breaks the line.

Avoid using `<br>` for general spacing or layout. For spacing between sections or elements, use CSS margin or padding instead.

### What are internal and external hyperlinks, and what attributes do they have?
**Hyperlinks** are elements that connect one page to another or navigate within the same page. In HTML, they are created using the `<a>` (anchor) tag.

#### Internal (Local)
Lead to a **page or section within the same website**.

```html
<!-- Link to another page on the same site -->
<a href="/about.html">About Us</a>

<!-- Link to a specific section on the same page -->
<a href="#contact">Contact</a>
```

#### External
Lead to resources outside the current website (different domain).

```javascript
<a href="https://developer.mozilla.org/" target="_blank" rel="noopener noreferrer">
  MDN Documentation
</a>
```

:::info
External links often open in a new tab (`target="_blank"`) and use security attributes like `rel="noopener noreferrer"`.
:::

#### Common Attributes of the `<a>` Tag
| Attribute  | Description                                                                                         | Example                                    |
| ---------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| `href`     | Specifies the URL or anchor the link points to.                                                     | `<a href="page.html">Link</a>`             |
| `target`   | Defines where to open the link: `_self`, `_blank`, `_parent`, `_top`.                               | `<a href="..." target="_blank">`           |
| `title`    | Tooltip text displayed when hovering over the link.                                                 | `<a href="#" title="Go to page">`          |
| `rel`      | Defines the relationship between the current page and the linked resource (important for security). | `<a href="..." rel="noopener noreferrer">` |
| `download` | Prompts the browser to download the linked file instead of navigating to it.                        | `<a href="file.pdf" download>Download</a>` |

### Ways to Add SVG to a Web Page?
1. Inline SVG - Directly include the `<svg>` markup inside the HTML.
Pros:
- Can be styled with CSS (fill, stroke, etc.).
- Can be controlled with JavaScript.
- Accessible and editable in the DOM.

Cons:
- Increases HTML file size if reused multiple times.

2. `<img>` Tag - Use the SVG file as a source like any other image.
Pros:
- Simple to use, caches easily.
- Works like PNG or JPG images.

Cons:
- Cannot be styled or accessed via CSS/JS internally (treated as an external resource).
- Limited interactivity.

3. CSS `background-image` - Include SVG via CSS for decorative or layout purposes.
Pros:
- Great for decorative icons or backgrounds.
- Keeps HTML clean.

Cons:
- No DOM access or interactivity.
- Harder to make accessible to screen readers.

4. `<object>`, `<embed>`, or `<iframe>` 


### Form, Form Validation
In HTML, a **form** is used to collect **user input** and send it to a server for processing.  

#### 🧾 Basic Structure of a Form
```html
<form action="/submit" method="POST">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required>

  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>

  <button type="submit">Submit</button>
</form>
```
| Attribute  | Description                                              |
| ---------- | -------------------------------------------------------- |
| `action`   | URL where the form data will be sent.                    |
| `method`   | HTTP method used to send data (`GET` or `POST`).         |
| `name`     | Identifies the input when sending data.                  |
| `required` | Makes the field mandatory.                               |
| `type`     | Defines the kind of input (text, email, password, etc.). |

#### Form Validation
Form validation can be HTML5-based (built-in) or custom (JavaScript). The browser automatically validates form fields based on attributes.
| Attribute                | Function                                         |
| ------------------------ | ------------------------------------------------ |
| `required`               | Field must be filled before submitting.          |
| `min`, `max`, `step`     | Numeric or date value limits.                    |
| `minlength`, `maxlength` | Restricts input length.                          |
| `pattern`                | Regular expression to match input.               |
| `type`                   | Automatically validates email, URL, number, etc. |

### `<input>`
The `<input>` tag is one of the most important form elements in HTML. It’s used to create **interactive controls** that let users enter data — such as text, numbers, passwords, or files — which can then be sent to a server or processed with JavaScript.

The `<input>` element is self-closing (it doesn’t have a closing tag) and its behavior depends mainly on the value of the `type` attribute.
| Attribute            | Description                                                          | Example                                            |
| -------------------- | -------------------------------------------------------------------- | -------------------------------------------------- |
| `type`               | Specifies the kind of input control (text, email, password, etc.).   | `<input type="email">`                             |
| `name`               | Name of the field — used as the key when sending data to the server. | `<input name="email">`                             |
| `value`              | Default or current value of the input.                               | `<input value="Oleh">`                             |
| `placeholder`        | Hint text displayed inside the input when it’s empty.                | `<input placeholder="Enter your name">`            |
| `required`           | Makes the field mandatory before form submission.                    | `<input required>`                                 |
| `disabled`           | Disables the field (cannot be edited or submitted).                  | `<input disabled>`                                 |
| `readonly`           | Field is visible but cannot be modified.                             | `<input readonly>`                                 |
| `maxlength`          | Sets the maximum number of characters allowed.                       | `<input maxlength="20">`                           |
| `min`, `max`, `step` | Used with numeric and date inputs to restrict values.                | `<input type="number" min="0" max="100" step="5">` |
| `pattern`            | Regular expression that the input value must match.                  | `<input pattern="[A-Za-z]+">`                      |
| `autocomplete`       | Suggests previously entered values.                                  | `<input autocomplete="on">`                        |

Input Types
| Type                             | Description                             | Example                                  |
| -------------------------------- | --------------------------------------- | ---------------------------------------- |
| `text`                           | Single-line text field.                 | `<input type="text">`                    |
| `password`                       | Hides input characters.                 | `<input type="password">`                |
| `email`                          | Validates email format.                 | `<input type="email">`                   |
| `number`                         | Numeric input with increment buttons.   | `<input type="number" min="0">`          |
| `tel`                            | Telephone number field.                 | `<input type="tel">`                     |
| `url`                            | Validates URL format.                   | `<input type="url">`                     |
| `checkbox`                       | Allows multiple selections.             | `<input type="checkbox">`                |
| `radio`                          | Allows a single selection from a group. | `<input type="radio" name="gender">`     |
| `range`                          | Slider control for numeric input.       | `<input type="range" min="0" max="100">` |
| `color`                          | Color picker input.                     | `<input type="color">`                   |
| `date`, `time`, `datetime-local` | Date and time inputs.                   | `<input type="date">`                    |
| `file`                           | Uploads a file.                         | `<input type="file">`                    |
| `hidden`                         | Invisible input used to store data.     | `<input type="hidden" value="123">`      |
| `submit`, `reset`, `button`      | Form control buttons.                   | `<input type="submit">`                  |

### What methods of sending form data do you know? How to send a form to a server (without JS)?
HTML forms can send user input to a server **automatically**, without any JavaScript. The way data is sent depends on the **`method`** and **`enctype`** attributes of the `<form>` element.

Forms can be submitted entirely via HTML:
- Use action to specify the target URL.
- Use method (GET or POST).
- Add `<button type="submit">` or `<input type="submit">`.
- The browser automatically handles submission and page navigation.

When the user clicks “Send,” the browser:
- Collects all form fields (name=value pairs).
- Encodes the data according to the enctype.
- Sends it to the URL in the action attribute using the specified method.
- Loads the server’s response as a new page.

### What are the types of lists in HTML? What are `ul`, `ol`, `dl`?
HTML provides several ways to display information in **list format**, helping organize related content for readability and structure.  

There are **three main types** of lists: unordered, ordered, and description lists.

#### Unordered List — `<ul>`
An unordered list is used when the **order of items doesn’t matter**. Each item is marked with a **bullet point** by default.

```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>
```

`type`: defines bullet style (disc, circle, square).

#### Ordered List — `<ol>`
An ordered list is used when the order of items matters, such as steps or rankings. Items are automatically numbered.

- `type`: numbering style (1, A, a, I, i).
- `start`: set the starting number.
- `reversed`: displays the list in reverse order.
```html
<ol type="A" start="3" reversed>
  <li>Step C</li>
  <li>Step B</li>
  <li>Step A</li>
</ol>
```

#### Description List — `<dl>`
A description list is used for name–value pairs, such as terms and definitions.

Tags:
- `<dl>` — description list container.
- `<dt>` — definition term.
- `<dd>` — definition description.

```html
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language – defines the structure of web pages.</dd>

  <dt>CSS</dt>
  <dd>Cascading Style Sheets – defines the visual presentation.</dd>

  <dt>JavaScript</dt>
  <dd>Adds interactivity and dynamic behavior to web pages.</dd>
</dl>
```

### What is an iframe and what is it used for?
The `<iframe>` (inline frame) element allows you to **embed another HTML document** inside the current web page. It’s like placing a **window** or **mini browser** inside your page that displays external or internal content independently.

```html
<iframe src="https://example.com" width="600" height="400"></iframe>
```

#### Common Use Cases
- Embedding YouTube videos, maps, or external widgets.
- Displaying content from another page of the same site.
- Loading third-party services (e.g., payment forms, analytics dashboards).
- Sandboxing external content for security isolation.

Important Attributes
| Attribute          | Description                                                    | Example                                 |
| ------------------ | -------------------------------------------------------------- | --------------------------------------- |
| `src`              | URL of the page to display.                                    | `<iframe src="page.html">`              |
| `width` / `height` | Dimensions of the frame (in px or %).                          | `<iframe width="600" height="400">`     |
| `title`            | Describes the content for accessibility.                       | `<iframe title="Google Map">`           |
| `frameborder`      | (Deprecated) Defines border visibility. Use CSS instead.       | `style="border:none"`                   |
| `allowfullscreen`  | Allows full-screen mode for videos or content.                 | `<iframe allowfullscreen>`              |
| `loading="lazy"`   | Defers loading until the iframe enters the viewport.           | `<iframe loading="lazy">`               |
| `sandbox`          | Restricts capabilities (scripts, forms, pop-ups) for security. | `<iframe sandbox>`                      |
| `referrerpolicy`   | Controls what referrer info is sent when loading content.      | `<iframe referrerpolicy="no-referrer">` |

#### How to communicate with an `<iframe>`
By default, an `<iframe>` runs its content in an **isolated browsing context** — meaning the parent page and the iframe have **separate JavaScript environments**.  

However, HTML5 introduced the **`postMessage()` API**, which allows **secure two-way communication** between a parent page and an embedded iframe.

Communication works via **message passing**:
- The **parent page** can send data to the iframe.
- The **iframe** can send data back to the parent.
- Both listen for the `message` event.

Both the parent and the iframe must listen for messages using the `message` event.

```javascript
<!-- Parent page -->
<iframe id="childFrame" src="child.html"></iframe>

<script>
  const iframe = document.getElementById("childFrame");

  // Send a message to the iframe
  iframe.onload = () => {
    iframe.contentWindow.postMessage(
      { type: "GREETING", text: "Hello from parent!" },
      "https://your-iframe-domain.com" // target origin
    );
  };
</script>

window.addEventListener("message", (event) => {
  // Always verify the origin for security
  if (event.origin !== "https://your-iframe-domain.com") return;

  console.log("Received from iframe:", event.data);
});
```

### What is microdata (micro markup)?
**Microdata (micro markup)** is a way to add **semantic meaning** to web page content so that search engines and other tools can better **understand what the data represents**, not just how it looks.

Microdata uses three main attributes:
- `itemscope` — declares a block of data (an item).  
- `itemtype` — specifies the type of the item (using a vocabulary URL, usually from [schema.org](https://schema.org/)).  
- `itemprop` — defines a property (field) of the item.

Example:
```html
<div itemscope itemtype="https://schema.org/Person">
  <span itemprop="name">Jonh Bin</span><br>
  <span itemprop="jobTitle">Front-end Developer</span><br>
  <a href="mailto:jonh@example.com" itemprop="email">jonh@example.com</a>
</div>
```

### Why do some tags (like `<img>`) not have pseudo-elements? 
Some HTML elements — such as `<img>`, `<input>`, `<br>`, and others — **do not support pseudo-elements** like `::before` and `::after` because of how they work in the document’s structure and rendering model.

1. No Content Model

- Pseudo-elements work only for elements that can contain text or child nodes.
- `<img>` is a replaced element, meaning it doesn’t have inner content — the image itself replaces the element’s box.

2. Rendered as External Resource

- The `<img>` element displays an external image file (JPEG, PNG, SVG, etc.).
- There’s no inner DOM structure for pseudo-elements to attach to.

3. Replaced Elements Behavior

- CSS treats `<img>`, `<video>`, `<iframe>`, and `<input>` as replaced elements: their content is handled by the browser’s rendering engine, not the CSS box model.
- As a result, pseudo-elements can’t generate additional content inside or around them.

:::tip
If you want to add decorative content around an image, wrap it in a container and apply pseudo-elements there.
```html
<div class="image-wrapper">
  <img src="photo.jpg" alt="Example">
</div>
```
:::

### Why would you use a `srcset` attribute in an image tag?
The **`srcset`** attribute is used to provide **multiple versions of the same image** for different screen sizes and resolutions. It helps browsers automatically choose the **most appropriate image** to display — improving both **performance** and **visual quality** across devices.

The `srcset` attribute works together with the `sizes` attribute to define:
- A list of available images (`srcset`)
- The conditions under which each image should be used (`sizes`)

```html
<img
  src="image-600.jpg"
  srcset="
    image-400.jpg 400w,
    image-800.jpg 800w,
    image-1200.jpg 1200w"
  sizes="(max-width: 600px) 100vw, 50vw" 
  alt="Example image">

  // sizes="(max-width: 600px) 100vw, 50vw" - If the viewport is ≤ 600px → image takes 100% of the viewport width. Otherwise → it takes 50% of the viewport width.
```

### How to ensure website accessibility with HTML and CSS?

#### 1. Use Semantic HTML
Semantic tags help browsers, search engines, and screen readers **understand the structure and meaning** of your page.

#### 2. Proper Heading Levels (`<h1>`–`<h6>`)
Use headings to define hierarchy, not just to make text bigger: Screen readers can list headings and let users jump to sections.

#### 3. Provide Text Alternatives for Images
Always include an `alt` attribute to describe an image’s purpose.

#### 4. Ensure Sufficient Color Contrast
Text and background should have enough contrast to remain readable.

#### 5. Focus States (Keyboard Navigation)
Users navigating via keyboard must see which element is focused.

#### 6. ARIA Attributes (When Needed)
ARIA (Accessible Rich Internet Applications) attributes add extra semantic meaning when native HTML tags aren’t enough.

#### 7. Keyboard Accessibility
Make sure all interactive elements (buttons, links, inputs) are reachable via keyboard:
- Use `<button>` for actions instead of `div` or `span`.
- Keep logical tab order.
- Avoid removing or misusing `tabindex`.

#### 8. CSS for Accessibility
- Don’t rely on `color` alone to convey meaning (use icons or text).
- Use relative units (`em`, `rem`) to allow text resizing.
- Make layouts responsive.
- Don’t disable browser zooming.

### How to Make a `<div>` Clickable — The Right Way
You **can** make a `<div>` respond to clicks, but since `<div>` is **not an interactive element by default**, you must add the **correct semantics and accessibility features** to make it work properly for all users.

Make it behave like a button using proper roles and attributes.
```html
<div
  class="card"
  role="button"
  tabindex="0"
  aria-label="Open settings"
  onclick="doSomething()"
  onkeydown="if(event.key === 'Enter' || event.key === ' ') doSomething()"
>
  ⚙️ Settings
</div>
```
| Attribute       | Purpose                                                       |
| --------------- | ------------------------------------------------------------- |
| `role="button"` | Tells screen readers it behaves like a button                 |
| `tabindex="0"`  | Makes it focusable via keyboard (Tab key)                     |
| `aria-label`    | Adds a text label for screen readers                          |
| `onkeydown`     | Allows `Enter` and `Space` to trigger action, not just clicks |

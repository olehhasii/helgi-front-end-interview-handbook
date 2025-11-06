---
sidebar_position: 2
---

# CSS

### What is CSS?
CSS (Cascading Style Sheets) is the language used to style and visually format HTML elements on a web page. It controls things like colors, fonts, spacing, layout, and animations.

The term **“cascading”** means styles are applied in order of priority — browser defaults → external styles → inline styles — and the most specific rule wins.
1. Specificity (inline > ID > class > element)
2. Source order (last rule overrides previous ones)
3. Importance (!important has highest priority)

#### Ways to Add CSS
1. Inline CSS - Applied directly to an HTML element using the `style` attribute.
```html
<p style="color: red;">Inline styled text</p>
```
2. Internal (Embedded) CSS - Placed inside a `<style>` tag in the HTML `<head>`.
```html
<head>
  <style>
    p { color: green; }
  </style>
</head>
```
3. External CSS (Recommended) - Linked as a separate .css file. `<link rel="stylesheet" href="styles.css">`


### CSSOM
The **CSS Object Model (CSSOM)** is a programming interface (API) that represents all the **CSS styles and rules** of a web page in a structured, object-oriented way — similar to how the **DOM (Document Object Model)** represents the HTML structure.

### What is Box model?
The **CSS Box Model** describes how every HTML element is represented as a **rectangular box** that determines how content is displayed, sized, and spaced on a web page. It defines the **layout structure** of elements — how padding, borders, and margins affect their total size and positioning.

Rectangular box made up of four layers:
- **Content** – the actual text or image inside.
- **Padding** – space between content and the border.
- **Border** – wraps around the padding and content.
- **Margin** – space outside the border, separating the element from others.

### What does `* { box-sizing: border-box; }` do?
The `box-sizing` property defines how the total width and height of an element are calculated.
- `content-box` The width/height apply only to the content box. Padding and borders are added on top.
- `border-box`: The width/height include content, padding, and border.


### What is the CSS `display` property?
The `display` property defines how an element is shown in the page layout — basically, how it behaves in the document flow.
| Value                              | Description                                                                                                          |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `block`                            | The element starts on a new line and takes up the **full width** available. Examples: `<div>`, `<p>`, `<section>`.   |
| `inline`                           | The element **does not start a new line** and only takes up as much width as its content. Examples: `<span>`, `<a>`. |
| `inline-block`                     | Like `inline`, but allows setting **width and height**.                                                              |
| `none`                             | The element is **removed from the layout flow** (hidden entirely).                                                   |
| `flex`                             | Makes the element a **flex container**; its children become flex items.                                              |
| `grid`                             | Makes the element a **grid container**; its children align to a grid layout.                                         |
| `inline-flex`                      | Same as `flex`, but behaves like an inline element.                                                                  |
| `inline-grid`                      | Same as `grid`, but inline.                                                                                          |
| `contents`                         | Makes the element disappear visually but keeps its children in the layout flow.                                      |
| `table`, `table-row`, `table-cell` | Used to mimic table layouts without actual `<table>` elements.                                                       |

### Difference between Block / Inline / Inline - Block?

1. `block`: 
    - **Size**: Fills up the width of its parent container.
    - **Positioning**: Start on a new line
    - **Can** specify width and height

2. `inline`
    - **Size**: Depends on content.
    - **Positioning**: Flows along with other content and allows other elements beside it.
    - **Can’t** specify width and height
    - **Can** be aligned with `vertical-align`

3. `inline-block`: 
    - Same as `inline`, but **can** specify width and height.
    - Used for buttons, images, and form fields that need custom sizes but stay in line with text.


### Positioning (Relative, Absolute, Fixed, Sticky)
The **`position`** property in CSS determines **how an element is placed in the document flow** — whether it moves relative to its normal position, another element, or the viewport.

1. `static` (default) - Default positioning for all elements. The element stays in **normal document flow**. `top`, `left`, `bottom`, and `right` have no effect.
2. `relative` - The element **remains in the document flow**. Positioned relative to its **original place**. You can move it with `top`, `left`, etc., but it still keeps its original space. Other elements do not move to fill its old space. Commonly used as a positioning context for absolutely positioned child elements.
3. `absolute` - The element is **removed from the normal flow**. Positioned relative to the nearest ancestor that has `position: relative`, `absolute`, or `fixed`. If no such ancestor exists, it’s positioned relative to the `<html>` (page itself). Does not affect the layout of other elements - — other elements ignore it. 
4. `fixed` - The element is **fixed to the viewport**, not affected by scrolling. Always stays in the same place (like sticky navigation bars). Removed from normal document flow. Position values (`top`, `left`, etc.) are relative to the viewport.
5. Acts like `relative` until the element reaches a certain scroll position, then behaves like `fixed`. Must have a defined `top`, `left`, `right`, or `bottom`. Works only within its parent container. Great for “sticky headers” or “sticky sidebars”.

### Describe `z-index` and how stacking context is formed
The **`z-index`** property in CSS controls the **stacking order** of elements along the **z-axis** (the imaginary depth axis perpendicular to the screen). 

`z-index` only applies when the element has a positioning context: `relative` | `absolute` | `fixed` | `sticky`;

It determines **which elements appear in front of or behind** others when they overlap.
- Higher z-index values appear closer to the viewer.
- Lower values appear behind.
- The z-index only works on elements with a positioning context:  `relative`, `absolute`, `fixed`, or `sticky`.

When z-index is not specified, elements are stacked in this order (from back to front):
1. Background and borders of the root element.
2. Elements positioned with z-index < 0.
3. Non-positioned elements (static flow).
4. Positioned elements with z-index: auto or 0.
5. Elements with higher positive z-index values.

#### What Is a Stacking Context?
A stacking context is a self-contained “layering system” that defines how child elements overlap within that context. Elements from different stacking contexts do not affect each other’s `z-index` values.

Think of it as layers of transparent sheets — each stacking context is one sheet, and inside it, child elements can stack independently.

```html
<div class="parent">
  <div class="child"></div>
</div>

.parent {
  position: relative;
  z-index: 1;          /* Creates a new stacking context */
  background: lightblue;
}

.child {
  position: absolute;
  z-index: 999;        /* High value, but only within parent */
  background: coral;
}
```
Even though `.child` has `z-index: 999`, it cannot overlap elements outside of `.parent’s` stacking context if those external elements have higher stacking order.

### What Units Does CSS Have?
Units fall into two main categories:
- **Absolute units** → fixed sizes that don’t change.  
- **Relative units** → sizes that depend on another value (like parent size, viewport, or font size).

#### Absolute Units
Absolute units represent **fixed physical sizes** (do not change with screen or parent size).

| Unit | Name | Equivalent | Description |
|------|------|-------------|--------------|
| `px` | Pixels | 1 px = 1/96 inch | Most common; relative to device pixels (logical pixels). |
| `cm` | Centimeters | 1 cm = 96px / 2.54 | Rarely used on screens. |
| `mm` | Millimeters | 1 mm = 1/10 of 1 cm | For print layouts. |
| `in` | Inches | 1 in = 2.54 cm = 96 px | For print or DPI settings. |
| `pt` | Points | 1 pt = 1/72 inch | Used in print typography. |
| `pc` | Picas | 1 pc = 12 pt | Also print-oriented. |

#### Relative units
Relative units scale based on context, such as font size or viewport size. They are key for responsive design.

1. Font-relative Units

| Unit  | Relative To                                    | Example                                      |
| ----- | ---------------------------------------------- | -------------------------------------------- |
| `em`  | Font size of the element (or parent)           | `2em` = twice the element’s font size        |
| `rem` | Root element’s (`<html>`) font size            | `1rem` = default root font size (often 16px) |
| `ex`  | x-height of the font (height of lowercase “x”) | Rarely used                                  |
| `ch`  | Width of the “0” (zero) character              | Useful for monospace layouts                 |

2. Viewport-relative Units

| Unit                | Relative To                                                              | Description                        |
| ------------------- | ------------------------------------------------------------------------ | ---------------------------------- |
| `vw`                | 1% of viewport width                                                     | `50vw` = half the browser width    |
| `vh`                | 1% of viewport height                                                    | `100vh` = full screen height       |
| `vmin`              | 1% of the smaller dimension (width or height)                            | Useful for square scaling          |
| `vmax`              | 1% of the larger dimension                                               | Scales with the largest side       |
| `svw`, `lvw`, `dvw` | Newer viewport units (small, large, dynamic viewports) for mobile safety | Handle browser UI bars dynamically |

3. Other Special Units

| Unit                  | Description                                                                 |
| --------------------- | --------------------------------------------------------------------------- |
| `%`                   | Percentage relative to the parent element’s property (width, height, etc.). |
| `fr`                  | “Fractional unit” used in **CSS Grid** to allocate available space.         |
| `deg`, `rad`, `turn`  | Angular units (used for rotations).                                         |
| `s`, `ms`             | Time units (for animations and transitions).                                |
| `Hz`, `kHz`           | Frequency (rarely used).                                                    |
| `dpi`, `dppx`, `dpcm` | Resolution units (for media queries).                                       |

### CSS Selectors (Class, ID, Combinators, Pseudo-classes)
**CSS selectors** define **which elements** on a web page a style rule applies to. Selectors can target elements by type, class, ID, relationships, state, and much more.

#### Basic Selectors
- Type (Element) Selector - Selects all instances of a specific HTML tag.
```css
p {
  color: gray;
}
```

- Class Selector - Targets elements with a specific `class` attribute.
```css
.highlight {
  background: yellow;
}
```

- ID Selector - Targets a single element with a unique `id` attribute.

#### Attribute Selectors
Select elements based on attributes and their values.
| Selector         | Matches                             |
| ---------------- | ----------------------------------- |
| `[attr]`         | Elements with the attribute.        |
| `[attr="value"]` | Attribute equals a value.           |
| `[attr~="word"]` | Attribute contains a specific word. |
| `[attr^="val"]`  | Attribute starts with a value.      |
| `[attr$="val"]`  | Attribute ends with a value.        |
| `[attr*="val"]`  | Attribute contains a substring.     |
```css
input[type="email"] {
  border-color: blue;
}
```

#### Combinators
Combinators define relationships between elements.
| Combinator                 | Meaning                                 | Example                            |
| -------------------------- | --------------------------------------- | ---------------------------------- |
| **Descendant** (` `)       | Selects elements inside another.        | `div p` → all `<p>` inside `<div>` |
| **Child** (`>`)            | Direct children only.                   | `ul > li`                          |
| **Adjacent sibling** (`+`) | Next element immediately after another. | `h2 + p`                           |
| **General sibling** (`~`)  | All siblings that follow.               | `h2 ~ p`                           |

#### Pseudo-classes
Pseudo-classes target elements in a specific state or condition.

```css
a:hover { color: red; }       /* When hovered */
a:active { color: orange; }   /* When clicked */
input:focus { outline: 2px solid blue; } /* Focused input */

ul li:first-child { color: green; }
ul li:last-child { color: red; }
ul li:nth-child(2) { font-weight: bold; }
```

### How to Make the Last Item in a List Red (Without JavaScript)
You can style the **last element** of a list dynamically using the CSS **`:last-child`** or **`:last-of-type`** pseudo-class.  
```css
ul li:last-child {
  color: red;
  font-weight: bold;
}
```

### Explain how specificity is calculated in CSS selectors
**Specificity** determines **which CSS rule takes precedence** when multiple selectors target the same element. It’s calculated based on the **types of selectors** used — not the order they appear in (except when specificity ties).

Specificity is represented as **a four-part numeric value**: ( inline, IDs, classes/attributes/pseudo-classes, elements/pseudo-elements )

You can think of it as a **4-digit number**:

where each letter represents:

| Category | Example | Value |
|-----------|----------|--------|
| **A** – Inline styles | `<div style="color:red">` | 1000 |
| **B** – ID selectors | `#header`, `#main` | 100 |
| **C** – Classes, attributes, pseudo-classes | `.title`, `[type="text"]`, `:hover` | 10 |
| **D** – Element and pseudo-element selectors | `div`, `h1`, `::before` | 1 |

####

| Selector | Specificity | Explanation |
|-----------|--------------|--------------|
| `div` | 0,0,0,1 | One element selector |
| `.item` | 0,0,1,0 | One class selector |
| `#nav` | 0,1,0,0 | One ID selector |
| `ul li a` | 0,0,0,3 | Three element selectors |
| `.menu a:hover` | 0,0,2,1 | One class + one pseudo-class + one element |
| `header .menu li.active a` | 0,0,3,2 | Three classes + two elements |
| `#header .nav a` | 0,1,1,1 | One ID, one class, one element |
| `style="color:red;"` | 1,0,0,0 | Inline style — always wins over CSS rules |

1. Compare **A → B → C → D** (from left to right).  
   The first difference decides which rule wins.
2. If specificity values are **equal**, the **last defined rule** in the stylesheet takes precedence.
3. `!important` overrides normal specificity — but only **within the same origin** (author vs user styles).

### Tell about Flexbox
**Flexbox** is a modern CSS layout model designed to create **one-dimensional layouts** — arranging elements **in a row or a column**, with flexible alignment and spacing. It makes it easy to center elements, distribute space evenly, and adapt layouts to different screen sizes.

Flexbox consists of:
- A **container** (`display: flex`)  
- Its **items** (direct child elements)

Example:
```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>
```
Flexbox has two axes:
- **Main axis** → determined by `flex-direction` (default: `row`)
- **Cross axis** → perpendicular to the main axis

#### Flex Container Properties
- `display` - `flex`, `inline-flex`
- `flex-direction` - Controls the direction of flex items.
```css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
- `flex-flow` - Shorthand for `flex-direction` and `flex-wrap`.
```css
.container {
  flex-flow: row wrap;
}
```
- `justify-content` - Aligns items along the **main** axis.
```css
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}
```
- `align-items` - Aligns items along the cross axis.
```css
.container {
  align-items: stretch | flex-start | flex-end | center | baseline;
}
```

- `align-content` - Aligns multiple lines (if wrapping) along the cross axis.
```css
.container {
  align-content: stretch | flex-start | flex-end | center | space-between | space-around;
}
```

#### Flex Item Properties
- `order` - Controls the visual order of items (default is 0).
```css
.item {
  order: 2;
}
```

- `flex-grow` - Defines how much an item can grow relative to others.
```css
.item {
  flex-grow: 1; /* fill remaining space equally */
}
```

- `flex-shrink` - Defines how much an item can shrink if space is limited.
```css
.item {
  flex-shrink: 0; /* prevent shrinking */
}
```

- `flex-basis` - Sets the initial size of an item before growing/shrinking.
```css
.item {
  flex-basis: 200px;
}
```

- `flex` (shorthand)
```css
.item {
  flex: flex-grow flex-shrink flex-basis;
}
.item {
  flex: 1 0 150px; /* grow:1, shrink:0, basis:150px */
}
```
- `align-self` - Overrides align-items for a specific item.

### Grid Layout
**CSS Grid** is a powerful 2D layout system that allows you to design complex web layouts using **rows and columns**.  

Unlike Flexbox (which is one-dimensional), **Grid works in both directions** — horizontally and vertically — making it ideal for full-page or component-level layouts.

A **grid container** is an element with `display: grid`, and its **direct children** become **grid items**.

#### Key Grid Properties
- `grid-template-columns` / `grid-template-rows` - Defines the structure of columns and rows. `px`, `%`, `fr` (fractional unit) values allowed.
```css
.grid {
  grid-template-columns: 200px 1fr 2fr;
  grid-template-rows: 100px auto;
}
```
- `gap` - Controls spacing between rows and columns.
```css
.grid {
  gap: 20px;             /* shorthand for row + column gap */
  row-gap: 10px;
  column-gap: 20px;
}
```

- `grid-template-areas` - Names parts of the grid for easier placement.
```css
.grid {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 200px 1fr;
  grid-template-rows: 60px 1fr 50px;
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

#### Grid Items

- `grid-column`: / `grid-row:` → specify where an item should start and end
```css
.item1 {
  grid-column: 1 / 3; /* spans columns 1 to 2 */
  grid-row: 1 / 2;    /* occupies first row */
}
```
- Auto-fit & Auto-fill - Automatically adjust item count based on available space.
```css
.grid {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

  - `minmax(200px, 1fr)` → each item is at least `200px` wide but grows up to available space.
  - `auto-fit` compresses empty tracks, auto-fill reserves them.

- Using Named Lines
```css
.grid {
  grid-template-columns: [start] 1fr [middle] 2fr [end];
}
.item {
  grid-column: start / end;
}
```

#### Alignment in Grid
| Property          | Applies To | Description                                          |
| ----------------- | ---------- | ---------------------------------------------------- |
| `justify-items`   | Items      | Aligns items **horizontally** within each cell       |
| `align-items`     | Items      | Aligns items **vertically** within each cell         |
| `place-items`     | Items      | Shorthand for both (`align-items` + `justify-items`) |
| `justify-content` | Container  | Aligns the entire grid **horizontally**              |
| `align-content`   | Container  | Aligns the entire grid **vertically**                |
| `place-content`   | Container  | Shorthand for both                                   |

#### `fr` (fraction) unit
In CSS Grid, the `fr` (fraction) unit represents a portion of the available space in the grid container. It’s used in `grid-template-columns` or `grid-template-rows` to distribute space flexibly.
- `1fr` = one share of remaining space
- `2fr` = twice as much as `1fr`

### What are the methods for centering an element in CSS?
| Method                   | Works For             | Centering Type        | Notes                    |
| ------------------------ | --------------------- | --------------------- | ------------------------ |
| **Flexbox**              | Any element           | Horizontal & Vertical | Modern, easy, responsive |
| **Grid**                 | Any element           | Horizontal & Vertical | One-liner solution       |
| **Absolute + Transform** | Absolutely positioned | Horizontal & Vertical | Works without modern CSS |
| **Margin auto**          | Block-level           | Horizontal only       | Needs width              |
| **Text-align center**    | Inline/text           | Horizontal only       | Simple and quick         |
| **text-align**           | Text only             | Vertical only         | Fixed-height containers  |
| **Fixed + Transform**    | Popups, modals        | Horizontal & Vertical | Centers across viewport  |


### Media Queries
**Media Queries** are a feature of CSS used to create **responsive designs** — layouts that adapt to different screen sizes, resolutions, orientations, and devices.

They allow you to apply CSS rules **conditionally**, depending on the characteristics of the device or viewport.
```css
@media media-type and (condition) {
  /* CSS rules go here */
}

/* This rule applies only when the screen width is 768px or less (e.g., tablets, phones). */
@media screen and (max-width: 768px) {
  body {
    background-color: lightblue;
  }
}
```

There are four types of `@media` properties (including `screen`):
- `all`: for all media type devices
- `print`: for printers
- `speech`: for screen readers that "reads" the page out loud
- `screen`: for computer screens, tablets, smart-phones etc.

#### Common Media Features
| Feature                          | Description                       | Example                        |
| -------------------------------- | --------------------------------- | ------------------------------ |
| `width` / `height`               | Viewport width or height          | `(max-width: 600px)`           |
| `device-width` / `device-height` | Physical screen size              | `(min-device-width: 320px)`    |
| `orientation`                    | Device orientation                | `(orientation: landscape)`     |
| `resolution`                     | Screen resolution (dpi, dppx)     | `(min-resolution: 2dppx)`      |
| `prefers-color-scheme`           | Light or dark mode                | `(prefers-color-scheme: dark)` |
| `hover`                          | Whether the device supports hover | `(hover: none)`                |
| `pointer`                        | Input precision (mouse, touch)    | `(pointer: coarse)`            |

#### Logical Operators
- `and` - Combine multiple conditions.
```css
@media screen and (min-width: 600px) and (orientation: landscape) {
  /* rules apply if both are true */
}
```
- `not` - Exclude specific conditions.
```css
@media not screen and (max-width: 800px) {
  /* applies to all except small screens */
}
```
- `only` - Prevents older browsers from applying unsupported queries.
```css
@media only screen and (max-width: 600px) {
  /* safe mobile rules */
}
```
- `,` (comma) — OR operator - Applies if any condition is true.
```css
@media (max-width: 600px), (orientation: portrait) {
  /* applies if width ≤ 600px OR portrait mode */
}
```
#### Breakpoints (Common Practice)
Developers often define breakpoints for device categories:
| Device        | Max Width |
| ------------- | --------- |
| Small phones  | 480px     |
| Tablets       | 768px     |
| Small laptops | 1024px    |
| Desktops      | 1200px+   |
```css
@media (max-width: 480px) { /* phones */ }
@media (max-width: 768px) { /* tablets */ }
@media (max-width: 1024px) { /* small laptops */ }
```

### How is responsive design different from adaptive design?
Both **responsive** and **adaptive** design aim to make websites look and work well across various screen sizes — from phones to desktops. However, they differ in **how** they achieve that flexibility.

#### Responsive Design
Responsive design uses **fluid layouts** that **automatically adjust** based on the viewport size. It relies heavily on **relative units** (%, `em`, `rem`, `vw`, etc.) and **media queries**.

- Layout continuously scales and reflows as the screen size changes.
- Works on any viewport width — even in-between breakpoints.

Example:
```css
.container {
  width: 90%;
  max-width: 1200px;
}

@media (min-width: 768px) {
  .container {
    display: flex;
  }
}
```

> Key Idea: One flexible layout that dynamically responds to any screen size.

#### Adaptive Design
Adaptive design uses **fixed layouts** for specific screen sizes or device types. The page detects the device or viewport and **loads the most suitable layout**.

> Key Idea: Multiple static layouts — one is chosen depending on the screen size.

Example:
```css
@media (max-width: 480px) {
  .container { width: 320px; }
}

@media (min-width: 481px) and (max-width: 768px) {
  .container { width: 600px; }
}

@media (min-width: 769px) {
  .container { width: 960px; }
}
```

#### Comparison
| Feature     | Responsive Design                     | Adaptive Design                       |
| ----------- | ------------------------------------- | ------------------------------------- |
| Layout      | Fluid & flexible                      | Fixed per breakpoint                  |
| Breakpoints | Uses media queries for smooth scaling | Uses discrete predefined layouts      |
| Adjustments | Continuous resizing                   | Jumps between layouts                 |
| Maintenance | One layout to maintain                | Multiple layouts to manage            |
| Performance | May need optimization                 | Can be optimized for specific devices |
| Best For    | Modern, flexible UI                   | Device-targeted experiences           |


### Can you explain the difference between coding a website to be responsive versus using a mobile-first strategy?
Both aim to make websites look good on all devices, but the approach is different:

- **Responsive Design (traditional approach)**: You start with a desktop layout and then adjust it for smaller screens using media queries that shrink or rearrange elements.
```css
@media (max-width: 768px) {
  /* Adjust layout for tablets and phones */
}
```
- **Mobile-First Design**: You start with the mobile layout by default, then use min-width media queries to add styles as the screen gets larger.
```css
/* Default = mobile */
.container { flex-direction: column; }

@media (min-width: 768px) {
  /* Add desktop layout */
  .container { flex-direction: row; }
}
```
A mobile-first strategy has the following main advantages:
- It's more performant on mobile devices, since all the rules applied for them don't have to be validated against any media queries.
- Mobile-first designs are more likely to be usable on larger devices (will just appear more stretched, but still usable). However, the reverse is not the case.


### What’s the Difference Between `margin` and `padding`?
Both `margin` and `padding` create **space around elements**, but they do it in **different places**. 
- `margin` creates **space outside** the element’s border.  
- `padding` creates **space inside** the element’s border — between the content and the border.

#### Margin
- Affects distance between elements.
- Can collapse vertically (e.g., two adjacent block elements).

#### Padding
- Affects content spacing inside the box.
- Increases the total element size (unless using `box-sizing: border-box`).
- Fills with the element’s background color.

### What is Margin Collapse?
**Margin collapse** is a CSS behavior where **vertical margins** of adjacent block-level elements **combine (collapse)** into a single margin instead of adding together.

When two (or more) vertical margins meet — for example, the bottom margin of one element and the top margin of another — **only the larger margin value is used**.

#### When Margins Collapse
Margins collapse only in vertical direction (top and bottom), and only under specific conditions.

1. Between Adjacent Block Elements
2. Between Parent and First/Last Child
3. Empty Elements - If an element has no content, padding, or border, its top and bottom margins collapse into one.

### What are the risks of using negative values for margins?
Margins can take **negative values**, which **shrink the space** between elements instead of increasing it.

When Negative Margins Can Be Useful:
- Adjusting alignment when no other option works
- Overlapping design elements intentionally (e.g., badges, banners)
- Fixing spacing in legacy layouts
- Creating “pull quotes” or decorative overlaps

Risks and Drawbacks:
- Overlapping content: Elements can cover each other, breaking visual flow or hiding important content.
- Unexpected scrollbars: Using large negative margins can move content outside the visible area, causing horizontal scrolling.
- Accessibility and readability issues: Overlapping text or elements may become unreadable or interfere with keyboard focus and hover states.

### Width and Height
The **`width`** and **`height`** properties define the **size of an element’s content box** — that is, how wide and tall the content area should be.  

By default, width and height apply only to the content area. Padding and borders add extra space unless you use box-sizing: border-box.

When `width` or `height` is set to `auto`:
- Block elements expand to fill their parent’s width.
- Inline elements shrink to fit their content.
- Height expands naturally to fit content (unless explicitly limited).

> height: 100% often doesn’t work as expected because it’s relative to the parent’s height — and if the parent doesn’t have a defined height, the browser has nothing to base that 100% on.

#### `min-content`, `max-content`, and `fit-content` in CSS

1. `min-content` - The element becomes as **small as possible** without overflowing its content — it “shrinks-to-fit”.
2. `max-content` - The element expands to **fit its longest line of content** — no wrapping occurs.
3. `fit-content()` acts like a clamp between min-content and max-content, but with a maximum limit.

### Hidden Content in HTML and CSS
“Hidden content” refers to **elements that exist in the HTML**, but are **not visible** or **accessible** to users on the screen. 

- `display: none` - Completely removes the element from the page layout and accessibility tree. Use for: temporarily removing elements (e.g., modal closed).
- `visibility: hidden` - Makes the element invisible, but it still occupies space in the layout. Use for: toggling visibility without layout shift.
- `opacity: 0` - The element is fully transparent, but still clickable and focusable.
- `hidden` Attribute - HTML’s built-in way to hide elements: Good for semantic hiding controlled by JavaScript (element.hidden = false).

### Inheritance in CSS
**Inheritance** in CSS means that some property values are **passed down** automatically from a **parent element** to its **child elements**.  

When an element doesn’t have a specific value for a property, the browser checks:
1. If the property is **inheritable**, it takes the value from its parent.  
2. If not, it falls back to the **initial (default)** value defined by the CSS specification.

Common Inherited Properties
| Category         | Inherited Properties                                                                                                                                                  |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Text & Font**  | `color`, `font`, `font-family`, `font-size`, `font-weight`, `line-height`, `letter-spacing`, `word-spacing`, `text-align`, `text-indent`, `visibility`, `white-space` |
| **List**         | `list-style`, `list-style-type`, `list-style-position`                                                                                                                |
| **Table**        | `border-collapse`, `caption-side`                                                                                                                                     |
| **Other visual** | `cursor`, `quotes`, `direction`                                                                                                                                       |

### Pseudo-classes and Pseudo-elements in CSS

#### Pseudo-classes
A **pseudo-class** represents a **special state** of an element. It’s written with a single colon `:` followed by the pseudo-class name.

Common Pseudo-classes:
| Pseudo-class     | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `:hover`         | When the user hovers over an element                      |
| `:active`        | While an element (like a link or button) is being clicked |
| `:focus`         | When an element (like an input) is focused                |
| `:visited`       | A link that has been visited                              |
| `:disabled`      | Disabled form elements                                    |
| `:checked`       | Checked checkbox or radio input                           |
| `:focus-visible` | Focused via keyboard (for accessibility)                  |

Structural (Position-based)
| Pseudo-class      | Description                                 |
| ----------------- | ------------------------------------------- |
| `:first-child`    | First child of its parent                   |
| `:last-child`     | Last child of its parent                    |
| `:nth-child(n)`   | Selects the nth child (e.g., `2n` for even) |
| `:nth-of-type(n)` | Nth child of a specific type                |
| `:not(selector)`  | Excludes specific elements                  |
| `:empty`          | Elements with no children or text           |

#### Pseudo-elements
A **pseudo-element** allows you to style a specific part of an element’s content, such as the first letter, first line, or content inserted before/after an element. They are written with two colons `(::)` to distinguish them from pseudo-classes.

Common Pseudo-elements
| Pseudo-element   | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `::before`       | Inserts generated content **before** an element’s content |
| `::after`        | Inserts generated content **after** an element’s content  |
| `::first-line`   | Styles only the **first line** of text                    |
| `::first-letter` | Styles only the **first letter** of text                  |
| `::selection`    | Styles the text selected by the user                      |
| `::marker`       | Styles list bullets or numbers                            |
| `::placeholder`  | Styles placeholder text in input fields                   |

### What Are the Options for Setting Color in CSS?
| Format             | Example                           | Supports Alpha? | Readability         | Use Case                |
| ------------------ | --------------------------------- | --------------- | ------------------- | ----------------------- |
| **Named**          | `red`                             | ❌               | 👍 Simple           | Quick styling           |
| **HEX**            | `#ff0000`                         | ✅ (8-digit)     | 👌 Common           | Precise colors          |
| **RGB(A)**         | `rgba(255,0,0,0.5)`               | ✅               | ⚙️ Technical        | Dynamic colors, JS      |
| **HSL(A)**         | `hsla(0,100%,50%,0.5)`            | ✅               | 🌈 Intuitive        | Theming, design systems |
| **currentColor**   | `border: 2px solid currentColor;` | —               | 🔁 Reuse text color | Consistent styling      |
| **Modern color()** | `color(display-p3 1 0 0)`         | ✅               | 🌐 Advanced         | Wide gamut / HDR        |


### CSS Logical Properties
**CSS Logical Properties** are an alternative to physical properties like `margin-left` or `padding-top`. They define **layout and spacing based on writing direction** (left-to-right, right-to-left, top-to-bottom, etc.), making your styles **direction- and language-aware**.

Сommon Logical Property Pairs
- `width` / `height` - `inline-size` / `block-size`
- `margin-top` / `margin-bottom` - `margin-block-start` / `margin-block-end`
- `padding-left` / `padding-right` - `padding-inline-start` / `padding-inline-end`
- `border-top` / `border-bottom` - `border-block-start` / `border-block-end`

### Border vs. Outline
While a border is part of the element’s **box model**, an outline is drawn **outside** the box — mainly used for **focus indication and accessibility**.

- The **border** is drawn **between the element’s padding and margin**, forming part of the element’s total size.
- The **outline** is a line drawn around the element’s border — it does not affect the layout or box model.
  - outline applies to all sides only
  - Ignores `border-radius`
  - use for accessibility and focus indication

### What's the difference between "resetting" and "normalizing" CSS?
- A **CSS reset** removes all default browser styles, so you start from a blank slate. It “resets” margins, paddings, fonts, and other defaults for all elements.
- **Normalize.css** doesn’t remove styles — it makes browser defaults consistent. It fixes inconsistencies (like line heights, form controls, heading sizes) while keeping sensible defaults intact.

### Is there any reason you'd want to use `translate()` instead of `absolute` positioning, or vice-versa? And why?

1. `position: absolute;`
Absolute positioning **removes an element from the normal document flow** and positions it relative to the nearest **positioned ancestor** (`relative`, `absolute`, `fixed`, or `sticky`).

2. `transform: translate()`
The translate() function moves an element visually relative to its current position, without changing its place in the DOM flow or affecting other elements.
- The element’s position visually shifts, but it still occupies its original space in the layout.
- Great for animations and micro-interactions.

`translate()`:
- Handled by the GPU, not the CPU.
- Does not trigger reflow or repaint — only composite.
- Ideal for animations (smooth 60fps transitions).

`position: absolute`:
- Triggers layout recalculations when moved dynamically.
- More expensive for animations or frequent updates.

### Floats in CSS — How They Work
The **`float`** property in CSS was originally designed to let text **wrap around images** — similar to how newspapers wrap text around photos.  

When you apply `float` to an element, it is **taken out of the normal document flow** (partially) and **pushed to one side** — left or right.  
Other inline content (like text) will **flow around it**.

```css
img {
  float: left;
  margin-right: 1rem;
}
```
The image “floats” to the left, and surrounding text wraps around it.

#### Clearing Floats
When a parent element contains only floated children, it may collapse because the floated items are removed from normal flow.

```html
<div class="container">
  <div class="box">Box 1</div>
  <div class="box">Box 2</div>
</div>

.box {
  float: left;
  width: 50%;
  height: 100px;
  background: lightcoral;
}

/* Parent has no height! */
```

Use the **clearfix** hack to make the parent wrap around floated children.

```css
.container::after {
  content: "";
  display: block;
  clear: both;
}
```
The **pseudo-element** clears the float and restores normal height for the parent.

The `clear` Property Prevents elements from sitting beside floated ones — forces them below.


### Styling SVG in CSS
**SVG (Scalable Vector Graphics)** can be styled with **CSS**, just like HTML elements.  
You can control colors, strokes, fills, filters, and even animations — depending on **how the SVG is included** in your page.

How you include the SVG determines how much control you have with CSS.

| Method | Example | CSS Styling Support |
|---------|----------|---------------------|
| **Inline SVG** | `<svg>...</svg>` directly in HTML | ✅ Full control |
| **`<img src="...">`** | `<img src="icon.svg" alt="">` | ⚠️ Limited (cannot style internal parts) |
| **`background-image`** | `background: url(icon.svg);` | ⚠️ Only filters, size, position |
| **`<object>` / `<iframe>`** | `<object data="icon.svg"></object>` | ⚠️ Partial, needs internal CSS or JS |

Common SVG Styling Properties
| Property                          | Description                                       | Example                                  |
| --------------------------------- | ------------------------------------------------- | ---------------------------------------- |
| `fill`                            | Fills the shape area                              | `fill: red;`                             |
| `stroke`                          | Border (outline) of shape                         | `stroke: black;`                         |
| `stroke-width`                    | Width of the border                               | `stroke-width: 3;`                       |
| `stroke-dasharray`                | Creates dashed or dotted lines                    | `stroke-dasharray: 5 3;`                 |
| `stroke-linecap`                  | End shape for strokes (`butt`, `round`, `square`) | `stroke-linecap: round;`                 |
| `opacity`                         | Transparency                                      | `opacity: 0.7;`                          |
| `fill-opacity` / `stroke-opacity` | Opacity for specific parts                        | `fill-opacity: 0.5;`                     |
| `filter`                          | Apply effects (blur, drop shadow, etc.)           | `filter: drop-shadow(2px 2px 2px gray);` |

When using an external file (via `<img>` or `background-image`), you can only style the container, not the SVG internals.

### What is the difference between images added via the HTML `<img>` tag and the CSS `background-image` property?
- `<img>` contributes to the meaning of a page and is read by assistive technologies.
- background-image is purely decorative and should not be used for essential content.

| Feature | `<img>` | `background-image` |
|----------|----------|--------------------|
| **Purpose** | Displays **content images** (part of the document meaning) | Displays **decorative images** (part of design, not content) |
| **Semantics** | Part of the HTML content structure | Purely visual styling via CSS |
| **Accessibility** | Accessible to screen readers via `alt` attribute | Not accessible — ignored by screen readers |
| **SEO impact** | Indexed by search engines | Not indexed as content |
| **Control** | Controlled by HTML attributes (`src`, `alt`, `width`, `height`) | Controlled entirely by CSS properties |

### Explain how a browser determines what elements match a CSS selector.
When a browser applies CSS, it must figure out **which elements** on the page match **each CSS selector**.  
This process is called **CSS selector matching**, and it happens during the **rendering** and **recalculation of styles** stages of the browser’s rendering pipeline.

The browser doesn’t check CSS selectors from **left to right** (as you read them).  
Instead, it matches them **right to left** — starting from the **rightmost part** of the selector (the **key selector**).

#### Example
```css
div.article p.highlight span {
  color: red;
}
```

- The browser starts from the rightmost selector → span.
- It finds all `<span>` elements in the document.
- For each `<span>`, it checks whether its parent matches p.highlight.
- If the parent matches, it then checks whether that `<p>` is inside a div.article.
- Only those `<span>` elements passing all tests get color: red.

### How would you approach fixing browser-specific styling issues?
- After identifying the issue and the offending browser, use a separate style sheet that only loads when that specific browser is being used. This technique requires server-side rendering though.
- Use libraries like Bootstrap that already handles these styling issues for you.
- Use autoprefixer to automatically add vendor prefixes to your code.
- Use Reset CSS or Normalize.css.
- If you're using PostCSS (or a similar CSS transpilation library), there may be plugins which allow you to opt in to using modern CSS syntax (and even W3C proposals) that will transform those sections of your code into equivalent backward-compatible code that will work in the targets you've used.


### What CSS methodologies do you know and use yourself?

#### BEM (Block, Element, Modifier)
**BEM** is one of the most popular CSS methodologies. It encourages a **modular, component-based** approach with clear naming conventions.

- **Block**: A standalone, independent entity that is meaningful on its own. Blocks can be reused across different parts of a website. Examples include header, menu, button, or card. 
- **Element**: A part of a block that has no standalone meaning and is semantically tied to its parent block. Elements are named using the block's name followed by a double underscore (`__`) and the element's name. For example, a menu block might have `menu__item` elements. 
- **Modifier**: A flag on a block or element used to change its appearance, state, or behavior. Modifiers are named using the block or element's name followed by a double hyphen (--) and the modifier's name. For instance, a button block could have a `button--primary` modifier for a primary style, or a `button--disabled` modifier for a disabled state. 

Example
```html
<div class="card">
  <h2 class="card__title">Card Title</h2>
  <p class="card__content">This is the content of the card.</p>
  <button class="card__button card__button--primary">Learn More</button>
</div>
```

#### SMACSS (Scalable and Modular Architecture for CSS)
**SMACSS** categorizes CSS rules into five distinct types: Base, Layout, Module, State, and Theme. This provides a clear structure for organizing stylesheets and promoting modularity.

1. Base – default element styles (html, body, a, etc.)
2. Layout – structure and positioning of main areas (.header, .sidebar)
3. Module – reusable parts (buttons, cards)
4. State – temporary states (.is-active, .hidden)
5. Theme – visual variations (colors, themes)

#### OOCSS (Object Oriented CSS)
**OOCSS** emphasizes the separation of structure and skin. It encourages creating reusable "objects" (like media objects or button styles) that can be applied independently of their content or context.
```css
.media {
  display: flex;
  align-items: center;
}

.media__img {
  margin-right: 1rem;
}

.media__body {
  flex: 1;
}
```

#### Atomic CSS
Atomic CSS focuses on single-purpose utility classes (like Tailwind CSS). Each class handles one job — no large custom stylesheets.
```html
<button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
  Save
</button>
```

#### ITCSS (Inverted Triangle CSS)
ITCSS organizes CSS files based on specificity, starting with low-specificity global styles and gradually moving to high-specificity component-specific styles. This helps manage the cascade and prevent specificity conflicts.

Layer hierarchy:
1. Settings – global variables
2. Tools – mixins, functions
3. Generic – resets, normalize
4. Elements – raw HTML elements
5. Objects – layout abstractions
6. Components – UI components
7. Utilities – helpers with high specificity

#### CSS-in-JS (Modern Component-Based Approach)
Used in frameworks like React, Vue, or Svelte — styles are scoped to components and written in JS.

### Explain the concept of CSS variables.
**CSS Variables** (also known as **custom properties**) let you **store values in variables** and **reuse them** throughout your styles.  

CSS variables always start with **two dashes `--`**, and you access them using the `var()` function.

```css
:root {
  --main-color: #007bff;
  --text-color: #333;
  --padding: 1rem;
}

button {
  background-color: var(--main-color);
  color: var(--text-color);
  padding: var(--padding);
}
```

:::info
`:root` targets the `<html>` element — variables defined here are global and available everywhere.
:::

Benefits of CSS Variables
- ♻️ Reusable values throughout your code
- 🎨 Easy theme switching (dark/light mode)
- 🧩 Responsive and adaptive control
- ⚙️ Can be updated dynamically with JS
- 🧱 Cleaner, more maintainable styles

### Which CSS properties affect performance?
Some trigger **expensive browser operations** — such as reflow (layout recalculation) or repaint (visual update) — while others are **GPU-accelerated and lightweight**.

When CSS changes, the browser may perform several steps:

1. **Recalculate Style** → determine which styles apply.  
2. **Layout (Reflow)** → compute sizes and positions of elements.  
3. **Paint (Repaint)** → fill pixels (colors, borders, text, shadows, etc.).  
4. **Composite** → combine layers (handled by GPU).

Some properties trigger *all* of these steps, while others skip directly to painting or compositing.
| Category | Examples |
|-----------|-----------|
| **Box size & position** | `width`, `height`, `top`, `left`, `bottom`, `right` |
| **Box model** | `margin`, `padding`, `border-width`, `border`, `border-radius` |
| **Display & float** | `display`, `float`, `clear` |
| **Typography metrics** | `font-size`, `font-family`, `line-height`, `letter-spacing`, `word-spacing` |
| **Content change** | Changing text or adding/removing DOM elements |

### What are the advantages and disadvantages of CSS animations compared to JS animations?
| Aspect                   | CSS Animations                      | JavaScript Animations                 |
| ------------------------ | ----------------------------------- | ------------------------------------- |
| **Performance**          | 🟢 Very efficient (GPU-accelerated) | 🟠 Depends on implementation          |
| **Ease of use**          | 🟢 Simple for basic transitions     | 🔴 Requires more setup                |
| **Control**              | 🔴 Limited runtime control          | 🟢 Full programmatic control          |
| **Complex sequences**    | 🔴 Difficult                        | 🟢 Easy to coordinate                 |
| **Dynamic data**         | 🔴 Not supported                    | 🟢 Fully supported                    |
| **Browser optimization** | 🟢 Automatically optimized          | ⚙️ Manual optimization needed         |
| **Best for**             | UI effects, simple animations       | Interactive or logic-based animations |

### What are pre-/post-processors used for?

#### Preprocessor
A **preprocessor** lets you write CSS using a **custom syntax with advanced features** (like variables, nesting, mixins, and logic), and then compiles it into standard CSS.

Common Preprocessors:
- **Sass/SCSS** (`.scss`, `.sass`)
- **LESS**
- **Stylus**

Example (SCSS):
```scss
$primary-color: #3498db;

button {
  background: $primary-color;
  color: white;

  &:hover {
    background: darken($primary-color, 10%);
  }
}
```

#### Postprocessors
A postprocessor runs after the CSS is written or compiled, optimizing or modifying it before delivery to the browser.

| Task              | Description                                                                 |
| ----------------- | --------------------------------------------------------------------------- |
| **Autoprefixing** | Automatically add `-webkit-`, `-moz-`, `-ms-` prefixes.                     |
| **Minification**  | Compress CSS to reduce file size.                                           |
| **Linting**       | Catch errors or enforce a consistent style guide.                           |
| **Optimization**  | Remove unused CSS, merge selectors, optimize media queries.                 |
| **Future CSS**    | Use modern CSS features before browsers fully support them (via polyfills). |


Common Postprocessors:
- PostCSS — most popular and extensible (acts like a plugin system)
- Autoprefixer — adds vendor prefixes automatically
- CSSNano / CleanCSS — minifies and optimizes CSS for production

### Difference Between Transition and Animation
**Transitions** allow property changes in CSS to occur **smoothly over a specified duration**, triggered by a **state change** (like `:hover`, `:focus`, or adding/removing a class).

They are **event-driven** — the browser interpolates between the start and end values automatically.

Use transitions when:
- You need simple state changes (hover, focus, active, etc.).
- Only start and end states are required.
- The animation depends on user interaction or DOM updates.

**Animations** use the `@keyframes` rule to define multiple steps or states of change over time. They can start automatically and loop indefinitely, without any user interaction.
Example:
```css
@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.icon {
  animation: pulse 1.5s infinite ease-in-out;
}
```

Use animations when:
- You need complex or looping effects (loading spinners, attention grabbers, etc.).
- There are multiple intermediate states.
- You want it to run automatically or repeat.

### You need to add custom styles for the scrollbar. How would you do it?

#### 🟦 Using `::-webkit-scrollbar` (WebKit Browsers)
Most browsers (Chrome, Edge, Safari) support custom scrollbar styling through **WebKit-specific pseudo-elements**.

**Example:**
```css
/* Scrollbar container */
::-webkit-scrollbar {
  width: 10px; /* vertical scrollbar width */
  height: 10px; /* horizontal scrollbar height */
}

/* Track (the background area) */
::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

/* Thumb (the draggable handle) */
::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 10px;
}

/* Thumb on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555;
}
```
- ::-webkit-scrollbar — targets the scrollbar itself.
- ::-webkit-scrollbar-track — styles the background area.
- ::-webkit-scrollbar-thumb — styles the draggable bar.

#### Using `scrollbar-color` and `scrollbar-width` (Firefox)
Firefox uses standardized properties to control scrollbar appearance.
```css
/* Apply to any scrollable element */
.scrollable {
  scrollbar-color: #888 #f1f1f1; /* thumb color, track color */
  scrollbar-width: thin; /* auto | thin | none */
}
```
### What methods of style isolation do you know?
**Style isolation** is the practice of preventing CSS from **leaking** or **conflicting** between components or parts of an application.  

#### 🟦 1. BEM (Block, Element, Modifier)
A **naming convention** for CSS classes that provides natural isolation through unique, structured class names.

#### 2. CSS Modules
Each file automatically scopes its class names locally by hashing them, preventing conflicts between components.

#### 3. Shadow DOM (Web Components)
The Shadow DOM encapsulates both markup and styles inside a component’s shadow root, completely isolating them from the global CSS.

#### 4. CSS-in-JS (Styled Components, Emotion)
Styles are written directly in JavaScript and scoped to components automatically via generated class names.

### How does the CSS clamp() function work? Give some useful examples of its use.
The **`clamp()`** function allows you to define a value that **adapts dynamically** within a **specified range**, combining responsiveness with constraints.  

It takes **three parameters**: a **minimum**, a **preferred (ideal)**, and a **maximum** value.

It ensures the value:
- never drops below the minimum,
- never exceeds the maximum,
- and scales dynamically within the range.

#### 🧩 Syntax
```css
property: clamp(min, preferred, max);
```
- `min` — the smallest allowed value
- `preferred` — the ideal or dynamic value (often uses relative units like vw)
- `max` — the largest allowed value

Example: Responsive Container Width
```css
.container {
  width: clamp(300px, 80%, 1200px);
}
```
- At least 300px wide
- Ideally 80% of the viewport
- But no more than 1200px
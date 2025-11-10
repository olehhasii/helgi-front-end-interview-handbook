---
sidebar_position: 1
---

# HTTP, APIs and Networks

## Networks

### What is WWW? How Web Works?
The World Wide Web, commonly known as the Web, is a system of interlinked hypertext documents and resources. Accessed via the internet, it utilizes browsers to render web pages, allowing users to view, navigate, and interact with a wealth of information and multimedia.

#### 🌐 The Web vs. the Internet

| Term | Description |
|:-----------|:-------------|
| **Internet** | The physical network of computers and servers connected globally. It’s the infrastructure. |
| **World Wide Web** | A system of **interlinked documents** (websites, apps) that run *on top* of the Internet, using the **HTTP/HTTPS protocol**. |

> 💡 The Web is *one of many services* that run on the Internet — others include email (SMTP), file transfer (FTP), and messaging (IRC).

#### How the Web Works (Simplified)
When you visit a website:

When you type a web address (which is technically part of a URL) into your browser address bar, the following steps occur:

1. You enter a **URL** (e.g., `https://www.example.com`)
2. The browser goes to the DNS server and finds the real address of the server that the website lives on.
3. The browser sends an HTTP request message to the server, asking it to send a copy of the website to the client. This message, and all other data sent between the client and the server, is sent across your internet connection using TCP/IP.
4. If the server approves the client's request, the server sends the client a "200 OK" message, which means "Of course you can look at that website! Here it is", and then starts sending the website's files to the browser as a series of small chunks called packets.
5. The browser assembles the small chunks into a complete web page and displays it to you.

### What is DNS, TCP/IP?

#### What is DNS (Domain Name System)?
**DNS** is the Internet’s **address book** — it translates **human-readable domain names** (like `example.com`) into **IP addresses** (like `93.184.216.34`) that computers use to locate servers. This system uses special servers that match up a web address you type into your browser (like "mozilla.org") to the website's real (IP) address.
> 💡 Without DNS, you would have to type numeric IP addresses into your browser instead of names.

#### What is TCP/IP?
TCP/IP is the foundation of Internet communication — a suite of network protocols that define how data is packaged, addressed, transmitted, and received.

TCP (Transmission Control Protocol):
- Ensures reliable, ordered, error-free delivery of data
- Establishes a connection before transferring (handshake)
- Splits data into packets, sends them, and reassembles them on arrival

IP (Internet Protocol):
- Handles addressing and routing of packets across networks
- Each device on the Internet has a unique IP address
- Works with IPv4 or IPv6 formats

### What is HTTP?
**HTTP** is a protocol for fetching resources such as HTML documents. It is the foundation of any data exchange on the Web and it is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser. 

HTTP is generally designed to be human-readable, even with the added complexity introduced in HTTP/2 by encapsulating HTTP messages into frames. HTTP messages can be read and understood by humans, providing easier testing for developers, and reduced complexity for newcomers.
```text
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Chrome/124.0
Accept: text/html
``` 

### HTTP VS HTTPS
HTTPS = HTTP over SSL/TLS encryption. It secures the connection so data can’t be read or modified in transit.

### What are HTTP methods (GET, POST, PUT, PATCH, DELETE)?
| Method      | Description                    | Typical Use                    |
| :---------- | :----------------------------- | :----------------------------- |
| **GET**     | Retrieve data from a resource  | Fetch web pages, API data      |
| **POST**    | Send data to create a resource | Submit forms, send JSON        |
| **PUT**     | Replace an existing resource   | Update a record fully          |
| **PATCH**   | Partially update a resource    | Update specific fields         |
| **DELETE**  | Remove a resource              | Delete a user or item          |
| **HEAD**    | Like GET but without a body    | Check headers or availability  |
| **OPTIONS** | Returns supported methods      | Used for CORS preflight checks |


### What are HTTP status codes?
| Code    | Meaning       | Description                                 | 
| :------ | :------------ | :------------------------------------------ |
| **1xx** | Informational | Request received, continuing                |
| **2xx** | Success       | Action successfully completed               |
| **3xx** | Redirection   | Resource moved or cached                    |
| **4xx** | Client Error  | Request error (e.g., bad syntax, not found) |
| **5xx** | Server Error  | Problem on the server side                  |

| Code                        | Meaning                   |
| :-------------------------- | :------------------------ |
| `200 OK`                    | Successful response       |
| `201 Created`               | Resource created          |
| `301 Moved Permanently`     | Redirected                |
| `304 Not Modified`          | Cached response           |
| `400 Bad Request`           | Invalid client request    |
| `401 Unauthorized`          | Authentication required   |
| `403 Forbidden`             | Access denied             |
| `404 Not Found`             | Resource missing          |
| `500 Internal Server Error` | Server-side failure       |
| `503 Service Unavailable`   | Server overloaded or down |


### Client-Server Architecture

**Client–Server Architecture** is a **model of communication** in which two main entities — a **client** (e.g., browser, app) and a **server** (e.g., web host, API backend) — interact over a network to exchange data or perform tasks.

The **client** sends requests for resources or actions, and the **server** processes those requests and sends back responses.

1. The **client** (like your web browser) initiates a **request** — for example, loading a web page.  
2. The **server** (like Apache, Nginx, or Node.js) receives and processes that request.  
3. The server sends a **response** — often HTML, JSON, or other data.  
4. The client interprets or renders the response for the user.

#### Common Communication Protocols
| Protocol           | Purpose                                |
| :----------------- | :------------------------------------- |
| **HTTP / HTTPS**   | Web requests and responses             |
| **WebSocket**      | Real-time, bidirectional communication |
| **FTP / SFTP**     | File transfers                         |
| **SMTP / IMAP**    | Email transmission                     |
| **gRPC / GraphQL** | API communication protocols            |

### What are response headers?

**Response headers** are pieces of **metadata** that the **server sends back to the client** along with an HTTP response.  

They provide **information about the response**, such as:
- Content type and length  
- Caching rules  
- Cookies or authentication details  
- Server type and security policies  

Response headers help browsers or clients understand **how to handle the response body**.

Each response header is a key–value pair, separated by a colon
```text
Header-Name: Header-Value
```

#### Common Response Headers
| Header               | Purpose / Description                     | Example                         |
| :------------------- | :---------------------------------------- | :------------------------------ |
| **Content-Type**     | Describes the media type of the response  | `text/html`, `application/json` |
| **Content-Length**   | Size of the response body in bytes        | `2048`                          |
| **Date**             | Timestamp when the response was generated | `Thu, 23 Oct 2025 13:10:00 GMT` |
| **Server**           | Identifies the server software            | `nginx`, `Apache`               |
| **Connection**       | Controls whether connection stays open    | `keep-alive`, `close`           |
| **Content-Encoding** | Compression type used                     | `gzip`, `br`                    |
| **Cache-Control**    | Defines caching behavior                  | `no-cache, `, `max-age=3600`    |
| **Access-Control-Allow-Origin** | Compression type used          | `gzip`, `br`                    |
| **Set-Cookie**       | (CORS) Specifies allowed origins          | `Access-Control-Allow-Origin: *`|

### URLs: absolute, relative, url parts
A **URL (Uniform Resource Locator)** is the **address** used to locate a resource on the web. It specifies **where** the resource is and **how** to access it (via a protocol like HTTP or HTTPS).

#### Parts of a URL

| Part | Example | Description |
|:-----------|:-------------|:-------------|
| **Scheme (Protocol)** | `https` | Defines how the resource is accessed (e.g., `http`, `https`, `ftp`, `file`) |
| **Host (Domain)** | `example.com` | The domain name or IP address of the server |
| **Port (Optional)** | `:8080` | Specifies the network port (default: `80` for HTTP, `443` for HTTPS) |
| **Path** | `/users/profile` | The route to the specific resource on the server |
| **Query String** | `?id=42` | Key–value parameters sent with the request |
| **Fragment (Hash)** | `#details` | Points to a specific section within the page |

#### Absolute URL
Contains **all parts of the address**, including the protocol and domain. Always points to the same resource, no matter where it’s used.
```html
<a href="https://example.com/images/logo.png">Logo</a>
```

#### Relative URL
Specifies a path relative to the current page or directory. Depends on the current page’s location.
```html
<a href="/images/logo.png">Logo</a>      <!-- Root-relative -->
<a href="../about.html">About</a>        <!-- Relative to parent directory -->
```

:::tip
You can encode/decode with JS:
```javascript
encodeURIComponent("hello world"); // "hello%20world"
decodeURIComponent("hello%20world"); // "hello world"
```
:::

### What are cross-origin requests? (CORS)
**CORS (Cross-Origin Resource Sharing)** is a **browser security mechanism** that controls how web pages can **request resources from a different origin**. It prevents malicious websites from making unauthorized requests to another site’s APIs or user data.

An **origin** is defined by the combination of:
- **Protocol** (`http` / `https`)
- **Domain (Host)**
- **Port**

Two URLs share the same origin **only if all three are identical**.

#### The Same-Origin Policy (SOP)
Browsers enforce a **Same-Origin Policy** — a web page can only make requests to the **same origin** it was loaded from. Without SOP, any website could secretly access your private data on another domain (e.g., your banking session cookies).

#### What Is CORS?
CORS is a controlled way to relax the Same-Origin Policy. It allows a server to specify which other origins are permitted to access its resources. The server does this using special HTTP headers in its response.

#### How CORS Works (Step by Step)

**Simple Request**
For basic GET or POST requests (no custom headers, plain content types):
1. Browser sends the request to another origin.
2. Server responds with: `Access-Control-Allow-Origin: https://myapp.com`
3. Browser checks the header — if allowed, it delivers the response to JS. Otherwise, it blocks it.

**Preflight Request (for Non-Simple Requests)**
For requests that:
- Use methods like PUT, DELETE, PATCH
- Include custom headers (e.g., Authorization)
- Use non-standard content types (application/json)

The browser first sends an OPTIONS request — called a preflight.

```text
1️⃣ Browser → OPTIONS /api/data
   Origin: https://myapp.com
   Access-Control-Request-Method: POST
   Access-Control-Request-Headers: Authorization

2️⃣ Server → 200 OK
   Access-Control-Allow-Origin: https://myapp.com
   Access-Control-Allow-Methods: GET, POST, DELETE
   Access-Control-Allow-Headers: Authorization
```

If allowed, the browser then sends the actual request.

### What are WebSockets?
WebSockets are a protocol that allows persistent, full-duplex communication between the client and server over a single TCP connection. It allows the **client** (browser) and **server** to **send data to each other at any time**, without needing to repeatedly open new HTTP connections.

1. Client opens a WebSocket connection: new WebSocket("ws://yourserver.com")
2. A handshake happens over HTTP to upgrade to WebSocket protocol
3. After that, both client and server can send data any time
4. Connection stays open until either side closes it


### What are Server-Sent Events (SSE)?
**Server-Sent Events (SSE)** provide a **one-way, real-time communication channel** where the **server can push updates to the client** over a single **HTTP connection**.

Unlike **WebSockets** (which are **bidirectional**), SSE is **unidirectional** — only the **server** can send messages to the **client** once the connection is established.

1. The client opens a connection to the server using the special **`text/event-stream`** MIME type.
2. The server keeps the connection **open indefinitely**.
3. Whenever new data is available, the server **pushes messages** to the client.
4. The browser automatically **reconnects** if the connection drops.

In JavaScript, you use the `EventSource` API to connect:
```javascript
const source = new EventSource("https://example.com/events");

source.onmessage = (event) => {
  console.log("New message:", event.data);
};

source.onerror = (err) => {
  console.error("Connection lost:", err);
};

source.onopen = () => {
  console.log("Connected to server");
};
```

### What is Webhook
A **Webhook** is a **way for one application to send real-time data or event notifications** to another application over the web. 

It works by the **source application (sender)** making an **HTTP POST request** to a specific **URL** (the webhook endpoint) whenever a certain event occurs.
> In short:  
> **Webhooks push data to you**, instead of requiring you to poll for updates.

1. **You register a URL** (an endpoint) with an external service (e.g., Stripe, GitHub).
2. **An event occurs** in that service (e.g., new payment, new commit, form submission).
3. The service sends an **HTTP POST request** to your URL with event data (usually JSON).
4. Your server receives the request and processes it (e.g., updates database, sends notification).

### What is JSON?
**JSON (JavaScript Object Notation)** is a **lightweight, text-based data format** used to **store and exchange structured data** between systems, especially between a client and a server. It’s easy for humans to read and write, and easy for machines to parse and generate.

JSON is derived from **JavaScript object syntax**, but it’s language-independent — almost every programming language supports it.

Example JSON object:
```json
{
  "name": "Oleh",
  "age": 25,
  "isStudent": true,
  "skills": ["JavaScript", "React", "Node.js"],
  "address": {
    "city": "Kyiv",
    "country": "Ukraine"
  }
}
```
#### JSON Syntax Rules
| Rule                                     | Description                                     |
| :--------------------------------------- | :---------------------------------------------- |
| **Data is in name/value pairs**          | `"key": "value"`                                |
| **Data is separated by commas**          | Between multiple pairs                          |
| **Curly braces `{}`**                    | Represent objects                               |
| **Square brackets `[]`**                 | Represent arrays                                |
| **Keys must be strings (double quotes)** | `'key'` ❌ — must be `"key"` ✅                   |
| **Values can be**                        | string, number, boolean, null, array, or object |
| **No comments allowed**                  | Unlike JS objects                               |

#### Converting Between JSON and JavaScript
| Method                    | Description                       |
| :------------------------ | :-------------------------------- |
| **`JSON.stringify(obj)`** | Converts object to JSON string    |
| **`JSON.parse(str)`**     | Parses JSON string into JS object |


### JSON vs XML
| Feature             | JSON                                                               | XML                                                 |
| :------------------ | :----------------------------------------------------------------- | :-------------------------------------------------- |
| **Format Type**     | Data format                                                        | Markup language                                     |
| **Structure**       | Key–value pairs and arrays                                         | Hierarchical tree of elements                       |
| **Tags / Brackets** | `{}`, `[]`                                                         | `<tag> ... </tag>`                                  |
| **Comments**        | ❌ Not supported                                                    | ✅ Supported (`<!-- comment -->`)                    |
| **Data Types**      | Supports primitives (string, number, boolean, null, array, object) | Everything is text (no native data types)           |
| **Attributes**      | ❌ No attributes (only key–value pairs)                             | ✅ Supports attributes (`<user id="1">`)             |
| **Namespaces**      | ❌ No                                                               | ✅ Supported                                         |
| **Readability**     | Very concise                                                       | Verbose and nested                                  |
| **Parsing**         | Easy (`JSON.parse()`)                                              | Requires XML parser (`DOMParser`, `XMLHttpRequest`) |

```text
//JSON
{
  "users": [
    { "id": 1, "name": "Alice" },
    { "id": 2, "name": "Bob" }
  ]
}

//XML
<users>
  <user>
    <id>1</id>
    <name>Alice</name>
  </user>
  <user>
    <id>2</id>
    <name>Bob</name>
  </user>
</users>
```


### What is SSL/TSL?
**SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** are **cryptographic protocols** that secure communication between a **client** (like a web browser) and a **server** over the Internet.
> 💡 TLS is the modern, more secure version of SSL.  
> Today, when people say "SSL", they almost always mean **TLS**.

Without encryption, data sent over HTTP (like passwords, cookies, or credit cards) travels in **plain text**. Anyone on the same network (e.g., Wi-Fi, ISP, or attacker) could intercept and read it.

SSL/TLS protects against this by:
- Encrypting the connection  
- Verifying the server’s identity  
- Preventing tampering or eavesdropping  

A **Digital Certificate** proves the website’s identity. It’s issued by a Certificate Authority (CA) like Let’s Encrypt, DigiCert, or Sectigo.

## APIs

### What is API?
**API (Application Programming Interface)** is a set of **rules and definitions** that allows one piece of software to **communicate with another**.

In web development, an **API** typically refers to a **Web API** — an interface that allows a **client** (like a browser or mobile app) to **interact with a server** using standard web protocols such as **HTTP**.

> 💡 In simple terms:  
> An API defines **how different systems talk to each other** — what requests are allowed, what data is expected, and what responses will be returned.

### What is Rest API

A **REST API (Representational State Transfer API)** is a **style of web API** that follows a set of **architectural principles** for building **scalable, stateless, and resource-based** web services.

REST is not a protocol — it’s a **design pattern** that uses **HTTP** methods and **URIs** to perform operations on **resources** (data objects).

#### REST Core Concept
At the heart of REST are **resources**, identified by **unique URLs** (URIs). Clients interact with these resources using **standard HTTP methods**.

| Constraint                       | Description                                                              |
| :------------------------------- | :----------------------------------------------------------------------- |
| **1. Client–Server Separation**  | UI (client) and data (server) are independent — improves scalability     |
| **2. Statelessness**             | Each request is independent; server does not store session state         |
| **3. Cacheability**              | Responses should define whether they can be cached (e.g., using headers) |
| **4. Uniform Interface**         | Standardized interactions (URIs + HTTP methods + media types)            |
| **5. Layered System**            | APIs can be composed of multiple layers (proxy, load balancer, etc.)     |
| **6. Code on Demand (Optional)** | Server can send executable code (e.g., JS) to the client                 |


### What are the formats for data exchange between the front end and back end?
| Format | Description | MIME Type | Common Use Case |
|:-----------|:-------------|:-------------|:-------------|
| **JSON (JavaScript Object Notation)** | Lightweight text format using key–value pairs | `application/json` | REST APIs, web and mobile apps |
| **XML (eXtensible Markup Language)** | Markup-based hierarchical format | `application/xml` | Legacy APIs, SOAP services |
| **Form Data (URL-encoded)** | Encoded as key=value pairs | `application/x-www-form-urlencoded` | HTML forms, simple POST requests |
| **Multipart Form Data** | Used for sending files or mixed content | `multipart/form-data` | File uploads, images |
| **CSV (Comma-Separated Values)** | Tabular text data format | `text/csv` | Exporting/importing data, spreadsheets |
| **YAML (YAML Ain’t Markup Language)** | Human-readable structured data | `application/x-yaml` | Config files, API definitions (OpenAPI) |
| **Binary / Protobuf** | Compact binary serialization format | `application/octet-stream` | High-performance or low-latency systems |
| **GraphQL Query/Response** | Query language over HTTP (usually JSON-encoded) | `application/json` | Modern APIs (GraphQL endpoints) |

### What is GraphQL?

**GraphQL** is a **query language** and **runtime** for APIs.

It allows clients to **request exactly the data they need** — no more, no less — by querying a single endpoint instead of multiple REST routes. All GraphQL operations use HTTP POST (most commonly).

In traditional REST APIs:
- Each endpoint returns a fixed structure.
- You often **overfetch** (get extra data) or **underfetch** (need multiple requests).

**GraphQL solves this** by allowing clients to define their **own response shape** through queries.

Example GraphQL Schema
```graphql
type User {
  id: ID!
  name: String!
  email: String!
}

type Query {
  user(id: ID!): User
  users: [User!]!
}

type Mutation {
  createUser(name: String!, email: String!): User
}
```
Example Query, Mutation, and Response
```graphql
{
  user(id: 1) {
    name
    email
  }
}

mutation {
  createUser(name: "Anna", email: "anna@example.com") {
    id
    name
  }
}
```
### What is AJAX?


## Browser, Browser rendering

### Browser rendering step by step (HTML parsing, etc.)
When you load a webpage, the browser performs a series of **well-defined steps** to transform raw HTML, CSS, and JavaScript into a fully rendered page on your screen.  

#### 🧩 1. Loading Resources
- The browser sends an **HTTP(S) request** to the server.  
- The server responds with **HTML**, and the browser begins parsing it immediately.  
- As it encounters external resources (CSS, JS, images, fonts, etc.), it starts **fetching them in parallel**.

#### 🧱 2. Parsing HTML → Building the DOM
- The browser **parses the HTML** line by line.  
- Each tag (e.g., `<div>`, `<p>`, `<img>`) becomes a **DOM node** (Document Object Model).  
- The DOM represents the **structure and content** of the document.

#### 🎨 3. Parsing CSS → Building the CSSOM
- All linked and inline stylesheets are downloaded and parsed.
- The browser creates a CSS Object Model (CSSOM) — a tree representing all applied styles and their computed values.

#### 4. Combining DOM + CSSOM → Render Tree
- The Render Tree describes what will actually appear on the screen.
- It excludes elements like `<head>` or display: none.
- Each visible node has style information attached.

#### 5. Layout (Reflow)
- The browser calculates the size, position, and geometry of each element based on the Render Tree.
- This step determines where and how big each element should be.

#### 6. Painting
- The browser fills in pixels (colors, borders, shadows, text, etc.) for each element.
- The result is a set of paint instructions.

#### 7. Compositing

#### 🧾 Summary
| Step            | Description                             |
| --------------- | --------------------------------------- |
| 1. Loading      | Fetch HTML and linked resources         |
| 2. HTML Parsing | Build DOM tree                          |
| 3. CSS Parsing  | Build CSSOM tree                        |
| 4. Render Tree  | Combine DOM + CSSOM                     |
| 5. Layout       | Calculate positions and sizes           |
| 6. Paint        | Fill in visuals (colors, text, borders) |
| 7. Composite    | Merge layers into final frame           |


### How can you store data in browser?
| Storage Type | Capacity | Accessible From | Expiration | Data Type | Typical Use |
|:--------------|:----------|:----------------|:------------|:------------|:-------------|
| **Cookies** | ~4 KB per cookie | Client + Server | Manual or session | String | Sessions, auth tokens |
| **localStorage** | ~5–10 MB | Client only | Never (until cleared) | String | Preferences, themes |
| **sessionStorage** | ~5 MB | Client only (tab-scoped) | Until tab closed | String | Temporary data per tab |
| **IndexedDB** | Hundreds of MB | Client only | Persistent | Object (structured) | Offline apps, caching large data |
| **Cache API** | Large (depends on browser) | Client only | Persistent | Requests/Responses | Service workers, PWAs |

### What are cookies flags (secure, httpOnly, sameSite)?
| Flag | Purpose |
|------|----------|
| **Secure** | Send cookie only via HTTPS. |
| **HttpOnly** | Hide from JavaScript (protects from XSS). |
| **SameSite** | Control cross-site sending (`Strict`, `Lax`, `None`). |
| **Domain** | Set which domain can access it. |
| **Path** | Limit to a specific URL path. |
| **Expires / Max-Age** | Define cookie lifetime. |


### What is SPA
A **Single Page Application (SPA)** is a **web application that loads a single HTML page** and dynamically updates its content **without reloading the entire page** as the user interacts with it.

> 💡 In an SPA, the browser downloads the page once — then all navigation and data updates happen via **AJAX** or **fetch** calls to APIs.

1. The user visits `https://example.com`
2. The server sends back a single `index.html`
3. JavaScript framework (e.g., React, Angular, Vue) runs in the browser
4. The app loads data dynamically from APIs (usually JSON)
5. Page content updates **without a full reload**

SPAs use the HTML5 History API (`pushState`, `replaceState`) to manage navigation within the browser — keeping the URL updated without triggering a full server request.

#### Advantages
| Advantage                  | Description                                  |
| :------------------------- | :------------------------------------------- |
| **Fast user experience**   | No full page reloads                         |
| **Reduced server load**    | Data only (JSON), not full HTML              |
| **App-like behavior**      | Smooth transitions, instant UI updates       |
| **Easier API integration** | Communicates directly with back-end services |
| **Reusable components**    | Encouraged by modern JS frameworks           |

#### Disadvantages
| Disadvantage                             | Description                                               |
| :--------------------------------------- | :-------------------------------------------------------- |
| **SEO challenges**                       | Pages rendered by JS aren’t indexed easily (requires SSR) |
| **Slow initial load**                    | Large JS bundles must load before app is interactive      |
| **Complex routing and state management** | Requires client-side libraries                            |
| **Security**                             | More surface area for XSS or data leaks via APIs          |
| **Analytics & metadata**                 | Harder to manage since no page reloads                    |


### What is SSR
**Server-Side Rendering (SSR)** is a web rendering technique where the **HTML for a page is generated on the server**, then sent to the client **fully rendered**, ready to display immediately.

> 💡 In contrast to SPAs (which render pages in the browser using JavaScript),  
> SSR sends pre-rendered HTML — improving performance and SEO.

How SSR Works:
1. The user requests a page (e.g., `/about`)
2. The **server** runs the application code (React, Next.js, etc.)
3. The server renders the HTML on the fly and sends it to the browser
4. The **browser displays** the page instantly (no waiting for JS)
5. JavaScript bundle loads and **hydrates** the page to make it interactive — attaching event listeners and making it interactive

#### Advantages of SSR:
- Faster First Paint (SEO-friendly) – HTML is ready for search engines and users. 
- Fresh Data – Generated per request. Good for dynamic content (e.g., dashboards, user-specific data)
- Better core web vitals


#### Disadvantage of SSR:
- Can't use `window`, `localStorage`, or `DOM` in SSR context
- Slower interactivity: While the initial page load is fast, dynamic interactions can feel delayed because they may require another server request to update content.
- Slower time to interactive (TTI): The page may appear visually, but users will have to wait for the client-side JavaScript to download and execute before they can interact with interactive elements, like buttons or forms. 
- Increased server load: Rendering each page on the server for every request is computationally intensive, which can put a significant strain on server resources.
- Higher hosting costs

### What is SSG
**Static Site Generation (SSG)** is a web rendering technique where all pages of a website are **pre-rendered into static HTML files at build time**, instead of being rendered on the client (CSR) or dynamically on each request (SSR).

These pre-built pages are then served directly from a **CDN or static hosting** — making them extremely fast and scalable.

1. At build time, the framework fetches data (from APIs, CMS, or files).  
2. It generates **static HTML** for each route.
3. These files are deployed to a static hosting service (like Netlify, Vercel, or GitHub Pages).
4. When a user visits, the browser simply downloads the pre-built HTML.

#### Advantages of SSG
| Advantage               | Description                               |
| :---------------------- | :---------------------------------------- |
| ⚡ **Speed**             | Pre-rendered pages load instantly         |
| 🧭 **SEO-friendly**     | HTML ready for search crawlers            |
| 🏗️ **Low server cost** | No server logic needed per request        |
| 🌎 **Scalable**         | Can be served via CDN globally            |
| 💾 **Security**         | No runtime server = fewer vulnerabilities |

#### Disadvantages of SSG
| Disadvantage                     | Description                               |
| :------------------------------- | :---------------------------------------- |
| **Stale content**                | Requires rebuilds for updated data        |
| **Build time increases**         | Many pages = long build process           |
| **Not ideal for real-time apps** | Data is static between builds             |
| **Limited interactivity**        | Needs client-side JS for dynamic behavior |

A large number of HTML files: Individual HTML files need to be generated for every possible route that the user may access.

### What Is Incremental Static Generation (ISG) / Incremental Static Regeneration (ISR)?
**Incremental Static Generation (ISG)** — often called **Incremental Static Regeneration (ISR)** — is a modern rendering approach that allows **static pages to be updated automatically after deployment**, without needing to rebuild the entire site manually.

In traditional **SSG**, pages are generated once during build time and never change  
until you manually rebuild the site. This is fast, but problematic for **frequently updated data** (like blogs, e-commerce, or news).

**ISG fixes this** by allowing static pages to **rebuild automatically** in the background after a set time — keeping content up-to-date *without full redeployment*.

1. A page is **generated once** at build time.
2. When a user visits the page, they get the **cached static HTML**.
3. After a set **revalidation period** (e.g., 60 seconds),  
   the next request triggers the **server to rebuild** the page in the background.
4. Once the rebuild finishes, future visitors see the **updated page**.





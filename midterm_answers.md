# Web Technology Mid-Term Exam Solutions (Fall 2025)
**Student Name:** M. Abu Bakar  
**Registration/Roll No:** 232201079  
**Course:** Web Technology (BSCS - V, A&B)  
**Instructor:** Mr. Syed Hussain Ali Shah Gillani  
**Institution:** Institute of Space Technology, KICSIT Campus, Kahuta  

---

## Question #1: Define a web server and explain its primary function in delivering web content. And the working process of web servers. Discuss how web servers manage client connections, handle requests, and serve both static and dynamic content. Include examples where possible.

### Definition of a Web Server
A web server is a computer system (comprising both hardware and software) that stores web server software and website components (such as HTML documents, images, stylesheets, and JavaScript files). It listens for requests from web clients (such as browsers like Chrome, Firefox, or Safari) over the internet or intranet and delivers the requested web pages.
* **Examples:** Apache HTTP Server, Nginx, Microsoft Internet Information Services (IIS).

### Primary Function
The primary function of a web server is to process incoming HTTP/HTTPS requests from clients, locate the requested resources, and return the appropriate web content back to the client.

### Working Process and Client Connections
1. **Establishing Client Connections:**
   * When a user enters a URL or clicks a link, the client initiates a TCP/IP connection with the web server through a standard network procedure known as the **Three-Way Handshake** (`SYN`, `SYN-ACK`, `ACK`).
   * For secure websites, a cryptographic handshake (TLS/SSL) follows to establish an encrypted session.
2. **Handling Requests:**
   * Once connected, the client sends an HTTP request message (which includes a method like `GET`, `POST`, `PUT`, `DELETE`, headers, and sometimes a request body).
   * The web server parses this request to determine what resource or action the client is asking for.
3. **Serving Static vs. Dynamic Content:**
   * **Static Content:** If the requested file exists as-is on the server storage (e.g., `.html`, `.css`, `.jpg`, `.png`), the web server reads the file directly from disk and sends it back to the client inside an HTTP response.
     * *Example:* Fetching a static image file `logo.png`.
   * **Dynamic Content:** If the request requires real-time data processing, database queries, or user-specific generation, the web server cannot fulfill it alone. It passes the request to an application server or backend handler (running PHP, Node.js, Python, etc.), receives the dynamically generated output, and sends it back to the client.
     * *Example:* Logging into a user dashboard where data is fetched live from a database.

---

## Question #2: Discuss the differences between a web server and an application server, and explain how each contributes to the functioning of a web application.

### Key Differences

| Feature | Web Server | Application Server |
| :--- | :--- | :--- |
| **Primary Focus** | Serving static web pages and handling HTTP/HTTPS communication. | Executing business logic, application workflows, and database interactions. |
| **Protocol Support** | Primarily handles HTTP and HTTPS protocols. | Supports various protocols including HTTP, RMI/RPC, JDBC, and custom messaging protocols. |
| **Content Type** | HTML, CSS, images, client-side scripts (static files). | Dynamic content, calculated data, server-side code execution. |
| **Examples** | Apache HTTP Server, Nginx, Microsoft IIS. | Apache Tomcat, GlassFish, Node.js runtimes, JBoss. |

### Contribution to Web Applications
* **Web Server Contribution:** Acts as the frontline gatekeeper. It receives incoming browser traffic, manages network sockets, serves fast static assets, handles SSL termination, and routes requests to the appropriate backend service.
* **Application Server Contribution:** Acts as the backend brain. It performs heavy lifting, processes calculations, handles user sessions, queries databases, and applies core business rules before returning results to the web server.

---

## Question #3: Apply your understanding of web architecture to illustrate how DNS and TCP/IP protocols work together to load a web page.

When a user types a URL (e.g., `https://www.example.com`) into a browser and presses Enter, DNS and TCP/IP protocols coordinate in the background to load the page through the following sequence:

1. **DNS Resolution (Finding the Address):**
   * Humans use easy-to-remember domain names (`example.com`), but routers and servers communicate using numeric IP addresses (e.g., `192.0.2.45`).
   * The browser queries the **DNS (Domain Name System)**—checking local cache, OS cache, router cache, and DNS servers (ISP, Root, TLD, and Authoritative servers)—to translate the domain name into its corresponding IP address.

2. **IP Addressing & Routing:**
   * Once the IP address is retrieved, the **Internet Protocol (IP)** ensures that data packets are properly addressed with both the source and destination IP addresses so they can route through various routers across the global internet.

3. **TCP Connection Establishment (Three-Way Handshake):**
   * Before sending actual website data, **TCP (Transmission Control Protocol)** establishes a reliable connection with the target server:
     * **SYN:** Client sends a synchronization packet to the server.
     * **SYN-ACK:** Server acknowledges and responds with its own synchronization packet.
     * **ACK:** Client acknowledges the server's response. Connection is established.

4. **Data Transmission and Page Loading:**
   * The browser sends an HTTP `GET` request over the established TCP connection.
   * The web server breaks the HTML, CSS, and script responses into smaller packets. TCP numbers and orders these packets.
   * IP routes these packets across the network. If any packet is lost, TCP requests retransmission, ensuring zero data loss.
   * Upon arrival, TCP reassembles the packets into the complete web page, which the browser then renders.

---

## Question #4: Define HTML and explain its role as the foundation of web development also explain the difference between HTML tags, elements, and attributes. Use examples to demonstrate how they work together to form webpage content.

### Definition of HTML
**HTML (HyperText Markup Language)** is the standard markup language used to create and structure documents designed to be displayed in a web browser.

### Role as the Foundation
HTML provides the skeleton and structural layout of a webpage. Without HTML, a browser has no content, headings, paragraphs, images, or links to render. It acts as the structural foundation upon which CSS (styling) and JavaScript (behavior) are built.

### Difference Between Tags, Elements, and Attributes
* **Tags:** Keywords enclosed in angle brackets (e.g., `<p>`, `<h1>`) that instruct the browser on how to interpret content. Most tags come in opening and closing pairs.
* **Elements:** A complete component consisting of an opening tag, content, and a closing tag (e.g., `<p>Hello World</p>`).
* **Attributes:** Extra modifier keywords placed inside an opening tag that provide additional configuration or metadata (e.g., `class`, `id`, `href`, `src`).

### Comprehensive Example
```html
<!DOCTYPE html>
<html>
<head>
    <title>HTML Example Page</title>
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p class="intro-text">This is a paragraph element demonstrating HTML structure.</p>
    <a href="https://www.example.com" target="_blank">Click here to visit Example</a>
</body>
</html>
```
* In the line `<a href="https://www.example.com" target="_blank">Click here to visit Example</a>`:
  * `<a>` is the **tag**.
  * `href="..."` and `target="..."` are **attributes**.
  * The entire structure from `<a` to `</a>` is the **element**.

---

## Question #5: Explain the different types of CSS (inline, internal, and external). Describe when and why each type should be used, providing code examples also Describe the concept of the CSS box model and explain how content, padding, border, and margin interact to define element spacing and layout.

### Types of CSS

#### 1. Inline CSS
* **Description:** Applied directly to individual HTML elements using the `style` attribute.
* **When/Why to use:** Used for quick, one-off styling overrides or dynamic style injections via JavaScript. Not recommended for large-scale designs due to poor maintainability.
* **Example:**
  ```html
  <p style="color: blue; font-size: 18px;">This is an inline styled paragraph.</p>
  ```

#### 2. Internal (Embedded) CSS
* **Description:** Defined within the `<style>` tag located inside the `<head>` section of an HTML document.
* **When/Why to use:** Ideal for styling a single, standalone webpage where you want all CSS rules centralized in one place.
* **Example:**
  ```html
  <head>
      <style>
          body { background-color: #f9f9f9; }
          h2 { color: #333333; }
      </style>
  </head>
  ```

#### 3. External CSS
* **Description:** Written in a separate file with a `.ext` extension (e.g., `styles.css`) and linked to HTML documents via the `<link>` tag.
* **When/Why to use:** The industry standard for production websites. It allows multiple web pages to share the exact same stylesheet, ensuring consistency and easy site-wide updates.
* **Example:**
  ```html
  <!-- In HTML Head -->
  <link rel="stylesheet" href="styles.css">
  ```
  ```css
  /* In styles.css */
  body {
      font-family: Arial, sans-serif;
  }
  ```

---

### The CSS Box Model
Every element in CSS is treated as a rectangular box comprising four concentric layers, moving from the inside out:

1. **Content:** The actual text, image, or media inside the element. Its dimensions are controlled by `width` and `height`.
2. **Padding:** Clear space immediately surrounding the content area, sitting *inside* the border. It increases the inner clearance of the box.
3. **Border:** A visible line or boundary that wraps around the padding and content.
4. **Margin:** Clear space outside the border that separates the element from surrounding elements on the page.

* **Interaction & Layout:** When a browser calculates the total layout space occupied by an element, it sums up the content width/height, padding, borders, and margins. Proper manipulation of these layers prevents layout crowding and ensures clean spacing.

---

## Question #6: Define JavaScript and explain its role in enhancing interactivity and functionality of web applications. Discuss how it differs from HTML and CSS in the web stack and the basic structure of a JavaScript program. Identify key components such as variables, operators, and data types with examples. Also Implement a JavaScript function that manipulates HTML elements using DOM methods like `getElementById()` and `innerHTML`. Explain each method used.

### Definition of JavaScript
**JavaScript (JS)** is a lightweight, interpreted, or JIT-compiled programming language with first-class functions, best known as the scripting language for web pages.

### Role in Web Applications
JavaScript transforms static web pages into dynamic, interactive applications. It handles user interactions (button clicks, form validations, animations, modal popups, and real-time API data fetching) without requiring full page reloads.

### Difference in the Web Stack
* **HTML:** Structure (The Skeleton).
* **CSS:** Presentation & Design (The Skin and Clothes).
* **JavaScript:** Behavior, Logic, and Interactivity (The Brain and Nervous System).

### Basic Structure of a JavaScript Program
```javascript
// Variable declaration and initialization
let greeting = "Hello, World!";

// Function definition with logic
function showMessage(message) {
    console.log(message);
}

// Function call
showMessage(greeting);
```

### Key Components
* **Variables:** Containers for storing data values (`let`, `const`, `var`).
* **Data Types:** Types of values a variable can hold (e.g., String, Number, Boolean, Array, Object).
* **Operators:** Symbols used to perform operations on values (e.g., arithmetic operators `+`, `-`, comparison operators `===`, logical operators `&&`).

```javascript
let studentName = "Ali"; // String
let studentAge = 21;    // Number
let isPassed = true;    // Boolean
let totalScore = 45 + 5;// Arithmetic Operator
```

---

### Implementation: JavaScript Function Using DOM Methods

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>DOM Manipulation Example</title>
</head>
<body>

    <h2 id="main-heading">Original Heading Text</h2>
    <button onclick="updateHeading()">Change Text</button>

    <script>
        function updateHeading() {
            // Using getElementById and innerHTML to modify the DOM
            document.getElementById("main-heading").innerHTML = "Updated! JavaScript successfully modified this element.";
        }
    </script>

</body>
</html>
```

### Explanation of DOM Methods Used:
1. **`document.getElementById("id_name")`:** This method searches the HTML document for an element with the exact specified `id` attribute value and returns that element object as a reference so JavaScript can interact with it.
2. **`.innerHTML`:** This property gets or sets the HTML markup contained within an element. By assigning a new string value to it (`document.getElementById(...).innerHTML = "..."`), we dynamically replace the existing inner content of the target element on the live webpage.
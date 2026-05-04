---
layout: default
title: JavaScript — Complete Guide
---

# JavaScript: Complete Guide for Web Development

A structured walkthrough of every important JavaScript concept — from absolute fundamentals to advanced web APIs.

---

## Table of Contents

1. Fundamentals
2. Variables, Scope & Hoisting
3. Data Types & Type Coercion
4. Operators & Expressions
5. Control Flow
6. Functions (Deep Dive)
7. Objects & Prototypes
8. Arrays & Iteration
9. Strings, Numbers, Dates
10. ES6+ Modern Features
11. Closures & Execution Context
12. The `this` Keyword
13. Classes & OOP
14. Asynchronous JavaScript
15. Error Handling
16. Modules
17. The DOM
18. Events
19. Browser APIs (BOM)
20. Storage (Cookies, localStorage, IndexedDB)
21. Fetch API & AJAX
22. Forms & Validation
23. Web APIs (Geolocation, History, etc.)
24. Performance & Optimization
25. Security Basics
26. Tooling & Ecosystem

---

## 1. Fundamentals

JavaScript is a **single-threaded, dynamically typed, interpreted (JIT-compiled)** language. It runs on browsers (via engines like V8 in Chrome, SpiderMonkey in Firefox) and servers (Node.js, Deno, Bun).

**Three places JS lives in HTML:**

```html
<!-- Inline -->
<button onclick="alert('Hi')">Click</button>

<!-- Internal -->
<script>
  console.log("Hello");
</script>

<!-- External (preferred) -->
<script src="app.js" defer></script>
```

`defer` waits for HTML to parse before running. `async` runs as soon as it's downloaded (order not guaranteed).

**Statements vs Expressions:** A statement performs an action (`let x = 5;`). An expression produces a value (`5 + 3`).

**Strict mode** (`"use strict";`) catches silent errors and is enabled by default in modules and classes.

---

## 2. Variables, Scope & Hoisting

### Declarations

```javascript
var name = "Alice";   // function-scoped, hoisted, can redeclare
let age = 30;         // block-scoped, temporal dead zone
const PI = 3.14;      // block-scoped, must initialize, can't reassign
```

`const` prevents reassignment of the binding, not mutation of the value:

```javascript
const user = { name: "Bob" };
user.name = "Charlie"; // ✅ allowed
user = {};             // ❌ TypeError
```

### Scope

- **Global scope** — accessible everywhere
- **Function scope** — `var` lives here
- **Block scope** — `let`/`const` live inside `{ }`
- **Lexical scope** — inner functions see outer variables

### Hoisting

Declarations are moved to the top of their scope at parse time.

```javascript
console.log(x); // undefined (var hoisted, value is not)
var x = 5;

console.log(y); // ReferenceError (TDZ)
let y = 5;
```

Function declarations are fully hoisted; function expressions are not.

---

## 3. Data Types & Type Coercion

### Primitives (7)
`string`, `number`, `bigint`, `boolean`, `undefined`, `null`, `symbol`

### Reference Type
`object` (includes arrays, functions, dates, regex, etc.)

### Checking Types

```javascript
typeof "hi"        // "string"
typeof 42          // "number"
typeof undefined   // "undefined"
typeof null        // "object" (historical bug)
typeof []          // "object"
typeof function(){} // "function"

Array.isArray([])  // true
```

### Coercion

```javascript
"5" + 3        // "53"   (string concat)
"5" - 3        // 2      (numeric)
"5" == 5       // true   (loose equality coerces)
"5" === 5      // false  (strict equality — always use this)
Boolean("")    // false
Boolean("0")   // true   (non-empty string)
```

**Falsy values:** `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`. Everything else is truthy.

---

## 4. Operators & Expressions

```javascript
// Arithmetic
+ - * / % ** ++ --

// Assignment
= += -= *= /= **= ??=

// Comparison
== != === !== > < >= <=

// Logical
&& || !  ?? // nullish coalescing

// Bitwise
& | ^ ~ << >> >>>

// Ternary
const status = age >= 18 ? "adult" : "minor";

// Optional chaining
user?.address?.city

// Nullish coalescing
const port = process.env.PORT ?? 3000; // only null/undefined trigger fallback
```

---

## 5. Control Flow

```javascript
// if / else if / else
if (x > 0) {} else if (x < 0) {} else {}

// switch
switch (day) {
  case "Mon": case "Tue": work(); break;
  default: rest();
}

// Loops
for (let i = 0; i < 10; i++) {}
for (const item of array) {}      // values
for (const key in object) {}      // keys
while (cond) {}
do {} while (cond);

// Loop control
break;       // exit loop
continue;    // skip iteration
outer: for (...) { break outer; } // labeled
```

---

## 6. Functions (Deep Dive)

### Declaration Forms

```javascript
function add(a, b) { return a + b; }       // declaration (hoisted)
const sub = function(a, b) { return a - b; }; // expression
const mul = (a, b) => a * b;                  // arrow
const div = new Function("a", "b", "return a/b"); // rare
```

### Parameters

```javascript
function greet(name = "Guest") {}        // default
function sum(...nums) {}                 // rest
function profile({ name, age }) {}       // destructured
sum(...[1, 2, 3]);                       // spread
```

### Arrow Functions

- No own `this`, `arguments`, `super`, or `new.target`
- Can't be used as constructors
- Inherit `this` from enclosing scope (great for callbacks)

```javascript
const obj = {
  name: "X",
  regular() { return this.name; },     // "X"
  arrow: () => this.name              // undefined (this = outer)
};
```

### IIFE (Immediately Invoked Function Expression)

```javascript
(function() { console.log("runs once"); })();
```

### First-Class Functions

Functions are values — pass them as arguments, return them, store in arrays/objects.

---

## 7. Objects & Prototypes

```javascript
const user = {
  name: "Ada",
  age: 30,
  greet() { return `Hi, ${this.name}`; },
  get info() { return `${this.name}, ${this.age}`; },
  set info(v) { [this.name, this.age] = v.split(","); }
};

// Property access
user.name;
user["name"];

// Add / delete
user.email = "a@b.com";
delete user.age;
```

### Property Descriptors

```javascript
Object.defineProperty(user, "id", {
  value: 1,
  writable: false,
  enumerable: true,
  configurable: false
});
```

### Useful Methods

```javascript
Object.keys(obj);
Object.values(obj);
Object.entries(obj);
Object.assign({}, obj1, obj2);
Object.freeze(obj);          // immutable
Object.seal(obj);            // can't add/remove
{...obj1, ...obj2}           // spread (shallow copy)
structuredClone(obj);        // deep copy
```

### Prototypal Inheritance

Every object has a hidden link `[[Prototype]]` to another object. Property lookup walks the chain.

```javascript
const animal = { eats: true };
const rabbit = Object.create(animal);
rabbit.jumps = true;
console.log(rabbit.eats); // true (from prototype)
```

---

## 8. Arrays & Iteration

```javascript
const arr = [1, 2, 3];

// Mutating
arr.push(4); arr.pop();
arr.unshift(0); arr.shift();
arr.splice(1, 1, "x");
arr.sort(); arr.reverse();

// Non-mutating
arr.slice(1, 3);
arr.concat([4, 5]);
[...arr, 4, 5];
arr.join("-");

// Higher-order
arr.map(x => x * 2);
arr.filter(x => x > 1);
arr.reduce((acc, x) => acc + x, 0);
arr.find(x => x === 2);
arr.findIndex(x => x === 2);
arr.some(x => x > 2);
arr.every(x => x > 0);
arr.flat(2);
arr.flatMap(x => [x, x]);
arr.includes(2);

// Newer (ES2023)
arr.toSorted(); arr.toReversed(); arr.with(0, 99); // immutable versions
```

---

## 9. Strings, Numbers, Dates

```javascript
// Strings
"hello".length;
"hello".toUpperCase();
"hello".includes("ell");
"hello".replace("l", "L");
"hello".split("");
"  x  ".trim();
"a".padStart(3, "0"); // "00a"
`Template ${variable} ${1 + 2}`;

// Numbers
Number.isInteger(5);
Number.isFinite(Infinity); // false
parseInt("12px");          // 12
parseFloat("1.5e2");       // 150
(3.14159).toFixed(2);      // "3.14"
Math.max(...arr);
Math.random();
Math.floor(Math.random() * 10);

// Dates
const d = new Date();
d.getFullYear(); d.getMonth(); // 0-indexed!
d.toISOString();
Date.now(); // ms since epoch
```

---

## 10. ES6+ Modern Features

### Destructuring

```javascript
const [a, b, ...rest] = [1, 2, 3, 4];
const { name, age: years = 0 } = user;
```

### Template Literals

```javascript
`Hello, ${name}!`
String.raw`\n is literal`
```

### Spread / Rest

```javascript
const merged = { ...a, ...b };
const copy = [...arr];
function f(...args) {}
```

### Optional Chaining & Nullish Coalescing

```javascript
user?.address?.zip ?? "unknown"
arr?.[0]
fn?.()
```

### Symbols & Iterators

```javascript
const id = Symbol("id");
obj[Symbol.iterator] = function*() { yield 1; yield 2; };
```

### Generators

```javascript
function* range(start, end) {
  for (let i = start; i < end; i++) yield i;
}
[...range(0, 5)]; // [0,1,2,3,4]
```

---

## 11. Closures & Execution Context

A **closure** is a function that remembers variables from the scope where it was created — even after that scope has finished executing.

```javascript
function counter() {
  let count = 0;
  return () => ++count;
}
const c = counter();
c(); c(); c(); // 1, 2, 3
```

### Execution Context & Call Stack

Each function call creates a new execution context pushed onto the call stack. The context holds:
- Variable Environment
- Lexical Environment (scope chain)
- `this` binding

JavaScript runs on a single thread with an event loop that orchestrates the **call stack**, **Web APIs**, the **task queue** (macrotasks like `setTimeout`), and the **microtask queue** (Promises, `queueMicrotask`). Microtasks always drain before the next macrotask.

---

## 12. The `this` Keyword

`this` depends on **how a function is called**, not where it's defined.

| Call site | `this` value |
|---|---|
| Method `obj.fn()` | `obj` |
| Standalone `fn()` | `undefined` (strict) / `window` (sloppy) |
| `new Fn()` | newly created object |
| `fn.call(ctx)` / `fn.apply(ctx, args)` | `ctx` |
| `fn.bind(ctx)` | permanently `ctx` |
| Arrow function | inherited from enclosing scope |
| Event handler | the element (in non-arrow) |

---

## 13. Classes & OOP

```javascript
class Animal {
  #species;                          // private field
  static count = 0;                  // static
  
  constructor(name, species) {
    this.name = name;
    this.#species = species;
    Animal.count++;
  }
  
  speak() { return `${this.name} makes a sound`; }
  get species() { return this.#species; }
  static create(name) { return new Animal(name, "?"); }
}

class Dog extends Animal {
  constructor(name) {
    super(name, "dog");
  }
  speak() {
    return super.speak() + " (woof)";
  }
}
```

### Four pillars of OOP in JS
- **Encapsulation** — private fields with `#`, closures
- **Inheritance** — `extends`, `super`
- **Polymorphism** — overriding methods
- **Abstraction** — hiding implementation details

---

## 14. Asynchronous JavaScript

### Callbacks

```javascript
setTimeout(() => console.log("later"), 1000);
fs.readFile("a.txt", (err, data) => {});
```

Nesting them creates "callback hell."

### Promises

A Promise represents a future value with three states: **pending**, **fulfilled**, **rejected**.

```javascript
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve(42), 1000);
});

p.then(val => val + 1)
 .then(console.log)
 .catch(err => console.error(err))
 .finally(() => console.log("done"));
```

### Combinators

```javascript
Promise.all([p1, p2]);          // all resolve, or first rejection
Promise.allSettled([p1, p2]);   // wait for all, never reject
Promise.race([p1, p2]);         // first to settle
Promise.any([p1, p2]);          // first to fulfill
```

### async / await

```javascript
async function getUser(id) {
  try {
    const res = await fetch(`/users/${id}`);
    if (!res.ok) throw new Error("Failed");
    return await res.json();
  } catch (err) {
    console.error(err);
  }
}
```

`async` functions always return a Promise. `await` pauses execution within the async function until the Promise settles.

### Event Loop in Practice

```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
// Output: 1, 4, 3, 2  (microtasks before macrotasks)
```

---

## 15. Error Handling

```javascript
try {
  riskyCode();
} catch (err) {
  if (err instanceof TypeError) {}
  console.error(err.name, err.message, err.stack);
} finally {
  cleanup();
}

// Custom errors
class ValidationError extends Error {
  constructor(msg, field) {
    super(msg);
    this.name = "ValidationError";
    this.field = field;
  }
}
throw new ValidationError("Invalid email", "email");
```

For Promises, attach `.catch()` or use `try/catch` around `await`. Unhandled rejections fire `window.unhandledrejection`.

---

## 16. Modules

### ES Modules (standard)

```javascript
// math.js
export const PI = 3.14;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }

// app.js
import multiply, { PI, add } from "./math.js";
import * as math from "./math.js";
import("./math.js").then(m => {}); // dynamic import
```

In HTML: `<script type="module" src="app.js"></script>` — modules are deferred and strict mode by default.

### CommonJS (Node.js legacy)

```javascript
module.exports = { add };
const { add } = require("./math");
```

---

## 17. The DOM (Document Object Model)

The DOM is a tree of nodes representing the HTML document.

### Selecting

```javascript
document.getElementById("main");
document.querySelector(".btn");          // first match
document.querySelectorAll("li");         // NodeList (static)
document.getElementsByClassName("x");    // HTMLCollection (live)
```

### Traversing

```javascript
el.parentNode; el.parentElement;
el.children; el.childNodes;
el.firstElementChild; el.lastElementChild;
el.nextElementSibling; el.previousElementSibling;
```

### Manipulating

```javascript
el.textContent = "hi";           // text only
el.innerHTML = "<b>hi</b>";      // parses HTML (XSS risk)
el.innerText                     // respects styling

el.setAttribute("data-id", "1");
el.getAttribute("href");
el.dataset.id;                   // data-* attributes

el.classList.add("active");
el.classList.remove("hidden");
el.classList.toggle("on");
el.classList.contains("active");

el.style.color = "red";
getComputedStyle(el).color;
```

### Creating / Inserting

```javascript
const div = document.createElement("div");
div.textContent = "Hello";
parent.appendChild(div);
parent.append(node1, node2, "text");     // multiple
parent.prepend(node);
parent.insertBefore(newNode, refNode);
el.insertAdjacentHTML("beforeend", "<p>x</p>");
el.remove();
el.replaceWith(newEl);
```

### Performance Tips

- Batch DOM reads/writes (avoid layout thrashing)
- Use `DocumentFragment` for many inserts
- Cache references; don't re-query
- Prefer `textContent` over `innerHTML` when possible

---

## 18. Events

### Listening

```javascript
btn.addEventListener("click", handler);
btn.removeEventListener("click", handler);
btn.addEventListener("click", handler, { once: true, passive: true });
```

### Event Object

```javascript
function handler(e) {
  e.target;            // element that triggered
  e.currentTarget;     // element listener attached to
  e.preventDefault();  // stop default behavior
  e.stopPropagation(); // stop bubbling
  e.type;              // "click"
}
```

### Phases

1. **Capture** — root → target (set `{ capture: true }`)
2. **Target** — at the element
3. **Bubble** — target → root (default)

### Event Delegation

Attach one listener to a parent and use `e.target` to handle children. Efficient for many similar elements.

```javascript
list.addEventListener("click", e => {
  if (e.target.matches("li")) handleItem(e.target);
});
```

### Common Events

- Mouse: `click`, `dblclick`, `mousedown/up/move`, `mouseenter/leave`
- Keyboard: `keydown`, `keyup`
- Form: `submit`, `change`, `input`, `focus`, `blur`
- Window: `load`, `DOMContentLoaded`, `resize`, `scroll`
- Touch: `touchstart`, `touchmove`, `touchend`

### Custom Events

```javascript
const evt = new CustomEvent("userLogin", { detail: { id: 1 } });
el.dispatchEvent(evt);
```

---

## 19. Browser APIs (BOM)

```javascript
window.innerWidth; window.innerHeight;
window.scrollTo({ top: 0, behavior: "smooth" });

location.href; location.pathname; location.search;
location.assign(url); location.reload();

history.pushState({}, "", "/new"); history.back();

navigator.userAgent;
navigator.clipboard.writeText("copied");
navigator.geolocation.getCurrentPosition(pos => {});
navigator.onLine;

screen.width; screen.height;

alert("hi"); confirm("ok?"); prompt("name?");
```

### Timers

```javascript
const id = setTimeout(fn, 1000);
clearTimeout(id);
const id2 = setInterval(fn, 1000);
clearInterval(id2);
requestAnimationFrame(tick); // syncs to display refresh (~60fps)
```

---

## 20. Storage

### Cookies
Small (~4KB), sent with every HTTP request. Use for auth tokens with `HttpOnly`, `Secure`, `SameSite`.

```javascript
document.cookie = "name=Alice; max-age=3600; path=/";
```

### localStorage / sessionStorage
String-only (~5–10MB), synchronous, same-origin.

```javascript
localStorage.setItem("key", JSON.stringify(obj));
const data = JSON.parse(localStorage.getItem("key"));
localStorage.removeItem("key");
localStorage.clear();
// sessionStorage clears on tab close
```

### IndexedDB
Async, transactional NoSQL store, gigabytes of structured data. Use a wrapper like Dexie for sanity.

### Cache API
For Service Worker offline caching of network requests.

---

## 21. Fetch API & AJAX

```javascript
const res = await fetch("/api/users", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Ada" }),
  credentials: "include",
  signal: controller.signal // for abort
});

if (!res.ok) throw new Error(res.status);
const data = await res.json();
// Other body parsers: .text(), .blob(), .formData(), .arrayBuffer()
```

### Aborting

```javascript
const controller = new AbortController();
fetch(url, { signal: controller.signal });
controller.abort();
```

The legacy `XMLHttpRequest` still works but `fetch` is preferred everywhere.

---

## 22. Forms & Validation

```javascript
const form = document.querySelector("form");

form.addEventListener("submit", e => {
  e.preventDefault();
  const data = new FormData(form);
  const obj = Object.fromEntries(data);
  // or: data.get("email")
});
```

### HTML5 Constraints

```html
<input required minlength="3" maxlength="20" pattern="[A-Z][a-z]+">
<input type="email" required>
```

```javascript
input.validity.valid;
input.checkValidity();
input.setCustomValidity("Pick a stronger password");
```

---

## 23. Other Web APIs

- **Geolocation** — `navigator.geolocation.getCurrentPosition`
- **History** — `pushState`, `replaceState`, `popstate` event (SPA routing)
- **Intersection Observer** — detect when elements enter the viewport (lazy load, infinite scroll)
- **Mutation Observer** — react to DOM changes
- **Resize Observer** — element size changes
- **Web Workers** — run scripts on background threads
- **Service Workers** — proxy network requests, enable offline & push notifications
- **WebSockets** — full-duplex persistent connection
- **WebRTC** — peer-to-peer audio/video/data
- **Canvas / WebGL** — 2D/3D graphics
- **Notifications** — `new Notification("Hi")`
- **Drag & Drop** — `dragstart`, `dragover`, `drop` events
- **File API** — `<input type="file">`, `FileReader`, `Blob`, `URL.createObjectURL`

---

## 24. Performance & Optimization

- **Debounce** delays a call until activity stops; **throttle** caps frequency.

```javascript
function debounce(fn, ms) {
  let t;
  return (...args) => { clearTimeout(t); t = setTimeout(() => fn(...args), ms); };
}
function throttle(fn, ms) {
  let last = 0;
  return (...args) => { const now = Date.now(); if (now - last >= ms) { last = now; fn(...args); } };
}
```

- **Lazy load** images (`loading="lazy"`) and modules (`import()`).
- **Memoize** pure functions.
- **Virtualize** huge lists (only render what's visible).
- **Web Workers** for CPU-heavy work.
- **Minimize reflows** — read all geometry, then write.
- **Compress, minify, tree-shake, code-split** at build time.
- Use the **Performance API** (`performance.now()`, `performance.mark`, `measure`) and the browser's DevTools Performance panel.

---

## 25. Security Basics

- **XSS** — never inject untrusted strings via `innerHTML`. Prefer `textContent` or sanitize with DOMPurify.
- **CSRF** — use `SameSite` cookies and CSRF tokens.
- **CSP** — Content Security Policy header restricts script sources.
- **HTTPS** — always.
- **CORS** — controls cross-origin requests via response headers.
- Never trust client-side validation alone — validate server-side too.
- Don't store secrets in client code.

---

## 26. Tooling & Ecosystem

- **Package managers** — npm, yarn, pnpm
- **Bundlers** — Vite, esbuild, webpack, Rollup, Parcel
- **Transpilers** — Babel, SWC, TypeScript
- **Linters / Formatters** — ESLint, Prettier, Biome
- **Testing** — Vitest, Jest, Playwright, Cypress
- **Frameworks** — React, Vue, Svelte, Solid, Angular
- **Runtimes** — Node.js, Deno, Bun
- **TypeScript** — typed superset; strongly recommended for any non-trivial project

---

## Quick Mental Models

- **Variables**: `const` by default, `let` when reassigning, never `var`.
- **Equality**: always `===` and `!==`.
- **Async**: prefer `async/await`; use `Promise.all` for parallelism.
- **DOM**: query once, cache, batch updates.
- **Events**: delegate when you can.
- **`this`**: it's about the call site; arrow functions opt out.
- **Modules**: ESM everywhere new.
- **Errors**: throw `Error` instances, catch narrowly, log usefully.

---

*End of guide. Keep this as a reference and revisit topics by writing tiny experiments — JavaScript rewards curiosity in the console.*

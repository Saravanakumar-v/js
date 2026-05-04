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
27. Map & Set
28. WeakMap, WeakSet & WeakRef
29. Regular Expressions
30. JSON
31. Proxy & Reflect
32. Typed Arrays & ArrayBuffer
33. Internationalization (Intl API)
34. Functional Programming
35. Design Patterns
36. Memory Management & Garbage Collection

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

---

## 27. Map & Set

### Map

An ordered collection of key-value pairs where **any value** (including objects) can be a key.

```javascript
const map = new Map();
map.set("name", "Ada");
map.set(42, "answer");
map.set({ id: 1 }, "obj key");

map.get("name");       // "Ada"
map.has(42);           // true
map.size;              // 3
map.delete(42);

// Iteration (insertion order preserved)
for (const [key, val] of map) {}
map.forEach((val, key) => {});
[...map.keys()];
[...map.values()];
[...map.entries()];

// Convert to/from Object
const obj = Object.fromEntries(map);
const map2 = new Map(Object.entries(obj));
```

**When to use Map over Object:** non-string keys, frequent add/delete (better perf), need `.size`, need insertion-order iteration.

### Set

An ordered collection of **unique** values.

```javascript
const set = new Set([1, 2, 3, 2, 1]); // {1, 2, 3}

set.add(4);
set.has(2);    // true
set.delete(2);
set.size;      // 3

for (const val of set) {}

// Deduplicate an array
const unique = [...new Set(arr)];

// Set operations
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);

const union        = new Set([...a, ...b]);          // {1,2,3,4}
const intersection = new Set([...a].filter(x => b.has(x))); // {2,3}
const difference   = new Set([...a].filter(x => !b.has(x))); // {1}
```

---

## 28. WeakMap, WeakSet & WeakRef

### WeakMap

Keys must be **objects**; entries are garbage-collected when the key object is no longer reachable. Not iterable.

```javascript
const cache = new WeakMap();

function process(obj) {
  if (cache.has(obj)) return cache.get(obj);
  const result = expensiveOp(obj);
  cache.set(obj, result);
  return result;
}
// When obj is GC'd, cache entry disappears automatically
```

**Use cases:** private data per object, memoization without memory leaks, storing metadata about DOM nodes.

### WeakSet

Set of objects; membership doesn't prevent GC. Not iterable.

```javascript
const seen = new WeakSet();

function process(node) {
  if (seen.has(node)) return;
  seen.add(node);
  // ...
}
```

### WeakRef

Hold a weak reference to an object; `.deref()` returns the object or `undefined` if collected.

```javascript
let obj = { data: "heavy" };
const ref = new WeakRef(obj);

// later...
const val = ref.deref();
if (val) console.log(val.data);
```

Use `FinalizationRegistry` to run cleanup when an object is GC'd (rare, advanced).

---

## 29. Regular Expressions

```javascript
const re1 = /pattern/flags;
const re2 = new RegExp("pattern", "flags");
```

### Flags

| Flag | Meaning |
|---|---|
| `g` | global — find all matches |
| `i` | case-insensitive |
| `m` | multiline — `^`/`$` match line start/end |
| `s` | dotAll — `.` matches `\n` |
| `u` | unicode |
| `d` | indices — capture group positions |

### Character classes

```
.       any char except \n
\d \D   digit / non-digit
\w \W   word char [a-zA-Z0-9_] / non-word
\s \S   whitespace / non-whitespace
[abc]   a, b, or c
[^abc]  not a, b, or c
[a-z]   range
```

### Quantifiers & Anchors

```
^   start    $   end
*   0+       +   1+       ?   0 or 1
{n} exactly  {n,} at least  {n,m} range
*? +? ??     lazy (shortest match)
```

### Groups & Lookaheads

```javascript
/(foo)(bar)/        // capturing groups — $1, $2
/(?:foo)/           // non-capturing
/(?<year>\d{4})/    // named capture
/foo(?=bar)/        // positive lookahead
/foo(?!bar)/        // negative lookahead
/(?<=foo)bar/       // positive lookbehind
```

### String methods

```javascript
const str = "hello world";

str.match(/\w+/g);                  // ["hello", "world"]
str.matchAll(/(\w+)/g);             // iterator of all matches with groups
str.replace(/world/, "JS");         // "hello JS"
str.replace(/(\w+)/g, "[$1]");      // "[hello] [world]"
str.replaceAll("l", "L");           // "heLLo worLd"
str.search(/world/);                // 6 (index, or -1)
str.split(/\s+/);                   // ["hello", "world"]

/\d+/.test("abc123");               // true
/\d+/.exec("abc123");               // ["123", index: 3, ...]
```

### Common patterns

```javascript
const email    = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const url      = /^https?:\/\/[\w.-]+/;
const phone    = /^\+?[\d\s\-()]{7,15}$/;
const hexColor = /^#([0-9a-f]{3}|[0-9a-f]{6})$/i;
const slug     = /^[a-z0-9]+(?:-[a-z0-9]+)*$/;
```

---

## 30. JSON

JSON (JavaScript Object Notation) is a text format for serializing structured data. Only supports strings, numbers, booleans, null, arrays, and plain objects — **no** functions, `undefined`, `Date`, `Map`, `Set`, `BigInt`, or circular refs.

```javascript
// Serialize
const json = JSON.stringify({ name: "Ada", age: 30 });
// '{"name":"Ada","age":30}'

// Pretty-print
JSON.stringify(obj, null, 2);

// Replacer — filter/transform keys
JSON.stringify(obj, ["name", "age"]);           // only those keys
JSON.stringify(obj, (key, val) =>
  typeof val === "function" ? undefined : val); // drop functions

// Parse
const obj = JSON.parse('{"name":"Ada"}');

// Reviver — transform on parse
JSON.parse(json, (key, val) =>
  key === "date" ? new Date(val) : val);
```

### Gotchas

```javascript
JSON.stringify(undefined);          // undefined (not a string!)
JSON.stringify({ a: undefined });   // '{}'  — key dropped
JSON.stringify([undefined]);        // '[null]'
JSON.stringify(NaN);                // 'null'
JSON.stringify(Infinity);           // 'null'
JSON.stringify(new Date());         // '"2024-01-01T00:00:00.000Z"' (string)

// Circular reference throws
const a = {}; a.self = a;
JSON.stringify(a);                  // TypeError

// Custom serialization
class User {
  toJSON() { return { name: this.name }; } // controls what gets serialized
}
```

### Deep clone (simple objects only)

```javascript
const clone = JSON.parse(JSON.stringify(obj)); // loses Dates, functions, etc.
// Prefer structuredClone() for a proper deep clone
```

---

## 31. Proxy & Reflect

### Proxy

Wraps an object and intercepts fundamental operations via **traps**.

```javascript
const handler = {
  get(target, prop, receiver) {
    console.log(`get: ${prop}`);
    return Reflect.get(target, prop, receiver);
  },
  set(target, prop, value, receiver) {
    if (typeof value !== "number") throw new TypeError("Numbers only");
    return Reflect.set(target, prop, value, receiver);
  },
  has(target, prop) { return prop in target; },
  deleteProperty(target, prop) { return Reflect.deleteProperty(target, prop); }
};

const obj = new Proxy({ x: 1 }, handler);
obj.x;       // logs "get: x", returns 1
obj.x = "s"; // throws TypeError
```

### Common traps

| Trap | Triggered by |
|---|---|
| `get` | `obj.prop`, `obj[prop]` |
| `set` | `obj.prop = val` |
| `has` | `prop in obj` |
| `deleteProperty` | `delete obj.prop` |
| `apply` | calling a proxied function |
| `construct` | `new ProxiedClass()` |
| `ownKeys` | `Object.keys()`, `for...in` |

### Reflect

A built-in object with static methods matching every Proxy trap — the "default behavior" for traps.

```javascript
Reflect.get(target, prop);
Reflect.set(target, prop, value);
Reflect.has(target, prop);         // same as prop in target
Reflect.ownKeys(target);           // all own keys including Symbols
Reflect.apply(fn, thisArg, args);
Reflect.construct(Class, args);
```

**Use cases:** reactive data (Vue 3 reactivity), validation layers, logging/tracing, read-only views, auto-memoization.

---

## 32. Typed Arrays & ArrayBuffer

For working with raw binary data — used in WebGL, audio processing, file I/O, WebSockets, WebAssembly.

### ArrayBuffer

A fixed-length raw binary buffer. You can't read it directly — use a view.

```javascript
const buf = new ArrayBuffer(16); // 16 bytes
buf.byteLength; // 16
```

### Typed Array views

```javascript
const i32 = new Int32Array(buf);     // 4 × 32-bit ints
const f64 = new Float64Array(4);     // allocates its own buffer
const u8  = new Uint8Array([1,2,3]);

i32[0] = 42;
i32.length;       // 4
i32.byteLength;   // 16

// Typed arrays share memory — mutation is visible across views
const ab  = new ArrayBuffer(4);
const u8v = new Uint8Array(ab);
const u16 = new Uint16Array(ab);
u8v[0] = 255;
console.log(u16[0]); // 255 (same bytes)
```

### DataView

Fine-grained control over byte order (endianness).

```javascript
const view = new DataView(buf);
view.setInt32(0, 300, true);       // little-endian
view.getInt32(0, true);            // 300
view.setFloat64(4, 3.14, false);   // big-endian
```

### Common typed array types

`Int8`, `Uint8`, `Uint8Clamped`, `Int16`, `Uint16`, `Int32`, `Uint32`, `Float32`, `Float64`, `BigInt64`, `BigUint64`

---

## 33. Internationalization (Intl API)

Built-in, locale-aware formatting — no library needed.

### Numbers & Currency

```javascript
new Intl.NumberFormat("en-US").format(1234567.89);
// "1,234,567.89"

new Intl.NumberFormat("de-DE", { style: "currency", currency: "EUR" }).format(1234.5);
// "1.234,50 €"

new Intl.NumberFormat("en", { style: "percent" }).format(0.752);
// "75%"

new Intl.NumberFormat("en", { notation: "compact" }).format(1_500_000);
// "1.5M"
```

### Dates & Times

```javascript
const d = new Date();

new Intl.DateTimeFormat("en-US", { dateStyle: "full" }).format(d);
// "Wednesday, January 1, 2025"

new Intl.DateTimeFormat("ja-JP", { dateStyle: "short", timeStyle: "short" }).format(d);
// "2025/01/01 12:00"

new Intl.RelativeTimeFormat("en", { numeric: "auto" }).format(-1, "day");
// "yesterday"

new Intl.RelativeTimeFormat("en").format(-3, "month");
// "3 months ago"
```

### Strings & Sorting

```javascript
// Locale-aware string comparison (for sorting)
["Ångström", "Zebra", "apple"].sort(
  new Intl.Collator("en", { sensitivity: "base" }).compare
);

// Plural rules
const pr = new Intl.PluralRules("en");
pr.select(1);   // "one"
pr.select(2);   // "other"
```

### List formatting

```javascript
new Intl.ListFormat("en", { style: "long", type: "conjunction" })
  .format(["apples", "bananas", "oranges"]);
// "apples, bananas, and oranges"
```

---

## 34. Functional Programming

### Pure functions

Always return the same output for the same input; no side effects.

```javascript
// impure — mutates external state
let total = 0;
const addToTotal = n => (total += n);

// pure
const add = (a, b) => a + b;
```

### Immutability

```javascript
// Instead of mutating, return new values
const updated = { ...user, age: 31 };
const newArr  = [...arr, newItem];
const without = arr.filter(x => x !== item);
```

### Higher-order functions

Functions that take or return other functions.

```javascript
const withLogging = fn => (...args) => {
  console.log("calling with", args);
  const result = fn(...args);
  console.log("result:", result);
  return result;
};
const loggedAdd = withLogging(add);
```

### Currying

Transform a multi-argument function into a chain of single-argument functions.

```javascript
const multiply = a => b => a * b;
const double   = multiply(2);
double(5); // 10

// General curry helper
const curry = fn => {
  const arity = fn.length;
  return function curried(...args) {
    return args.length >= arity
      ? fn(...args)
      : (...more) => curried(...args, ...more);
  };
};
```

### Composition

Combine functions so output of one flows into input of the next.

```javascript
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
const pipe    = (...fns) => x => fns.reduce((v, f) => f(v), x);

const process = pipe(
  str => str.trim(),
  str => str.toLowerCase(),
  str => str.replace(/\s+/g, "-")
);
process("  Hello World  "); // "hello-world"
```

### Memoization

Cache results of pure functions.

```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const fib = memoize(n => n <= 1 ? n : fib(n - 1) + fib(n - 2));
```

### Partial application

```javascript
const partial = (fn, ...preset) => (...later) => fn(...preset, ...later);

const add3 = partial(add, 3);
add3(5); // 8
```

---

## 35. Design Patterns

### Creational

**Factory** — creates objects without `new` at call site.
```javascript
function createUser(name, role) {
  return { name, role, createdAt: Date.now() };
}
```

**Singleton** — one instance per application.
```javascript
const db = (() => {
  let instance;
  return {
    getInstance() {
      if (!instance) instance = { connected: false };
      return instance;
    }
  };
})();
```

**Builder** — construct complex objects step by step.
```javascript
class QueryBuilder {
  #table = ""; #conditions = []; #limit = null;
  from(table)  { this.#table = table; return this; }
  where(cond)  { this.#conditions.push(cond); return this; }
  take(n)      { this.#limit = n; return this; }
  build() {
    let q = `SELECT * FROM ${this.#table}`;
    if (this.#conditions.length) q += ` WHERE ${this.#conditions.join(" AND ")}`;
    if (this.#limit) q += ` LIMIT ${this.#limit}`;
    return q;
  }
}
new QueryBuilder().from("users").where("age > 18").take(10).build();
```

### Structural

**Module** — encapsulate private state with a public API (the revealing module pattern).
```javascript
const counter = (() => {
  let count = 0;
  return {
    increment() { count++; },
    decrement() { count--; },
    value()     { return count; }
  };
})();
```

**Decorator** — add behaviour to an object without changing its class.
```javascript
function readonly(target, key, descriptor) {
  descriptor.writable = false;
  return descriptor;
}
```

**Facade** — simplified interface over a complex subsystem.
```javascript
const VideoPlayer = {
  play(url) {
    Decoder.init();
    Buffer.load(url);
    Renderer.start();
  }
};
```

### Behavioural

**Observer** — publish/subscribe.
```javascript
class EventEmitter {
  #listeners = new Map();
  on(event, fn)  { (this.#listeners.get(event) ?? this.#listeners.set(event, new Set()).get(event)).add(fn); }
  off(event, fn) { this.#listeners.get(event)?.delete(fn); }
  emit(event, ...args) { this.#listeners.get(event)?.forEach(fn => fn(...args)); }
}
```

**Strategy** — swap algorithms at runtime.
```javascript
const sorters = {
  bubble: arr => { /* ... */ },
  quick:  arr => { /* ... */ },
};
function sort(arr, strategy = "quick") {
  return sorters[strategy](arr);
}
```

**Command** — encapsulate actions as objects (enables undo).
```javascript
const history = [];
function execute(cmd) {
  cmd.execute();
  history.push(cmd);
}
function undo() {
  history.pop()?.undo();
}
```

**Middleware / Chain of Responsibility** — pass a request through a chain of handlers (Express-style).
```javascript
function applyMiddleware(...fns) {
  return (req, res) => {
    let i = 0;
    const next = () => fns[i++]?.(req, res, next);
    next();
  };
}
```

---

## 36. Memory Management & Garbage Collection

JavaScript uses **automatic garbage collection** — the engine reclaims memory that is no longer reachable.

### Reachability

An object is kept alive as long as it's reachable from a root (global scope, call stack, closures). When all references are gone, the GC can collect it.

```javascript
let user = { name: "Ada" }; // object reachable via `user`
user = null;                 // now unreachable → eligible for GC
```

### Common memory leaks

```javascript
// 1. Forgotten global variables
function leak() { forgotten = "oops"; } // implicit global

// 2. Detached DOM nodes still referenced
const btn = document.getElementById("btn");
document.body.removeChild(btn);
// btn variable still holds a reference — node can't be collected

// 3. Closures holding large data unnecessarily
function outer() {
  const bigData = new Array(1_000_000).fill("x");
  return function inner() {
    return bigData[0]; // bigData can't be GC'd while inner lives
  };
}

// 4. Uncleared timers / intervals
const id = setInterval(() => { /* uses outerVar */ }, 1000);
// if you never call clearInterval(id), outerVar is pinned forever

// 5. Event listeners on removed elements
el.addEventListener("click", handler);
el.remove(); // listener + closure still alive if handler holds references
el.removeEventListener("click", handler); // fix
```

### Tools

- **Chrome DevTools → Memory tab**: heap snapshots, allocation timelines
- **`performance.measureUserAgentSpecificMemory()`** (Chrome, requires `cross-origin-isolated`)
- **WeakMap / WeakRef** — let objects be GC'd while still holding a reference (see §28)

### V8 GC strategy (brief)

V8 uses a **generational** GC:
- **Young generation (Scavenger)**: short-lived objects; collected frequently and fast.
- **Old generation (Mark-Compact)**: long-lived objects; collected less often.

Allocation in tight loops creates GC pressure — prefer object pools or reuse arrays where performance matters.

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
- **Collections**: `Map` for key-value with non-string keys, `Set` for unique values.
- **Memory**: clear timers, remove listeners, avoid unintended closures over large data.
- **FP**: pure functions + immutability + composition scale better than mutation-heavy code.

---

*End of guide. Keep this as a reference and revisit topics by writing tiny experiments — JavaScript rewards curiosity in the console.*

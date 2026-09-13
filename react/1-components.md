# React Phase 1 — Components, JSX & Rendering

## 1. Component — Why?

A React component is a **named unit of UI**.

```jsx
function Welcome() {
  return <h1>Hello Harsha</h1>;
}
```

Why use a component?

* Gives a piece of UI a clear boundary.
* Makes UI reusable.
* Gives the UI a place for its own data and behavior later.
* Helps organize large applications into a UI tree.

Example:

```text
App
├── Navbar
├── Welcome
├── ProductList
└── Footer
```

### Mental model

> **Component = a function that describes a piece of UI.**

---

## 2. JSX — What?

JSX allows us to write **HTML-like UI syntax inside JavaScript**.

```jsx
function Welcome() {
  return <h1>Hello Harsha</h1>;
}
```

JSX is **not HTML**.

It is a convenient syntax for describing UI inside JavaScript.

Conceptually:

```text
JavaScript + JSX
       ↓
UI description
       ↓
React
       ↓
Browser UI
```

JSX can use JavaScript values:

```jsx
function Welcome() {
  const name = "Harsha";

  return <h1>Hello {name}</h1>;
}
```

This is useful because UI often depends on JavaScript **data and logic**.

---

## 3. Rendering — What happens?

When React encounters:

```jsx
<Welcome />
```

conceptually:

```text
<Welcome />
     ↓
React renders the component
     ↓
Welcome() executes
     ↓
JSX is returned
     ↓
React processes the UI description
     ↓
React updates the DOM
     ↓
Browser displays the UI
```

Important:

> React does **not** send JSX directly to the browser.

The browser doesn't understand JSX. React's renderer turns the UI description into actual DOM updates.

---

## 4. A Component Must Return a UI Tree

### No return

```jsx
function Welcome() {
  <h1>Hello Harsha</h1>;
}
```

The function returns `undefined`.

React has nothing to render.

### Plain text

```jsx
function Welcome() {
  return "Hello <h1>Harsha</h1>";
}
```

The entire value is treated as **text**.

The `<h1>` does not become an HTML heading.

### Multiple sibling elements

This is invalid:

```jsx
function Welcome() {
  return (
    <h1>Hello</h1>
    <p>Harsha</p>
  );
}
```

A component's return must represent **one UI tree**.

We can create that tree with a parent:

```jsx
function Welcome() {
  return (
    <div>
      <h1>Hello</h1>
      <p>Harsha</p>
    </div>
  );
}
```

---

## 5. `<Welcome />` vs `Welcome()`

These can produce the same visible result in a simple example:

```jsx
<Welcome />
```

```jsx
{Welcome()}
```

But their meaning is different.

### `<Welcome />`

```text
Tell React:
"This UI contains the Welcome component."
```

It is a **component in the declarative UI tree**.

### `Welcome()`

```text
Tell JavaScript:
"Execute this function."
```

It is a normal JavaScript function call.

### Important distinction

```jsx
<Welcome />
```

→ React component

```jsx
{Welcome()}
```

→ JavaScript expression that calls a function

```jsx
Welcome()
```

→ normal JavaScript function call

Don't think of these as interchangeable just because a simple example produces the same output.

---

# Core Mental Model

Keep only this in your head:

```text
COMPONENT
    ↓
describes
    ↓
UI

JSX
    ↓
syntax for describing
    ↓
UI inside JavaScript

<Welcome />
    ↓
React renders component
    ↓
component returns JSX
    ↓
React updates DOM
    ↓
Browser shows UI
```

## The 80/20 takeaway

> **React lets us describe the UI as a tree of components, where components are functions that describe pieces of that UI.**

For now, don't worry about React internals such as the reconciler or Fiber.

## Retrieval Questions

Before moving forward, you should be able to answer these without looking:

1. What problem does a component solve?
2. Why is a component a function?
3. What is JSX?
4. Is JSX HTML? Why?
5. What happens when React encounters `<Welcome />`?
6. Why doesn't JSX go directly to the browser?
7. What happens if a component doesn't return anything?
8. Why does `return "Hello <h1>..."` produce text?
9. What's the difference between `<Welcome />` and `Welcome()`?
10. Why is `<Welcome />` useful for building a UI tree?

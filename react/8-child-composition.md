# React Composition + children

## 1. The Problem

Suppose several components need the same structure:

```text
┌─────────────────────┐
│       Panel         │
│                     │
│   different content │
│                     │
└─────────────────────┘
```

We want the component to control the **structure/layout**, while the parent decides **what content goes inside**.

Example:

```jsx
<Panel>
  <ProductInfo />
</Panel>

<Panel>
  <LoginForm />
</Panel>

<Panel>
  <button>Buy</button>
</Panel>
```

The `Panel` shouldn't need to know what content it will contain.

This is the problem **composition** solves.

---

# 2. Composition

Composition means:

> **Build a component by allowing its parent to provide the content that goes inside it.**

Example:

```jsx
function Panel({ children }) {
  return (
    <div className="panel">
      {children}
    </div>
  );
}
```

Usage:

```jsx
<Panel>
  <h1>Hello</h1>
  <p>Learning React</p>
</Panel>
```

Mental model:

```text
Parent decides WHAT
       ↓
    children
       ↓
Component decides WHERE
```

---

# 3. What is `children`?

`children` is a **special React prop** that represents the content placed between a component's opening and closing tags.

Example:

```jsx
<Panel>
  <p>Hello</p>
</Panel>
```

Conceptually:

```text
Panel
  ↓
props.children
  ↓
<p>Hello</p>
```

The component can decide where to render it:

```jsx
function Panel({ children }) {
  return (
    <section>
      <h2>Panel</h2>

      <main>
        {children}
      </main>
    </section>
  );
}
```

---

# 4. `children` Is Not the DOM

Important distinction:

```text
children
   ↓
React element / renderable content
   ↓
React renderer
   ↓
Actual DOM
```

For:

```jsx
<Panel>
  <button>Buy</button>
</Panel>
```

`children` represents a **React element describing the button**.

It is not the actual DOM `<button>` node.

React later uses that description to create/update the DOM.

This connects to the earlier mental model:

```text
JSX
 ↓
UI description
 ↓
React
 ↓
DOM
```

---

# 5. `children` Can Have Different Forms

Do not assume `children` is always one element.

### Text

```jsx
<Panel>
  Hello
</Panel>
```

Conceptually:

```text
children → "Hello"
```

### One React element

```jsx
<Panel>
  <h1>Hello</h1>
</Panel>
```

Conceptually:

```text
children → one React element
```

### Multiple React elements

```jsx
<Panel>
  <h1>Hello</h1>
  <p>World</p>
</Panel>
```

Conceptually:

```text
children
 ├── <h1>Hello</h1>
 └── <p>World</p>
```

Therefore:

> `children` represents whatever renderable content the parent places inside the component.

---

# 6. Why `children` Makes Components Reusable

Without composition, we might tightly couple a component to specific content.

With composition:

```jsx
function Panel({ children }) {
  return (
    <div className="panel">
      {children}
    </div>
  );
}
```

The same `Panel` can contain:

```jsx
<Panel>
  <ProductInfo />
</Panel>
```

or:

```jsx
<Panel>
  <LoginForm />
</Panel>
```

or:

```jsx
<Panel>
  <button>Buy</button>
</Panel>
```

The layout stays the same while the content changes.

---

# 7. `children` Is Not the Only Way to Compose

We could also pass content as a normal prop:

```jsx
<Panel content={<p>Hello</p>} />
```

```jsx
function Panel({ content }) {
  return (
    <div>
      <h2>Panel</h2>
      {content}
    </div>
  );
}
```

So `children` does **not** create reusability.

Its advantage is that it provides a natural API for nested content:

```jsx
<Panel>
  <p>Hello</p>
</Panel>
```

instead of:

```jsx
<Panel content={<p>Hello</p>} />
```

---

# 8. Responsibility

Composition separates responsibilities.

### Parent

Decides:

> **What content should go inside?**

### Component

Decides:

> **How and where should that content be structured/rendered?**

Example:

```text
Parent
  │
  │ provides content
  ↓
Panel
  │
  │ provides structure
  ↓
UI
```

---

# 9. 80/20 Mental Model

Think of:

```jsx
<Panel>
  CONTENT
</Panel>
```

as:

```text
Panel = structure
CONTENT = children
```

The parent says:

> "Here's what I want inside you."

The component says:

> "I'll decide where it goes."

---

# 10. Retrieval Questions

1. What problem does composition solve?
2. What is `children`?
3. Who decides what goes into `children`?
4. Who decides where `children` is rendered?
5. Can `children` be multiple elements?
6. Is `children` an actual DOM node?
7. What is the difference between `children` and a normal `content` prop?
8. Why does composition make components reusable?

### One sentence to remember

> **Composition lets a component own the structure while its parent provides the content through `children`.**
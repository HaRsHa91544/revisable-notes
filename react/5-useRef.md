# React Phase 2: useRef

## 1. Why `useRef` Exists

Sometimes a component needs to **remember a value between renders**, but changing that value should **not trigger a re-render**.

That is the problem `useRef` solves.

```jsx
const valueRef = useRef(0);
```

---

## 2. Core Mental Model

```text
useRef
   ↓
React preserves the ref object between renders
   ↓
ref.current can change
   ↓
Changing ref.current does NOT trigger a render
```

Example:

```jsx
const countRef = useRef(0);

countRef.current++;
```

The value changes, but React does not automatically re-render.

---

## 3. State vs Ref

|                                | `useState`           | `useRef`                                        |
| ------------------------------ | -------------------- | ----------------------------------------------- |
| Persists between renders       | Yes                  | Yes                                             |
| Changing value triggers render | Yes                  | No                                              |
| Value accessed as              | `count`              | `ref.current`                                   |
| Mainly used for                | Data that affects UI | Persistent data that does not directly drive UI |

Mental model:

```text
State
→ Remember + render when changed

Ref
→ Remember without rendering when changed
```

---

## 4. Ref Persists Across Re-renders

```jsx
const countRef = useRef(0);
```

Conceptually:

```text
Render 1
→ ref.current = 0

ref.current++

Render 2
→ ref.current is still 1
```

The component function runs again, but React preserves the ref object associated with that component.

---

## 5. `useRef` Does Not Prevent DOM Recreation

`useRef` does not tell React:

> "Don't create a new DOM element."

React's reconciliation process decides whether an existing DOM node can be reused.

A DOM ref simply gives the component access to the DOM node.

```jsx
const inputRef = useRef(null);

<input ref={inputRef} />
```

Then:

```jsx
inputRef.current.focus();
```

Mental model:

```text
React manages DOM
      ↓
ref.current
      ↓
Reference to specific DOM node
```

---

## 6. Why Not `querySelector()`?

You can use:

```js
document.querySelector("#email");
```

But `useRef` directly connects the component to the element it owns:

```jsx
const inputRef = useRef(null);

<input ref={inputRef} />
```

Then:

```js
inputRef.current.focus();
```

Use `useRef` when you genuinely need imperative access to a specific element.

Examples:

* Focus an input
* Scroll to an element
* Measure an element
* Interact with certain DOM APIs
* Integrate with third-party DOM libraries

Do not use refs when normal React state and rendering can solve the problem.

---

## 7. Ref and UI

Changing a ref does not update the UI automatically.

```jsx
const countRef = useRef(0);

countRef.current++;
```

```text
ref.current changes
      ↓
No render
      ↓
UI remains unchanged
```

But if another state update causes a render:

```text
state changes
      ↓
render
      ↓
React reads ref.current
      ↓
UI may display the latest value
```

---

# Core Definition

> **`useRef` lets a component persist a value across renders without causing a re-render when that value changes.**

For DOM elements:

> **`useRef` provides persistent access to a specific DOM node.**

---

# Retrieval Questions

## Basic

1. What problem does `useRef` solve?
2. Does changing `ref.current` trigger a re-render?
3. Does a ref persist between renders?
4. Where is the value of a ref accessed?
5. What is the difference between `useState` and `useRef`?

## DOM

6. Why can `useRef` be useful instead of `querySelector()`?
7. What does `inputRef.current` contain when used with:

   ```jsx
   <input ref={inputRef} />
   ```
8. Does `useRef` prevent React from creating or replacing DOM nodes?

## Mental Model

9. Why does this not update the UI?

   ```js
   countRef.current++;
   ```
10. Why can the updated ref value appear after some other state causes a render?
11. When should you use a ref instead of state?
12. When should you avoid using a ref?
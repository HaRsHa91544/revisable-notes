# React Phase 1: useEffect

## 1. The Problem

React rendering should primarily calculate:

> **What should the UI look like?**

But applications also need to interact with things outside React:

* APIs
* Browser APIs
* Timers
* WebSockets
* Subscriptions
* External libraries

These are side effects.

---

## 2. What is `useEffect`?

`useEffect` allows a component to **synchronize with something outside React** after rendering.

```jsx
useEffect(() => {
  fetch("/users");
}, []);
```

Mental model:

```text
Render
  ↓
Calculate UI
  ↓
React updates DOM
  ↓
Effect runs
  ↓
Synchronize with outside system
```

Core definition:

> **Render calculates UI. Effect synchronizes with the outside world.**

---

## 3. Why Not Put Effects in the Component Body?

Everything in the component body can execute whenever the component renders.

Example:

```jsx
function Products() {
  fetch("/products");

  return <h1>Products</h1>;
}
```

If the parent re-renders:

```text
Parent state changes
      ↓
Parent renders
      ↓
Products renders
      ↓
fetch() runs again
```

Therefore, external synchronization should be handled with `useEffect`.

---

## 4. Dependencies

```jsx
useEffect(() => {
  fetch(`/users/${userId}`);
}, [userId]);
```

The dependency array describes:

> **What values this synchronization depends on.**

If `userId` changes:

```text
userId = 1
    ↓
Effect → fetch user 1

userId = 2
    ↓
Cleanup previous effect
    ↓
Effect → fetch user 2
```

### Common forms

```jsx
useEffect(callback);
```

Runs after every render.

```jsx
useEffect(callback, []);
```

Runs after initial mount and does not re-run because of dependency changes.

Development Strict Mode can cause an additional setup and cleanup cycle.

```jsx
useEffect(callback, [count]);
```

Runs after initial mount and when `count` changes.

---

## 5. Cleanup

An effect can return a cleanup function.

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => {
    clearInterval(timer);
  };
}, []);
```

Mental model:

```text
Effect
  ↓
Start / synchronize
  ↓
Cleanup
  ↓
Stop / undo previous synchronization
```

Cleanup runs:

1. Before the effect runs again because dependencies changed
2. When the component unmounts

For dependency changes:

```text
Render
  ↓
Cleanup previous effect
  ↓
Run new effect
```

---

## 6. Important Distinction

Do not think:

> `useEffect` means "run code after rendering."

That describes when it runs, but not its purpose.

Think:

> **`useEffect` synchronizes React state/props with an external system.**

Examples:

```jsx
fetch("/users");
document.title = name;
setInterval(...);
WebSocket connection;
```

---

## 7. Rendering vs Effect

### Rendering

Pure calculations based on props/state:

```jsx
const fullName = firstName + lastName;
const total = price * quantity;
```

```text
Props/State
    ↓
Calculation
    ↓
JSX
```

### Effect

Interaction with something outside React:

```jsx
fetch("/users");
document.title = name;
setInterval(...);
```

```text
Props/State
    ↓
Effect
    ↓
Outside React
```

`document.title` is an effect because it modifies the browser's document. It is synchronous, but it is still outside React.

---

## 8. Avoid Unnecessary Effects

If something can be calculated during rendering, don't use an effect.

Bad:

```jsx
useEffect(() => {
  setTotal(price * quantity);
}, [price, quantity]);
```

Better:

```jsx
const total = price * quantity;
```

Why?

`total` is derived from existing state/props. No external synchronization is required.

---

## 9. Infinite Effect Loop

This can create an infinite loop:

```jsx
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

Flow:

```text
count changes
   ↓
Render
   ↓
Effect
   ↓
setCount()
   ↓
count changes
   ↓
Render
   ↓
Effect
   ↓
...
```

Core lesson:

> **Don't use an effect to calculate or update state when that creates a dependency cycle.**

---

# Core Mental Model

```text
React Rendering
      ↓
Calculate UI
      ↓
Update DOM
      ↓
Effect
      ↓
Synchronize with external system
      ↓
Cleanup when synchronization is replaced or removed
```

Remember:

> **Render = describe UI**

> **Effect = synchronize with outside React**

> **Dependency = what the synchronization depends on**

> **Cleanup = stop or undo the previous synchronization**

---

# Retrieval Questions

## Understanding

1. What problem does `useEffect` solve?
2. What is the difference between rendering and an effect?
3. Why can putting `fetch()` directly in the component body cause repeated requests?
4. Why is `document.title = name` an effect?
5. Is `document.title = name` asynchronous? Why or why not?

## Dependencies

6. What does `[count]` mean in an effect?
7. What happens when `count` changes?
8. What is the difference between:

   ```jsx
   useEffect(callback)
   ```

   and:

   ```jsx
   useEffect(callback, [])
   ```
9. What does an empty dependency array mean?
10. Why are dependencies better understood as synchronization dependencies rather than simply "things that trigger code"?

## Cleanup

11. Why does an effect need a cleanup function?
12. When does cleanup run?
13. Explain:

```text
Render → Cleanup → Effect
```

14. Give an example where failing to clean up causes a problem.

## Mental Model

15. Should `const total = price * quantity` be inside an effect? Why?
16. Why can this create an infinite loop?

```jsx
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

17. Explain `useEffect` in one sentence without mentioning its syntax.
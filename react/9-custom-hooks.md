# React Custom Hooks

## 1. The Problem

Suppose two components need the same stateful logic:

```text
Counter
 ├── count state
 └── increment logic

Score
 ├── count state
 └── increment logic
```

The UI is different, but the logic is duplicated.

We want to reuse the **logic** without forcing the components to share the same state.

---

# 2. What Is a Custom Hook?

A Custom Hook is a function that extracts reusable **stateful React logic**.

Example:

```jsx
function useCounter() {
  const [count, setCount] = useState(0);

  function increment() {
    setCount(prev => prev + 1);
  }

  return {
    count,
    increment
  };
}
```

A component can use it:

```jsx
function Counter() {
  const { count, increment } = useCounter();

  return (
    <>
      <h1>{count}</h1>
      <button onClick={increment}>+</button>
    </>
  );
}
```

---

# 3. Custom Hooks Share Logic, Not State

This is the most important idea.

If:

```jsx
function Counter() {
  const counter = useCounter();
}
```

and:

```jsx
function Score() {
  const counter = useCounter();
}
```

then:

```text
Counter
 └── useCounter() → State A

Score
 └── useCounter() → State B
```

The logic is shared.

The state is independent.

Therefore:

> **Custom Hooks are not a state-sharing mechanism.**

If components need the same actual state, use things like:

* Lifting State Up
* Context
* Other state-management solutions

---

# 4. Component Instances Have Separate State

Suppose:

```jsx
function App() {
  return (
    <>
      <Counter />
      <Counter />
    </>
  );
}
```

There are two `Counter` instances.

React maintains separate state for each:

```text
Counter instance A
 └── count = 3

Counter instance B
 └── count = 0
```

Even though both execute the same component function.

The important idea:

> `useState` gives state to a component instance, not to the component function globally.

---

# 5. Why Custom Hooks Start With `use`

Example:

```jsx
function useCounter() {
  // Hooks
}
```

The `use` prefix communicates that this function is a Hook and follows the Rules of Hooks.

It also allows React tooling/linting to recognize Hook usage patterns.

---

# Rules of Hooks

## 6. The Core Reason

React needs to reliably associate each Hook call with its preserved state/effect.

The mental model:

```text
Component instance

Hook 1 → State/effect 1
Hook 2 → State/effect 2
Hook 3 → State/effect 3
```

On every render, React needs the Hook calls to occur in the same order.

---

# 7. Why Conditional Hooks Break

❌ Don't:

```jsx
if (loggedIn) {
  useState("Harsha");
}

useState(20);
```

First render:

```text
loggedIn = true

Hook 1 → name
Hook 2 → age
```

Next render:

```text
loggedIn = false

Hook 1 → age
```

The Hook sequence changed.

Conceptually:

```text
Render 1:
1 → name
2 → age

Render 2:
1 → age
```

React can no longer reliably associate the Hook call with the intended state.

---

# 8. Correct Pattern

Instead of conditionally calling the Hook:

```jsx
if (loggedIn) {
  useEffect(...);
}
```

call the Hook unconditionally:

```jsx
useEffect(() => {
  if (loggedIn) {
    console.log("logged in");
  }
});
```

Now every render has:

```text
Hook 1 → useEffect
```

The condition only controls what happens **inside** the Hook.

---

# 9. Hooks and Loops

❌ Don't:

```jsx
for (let i = 0; i < 3; i++) {
  useState(0);
}
```

The number/order of Hook calls can depend on the loop.

Hooks need a predictable structure.

---

# 10. Hooks and Nested Functions

❌ Don't:

```jsx
function getName() {
  const [name, setName] = useState("Harsha");
}
```

and then call:

```jsx
getName();
```

This is an arbitrary nested function, not a Custom Hook.

Instead:

```jsx
function useName() {
  const [name, setName] = useState("Harsha");

  return name;
}
```

Then:

```jsx
function App() {
  const name = useName();

  return <h1>{name}</h1>;
}
```

Custom Hooks are specifically designed to contain Hook logic.

---

# 11. State Is Not Recreated Every Render

Don't think:

```text
Render
 ↓
new state created
```

Instead:

```text
Component renders
 ↓
component function executes again
 ↓
React provides the component's preserved state
```

That's why:

```jsx
const [count, setCount] = useState(0);
```

continues to have the previous value after re-renders.

---

# 12. The "Numbered Seats" Mental Model

A useful conceptual model:

```text
Component instance

Seat 1 → useState
Seat 2 → useState
Seat 3 → useEffect
```

Every render must encounter them in the same order:

```text
Render 1:
1 → useState
2 → useState
3 → useEffect

Render 2:
1 → useState
2 → useState
3 → useEffect
```

Not:

```text
Render 1:
1 → useState
2 → useState

Render 2:
1 → useState
```

The exact internal implementation is more complex, but this model is enough to understand the Rules of Hooks.

---

# 13. 80/20 Mental Model

### Custom Hook

> **Extract reusable stateful logic so multiple components can use the same behavior without duplicating it.**

### State

> **Each component instance gets its own state.**

### Rules of Hooks

> **Hooks must be called in a consistent order on every render so React can reliably associate them with the component's preserved state/effects.**

---

# Retrieval Questions

1. What problem do Custom Hooks solve?
2. Do Custom Hooks share state or logic?
3. Why can two components use the same Custom Hook but have independent state?
4. What is a component instance?
5. Why must Hook call order remain stable?
6. Why is a conditional Hook problematic?
7. Why is putting the condition inside the Hook usually okay?
8. Why can't Hooks be called inside arbitrary nested functions?
9. Why does a Custom Hook start with `use`?
10. What does the "numbered seats" model represent?

### One sentence to remember

> **Custom Hooks share reusable stateful logic, while each component instance keeps its own state, and Hook calls must remain in the same order across renders.**
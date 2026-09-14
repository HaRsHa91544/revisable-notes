# React Phase 1: State

## 1. Why State Exists

A component's UI can change over time.

Example:

```jsx
Count: 0

[ + ]

Count: 1
```

The problem is:

> When data changes, how does React know that the UI needs to update?

`useState` solves this.

---

## 2. What is State?

State is data that:

* Belongs to a component
* Can change over time
* Is preserved between renders
* Can trigger a re-render when updated

```jsx
const [count, setCount] = useState(0);
```

Mental model:

```text
State
  ↓
React remembers it
  ↓
State setter
  ↓
React schedules update
  ↓
Component renders again
  ↓
UI is updated
```

---

## 3. State Persists Between Renders

A normal variable is recreated every time the component function runs.

```jsx
function Counter() {
  let count = 0;
}
```

Conceptually:

```text
Render 1 → count = 0
Render 2 → count = 0
Render 3 → count = 0
```

State is different.

```jsx
const [count, setCount] = useState(0);
```

React associates the state with the component and preserves it between renders.

```text
React stores state
      ↓
Counter renders
      ↓
count = 5
      ↓
Counter renders again
      ↓
React gives count = 5
```

Think:

> State is owned and remembered by React, not by the local function variable.

---

## 4. State Setter

```jsx
setCount(10);
```

The setter does not immediately change the `count` variable of the current render.

Conceptually:

```text
setCount(10)
    ↓
React records update
    ↓
React renders again
    ↓
New render receives count = 10
```

Therefore:

> The state value inside a render represents the state for that particular render.

---

## 5. Normal State Update

```jsx
setCount(count + 1);
```

The expression uses the `count` value from the current render.

If:

```text
count = 0
```

then:

```jsx
setCount(count + 1);
```

means:

```jsx
setCount(1);
```

Multiple calls can therefore use the same current value:

```jsx
setCount(count + 1);
setCount(count + 1);
```

Both can effectively request:

```text
setCount(1)
setCount(1)
```

Result:

```text
1
```

---

## 6. Functional State Update

When the next state depends on the previous state:

```jsx
setCount(prev => prev + 1);
```

`prev` represents the latest state available when React processes that update.

Multiple updates:

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Conceptually:

```text
Current state = 0

Update 1 → prev = 0 → 1
Update 2 → prev = 1 → 2

Final state = 2
```

Mental model:

```text
setState(value)
→ Set the next state to this value

setState(prev => ...)
→ Calculate the next state from the latest state
```

---

## 7. State vs Normal Variable

```text
Normal variable
→ recreated on every render
→ changing it does not tell React to render

State
→ preserved by React
→ setter tells React about the update
→ can cause a re-render
```

Example:

```jsx
count++;
```

Changes a JavaScript variable but does not notify React.

```jsx
setCount(count + 1);
```

Requests a state update and tells React that the component may need to render again.

---

## 8. Re-rendering

When state changes:

```text
State update
    ↓
Component renders again
    ↓
New JSX/UI description
    ↓
React compares with previous result
    ↓
Necessary DOM updates
```

Re-rendering the component does not mean React blindly recreates the entire DOM.

---

## 9. Same State Value

If the new state is considered equal to the current state, React can skip the unnecessary update.

Example:

```jsx
setName("Harsha");
```

If the state is already `"Harsha"`, no meaningful state change occurred.

---

# Core Mental Model

> **State is React-managed data that persists between renders and whose updates can cause the component to render again.**

```text
User action
    ↓
State setter
    ↓
React stores update
    ↓
Re-render
    ↓
Component gets new state
    ↓
New UI description
    ↓
Necessary DOM update
```

# Retrieval Questions

Answer without looking at the notes.

### Basic

1. What problem does `useState` solve?
2. What is state?
3. Why does a normal variable not persist between renders?
4. How does React preserve state between renders?
5. Does `setCount()` immediately change the `count` variable in the current render?
6. What causes a component to render again?

### State Updates

7. Why can this result in `1` instead of `2`?

```jsx
setCount(count + 1);
setCount(count + 1);
```

8. What does `prev` represent here?

```jsx
setCount(prev => prev + 1);
```

9. Why does this produce `2`?

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

10. What is the difference between `count++` and `setCount(count + 1)`?

### Mental Model

11. Explain how state persists even though the component function runs again.
12. Explain the complete flow from a button click to the updated UI.
13. Why doesn't React need to recreate the entire DOM on every re-render?
14. When should you use a functional state updater?
# React Phase 1: Props

## 1. Why Props Exist

A component often needs different data from its parent.

Without props:

```jsx
function Welcome() {
  return <h1>Hello Harsha</h1>;
}
```

With props:

```jsx
function Welcome(props) {
  return <h1>Hello {props.name}</h1>;
}

<Welcome name="Harsha" />
<Welcome name="Rahul" />
```

**Core idea:**

> Props allow a parent to pass data into a child component.

---

## 2. Props Mental Model

Think of props like function inputs.

```text
Function argument → Function

Props             → Component
```

Example:

```jsx
<Welcome name="Harsha" />
```

Conceptually:

```text
Parent
  ↓
{ name: "Harsha" }
  ↓
Welcome(props)
  ↓
props.name
  ↓
UI
```

---

## 3. Props Are Read-Only

A child should read props, not modify them.

```jsx
function Welcome(props) {
  return <h1>{props.name}</h1>;
}
```

Reading:

```js
props.name
```

Allowed.

Mutating:

```js
props.name = "Rahul";
```

Not allowed.

### Why?

The parent owns the data.

```text
Parent owns data
      ↓
    Props
      ↓
Child reads data
```

Props are an input contract between parent and child.

---

## 4. Destructuring Does Not Change Ownership

These are equivalent for accessing the prop:

```jsx
function User(props) {
  return <h1>{props.name}</h1>;
}
```

```jsx
function User({ name }) {
  return <h1>{name}</h1>;
}
```

Destructuring only extracts values from the props object.

It does not give the child ownership of the data.

For object props:

```jsx
function User({ user }) {
  return <h1>{user.name}</h1>;
}
```

The child should not mutate:

```js
user.name = "Rahul";
```

Even though JavaScript may allow the mutation in some situations, it violates React's one-way data flow principle.

---

## 5. One-Way Data Flow

React follows the principle:

```text
Parent
   ↓
Props
   ↓
Child
```

Data flows downward.

The child does not directly modify the parent's data.

If the child needs to cause a change in parent-owned data, another mechanism is needed. We will learn this with State.

---

# Core Mental Model

> **Props are read-only inputs passed from a parent to a child.**

Remember:

```text
Parent owns data
      ↓
   passes props
      ↓
Child receives props
      ↓
Child reads props
      ↓
UI
```

---

# Retrieval Questions

Answer these without looking at the notes.

### Basic

1. What problem do props solve?
2. What is the relationship between props and function arguments?
3. What happens when `<Welcome name="Harsha" />` is rendered?
4. Why are props considered read-only?
5. Who owns the data passed through props?

### Mental Model

6. Explain this flow:

```text
Parent → Props → Child → UI
```

7. Does destructuring props give the child ownership of the data? Why?
8. Why is `user.name = "Rahul"` considered bad React practice?
9. Can a child directly change parent-owned data?
10. If a child needs to cause a change in parent data, what problem do we need to solve next?
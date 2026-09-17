# React Forms - First Principles

## 1. Why Forms Need State

An HTML input can maintain its own value in the DOM.

But React often needs to know the current value for:

* Validation
* Displaying data
* Submitting to an API
* Enabling/disabling UI
* Business logic

So we can make React state the source of truth.

---

## 2. Controlled Input

```jsx
const [name, setName] = useState("");

<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

Mental model:

```text
User types
    ↓
Browser changes input value
    ↓
onChange fires
    ↓
setName(...)
    ↓
React state changes
    ↓
Re-render
    ↓
value={name}
    ↓
Input displays new value
```

### Core definition

> A controlled input is an input whose value is controlled by React state.

---

## 3. Why `value` Needs `onChange`

```jsx
<input value={name} />
```

This tells React:

> "The input's value must come from `name`."

If `name` never changes, the input cannot become editable through React.

Therefore, a controlled mutable input normally needs:

```jsx
value={state}
onChange={...}
```

If the field is intentionally read-only:

```jsx
<input value={name} readOnly />
```

---

## 4. `defaultValue`

```jsx
<input defaultValue="Harsha" />
```

`defaultValue` provides the initial value.

After initialization, the browser/DOM manages the input value.

Mental model:

```text
defaultValue
    ↓
Initial DOM value
    ↓
Browser manages it
```

Compare:

```text
value
→ React controls the value

defaultValue
→ Browser controls the value after initialization
```

---

## 5. Multiple Form Fields

Instead of separate state:

```jsx
const [name, setName] = useState("");
const [email, setEmail] = useState("");
const [phone, setPhone] = useState("");
```

we can model one conceptual form as one state object:

```jsx
const [form, setForm] = useState({
  name: "",
  email: "",
  phone: ""
});
```

Why?

Because these values represent one conceptual entity:

```text
Form
 ├── name
 ├── email
 └── phone
```

The important principle is not "always use one state object."

> Choose state structure based on what the data represents and how it changes.

---

## 6. Updating One Field in an Object

We should not mutate the existing state object:

```jsx
form.name = "Harsha"; // ❌
```

Instead create a new object:

```jsx
setForm({
  ...form,
  name: "Harsha"
});
```

The spread operator copies the existing properties and the later property replaces the old value.

Example:

```js
{
  ...form,
  name: e.target.value
}
```

Mental model:

```text
Old form
   ↓
Copy properties
   ↓
Replace one property
   ↓
New object
   ↓
setForm(newObject)
```

---

## 7. Computed Property Names

If the field name is stored in a variable:

```js
const fieldName = "email";
```

we can use:

```js
{
  ...form,
  [fieldName]: "hello@gmail.com"
}
```

This produces:

```js
{
  ...form,
  email: "hello@gmail.com"
}
```

The brackets mean:

> Use the value of this variable as the property name.

---

## 8. One Handler for Multiple Inputs

Give each input a `name`:

```jsx
<input
  name="name"
  value={form.name}
  onChange={handleChange}
/>

<input
  name="email"
  value={form.email}
  onChange={handleChange}
/>
```

Then:

```jsx
function handleChange(e) {
  const { name, value } = e.target;

  setForm({
    ...form,
    [name]: value
  });
}
```

### Why this works

`e.target` is the input that triggered the event.

For:

```jsx
<input name="email" />
```

we get:

```js
e.target.name  // "email"
e.target.value // current input value
```

So one handler can update different fields.

Mental model:

```text
Input
 ├── name → tells us WHICH field
 └── value → tells us WHAT value

        ↓

handleChange

        ↓

setForm({
  ...form,
  [name]: value
})
```

---

## 9. Where Does `e.target.value` Come From?

The browser handles the physical typing.

When the user types:

```text
H
```

the browser updates the input's DOM value.

Then the event fires.

```text
User types H
    ↓
Browser input.value = "H"
    ↓
onChange event
    ↓
e.target.value = "H"
```

React then stores that value in state.

So React is not physically detecting the keyboard input itself.

> The browser handles the input interaction; the event gives React the new value.

---

## 10. Form Submission

Use:

```jsx
<form onSubmit={handleSubmit}>
```

and:

```jsx
function handleSubmit(e) {
  e.preventDefault();

  console.log(form);
}
```

### Why `preventDefault()`?

Forms have default browser behavior, such as submitting/navigating.

```js
e.preventDefault();
```

prevents that default action.

It does NOT stop the event itself.

```text
submit event
    ↓
handler runs
    ↓
preventDefault()
    ↓
browser default action is prevented
    ↓
our React logic continues
```

---

## 11. Validation

Validation can happen during submission:

```jsx
function handleSubmit(e) {
  e.preventDefault();

  if (form.name.trim().length === 0) {
    return;
  }

  // Submit form
}
```

Mental model:

```text
User fills form
      ↓
React state contains current data
      ↓
Submit
      ↓
Validate state
      ↓
Valid → submit
Invalid → stop/show error
```

---

## 12. Live Validation

If the requirement is to show errors while typing, validation can be derived during rendering:

```jsx
const nameError = form.name.trim() === "";
```

Then:

```jsx
{nameError && <p>Name is required</p>}
```

Mental model:

```text
form.name
    ↓
derive validation result
    ↓
render UI
```

### Important React principle

> If something can be calculated from existing state or props, don't automatically create another state variable for it.

For example, this may be unnecessary:

```jsx
const [nameError, setNameError] = useState("");
```

if the error can simply be calculated from:

```jsx
form.name
```

---

# 80/20 Core Mental Model

For controlled forms, remember:

```text
                React State
                    ↑
                    │
                onChange
                    ↑
                    │
                User Input
                    │
                    ↓
                 Input
```

More completely:

```text
User types
    ↓
Browser changes input value
    ↓
onChange
    ↓
Update React state
    ↓
Re-render
    ↓
React gives new value to input
    ↓
UI reflects state
```

### Retrieval Questions

1. Why do controlled inputs need `value` + `onChange`?
2. What is the difference between `value` and `defaultValue`?
3. Why shouldn't we mutate `form.name` directly?
4. Why do we use `...form`?
5. What does `[name]` mean inside an object?
6. What are `e.target.name` and `e.target.value`?
7. Why do we use `e.preventDefault()` on form submission?
8. Why can validation sometimes be derived during render instead of stored in state?

### One sentence to remember

> **A controlled React form keeps the current input data in React state, updates that state through events, and renders the UI from that state.**

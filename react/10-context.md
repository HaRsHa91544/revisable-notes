# React Context

## 1. The Problem: Prop Drilling

Suppose:

```text
App
 ↓
Layout
 ↓
Navbar
 ↓
UserMenu
 ↓
Avatar
```

`App` owns `user`, but only `Avatar` needs it.

Without Context:

```text
App
 ↓ user
Layout
 ↓ user
Navbar
 ↓ user
UserMenu
 ↓ user
Avatar
```

The intermediate components don't need `user`. They are only passing it through.

This is called **prop drilling**.

---

# 2. What Context Solves

Context allows a component to make data available to deeply nested descendants without manually passing it through every intermediate component.

```text
App
 ↓
Provider
 ↓
Layout
 ↓
Navbar
 ↓
UserMenu
 ↓
Avatar
             ↑
          useContext()
```

The intermediate components don't need to know about the data.

### 80/20 Definition

> **Context allows data to be shared with deeply nested components without prop drilling.**

---

# 3. Creating a Context

```jsx
import { createContext } from "react";

const UserContext = createContext(null);
```

The argument to `createContext()` is the **default value**.

Here:

```jsx
createContext(null)
```

means:

> If no matching Provider exists above the consumer, the consumer receives `null`.

---

# 4. Provider

A Provider makes a value available to its descendants.

```jsx
function App() {
  const user = {
    name: "Harsha"
  };

  return (
    <UserContext.Provider value={user}>
      <Layout />
    </UserContext.Provider>
  );
}
```

Everything inside the Provider can access the context.

```text
UserContext.Provider
        ↓
      Layout
        ↓
      Navbar
        ↓
     Profile
```

---

# 5. Consuming Context

A descendant uses `useContext()`:

```jsx
import { useContext } from "react";

function Profile() {
  const user = useContext(UserContext);

  return <h2>{user.name}</h2>;
}
```

Mental model:

```text
Provider
   ↓
value
   ↓
useContext()
   ↓
component receives value
```

---

# 6. Context Does NOT Own the Data

This distinction is extremely important.

Usually:

```jsx
const [user, setUser] = useState("Harsha");
```

owns the state.

Context distributes it:

```jsx
<UserContext.Provider value={user}>
```

So:

```text
useState
   ↓
owns the data

Context
   ↓
distributes access to the data
```

Context is not a replacement for state.

---

# 7. Context + State

A common pattern:

```jsx
function App() {
  const [user, setUser] = useState("Harsha");

  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Profile />
    </UserContext.Provider>
  );
}
```

Now deeply nested components can both read and update the state:

```jsx
function Profile() {
  const { user, setUser } = useContext(UserContext);

  return (
    <>
      <h2>{user}</h2>

      <button onClick={() => setUser("Ravi")}>
        Switch User
      </button>
    </>
  );
}
```

The important idea:

```text
State → owns
Context → distributes
Consumer → uses
```

---

# 8. Context Is Limited to Its Subtree

A Provider only affects components underneath it.

```jsx
<UserContext.Provider value="Harsha">
  <Profile />
</UserContext.Provider>

<Profile />
```

The first `Profile` receives:

```text
"Harsha"
```

The second receives the default value:

```text
null
```

because it is outside the Provider.

---

# 9. Nearest Provider Wins

You can have nested Providers.

```jsx
<UserContext.Provider value="Harsha">

  <Navbar />

  <UserContext.Provider value="Ravi">
    <Profile />
  </UserContext.Provider>

</UserContext.Provider>
```

`Navbar` receives:

```text
Harsha
```

`Profile` receives:

```text
Ravi
```

The closest matching Provider is used.

Mental model:

> **Nearest Provider wins.**

---

# 10. Context Updates

Consider:

```jsx
function App() {
  const [user, setUser] = useState("Harsha");

  return (
    <UserContext.Provider value={user}>
      <Profile />
    </UserContext.Provider>
  );
}
```

When:

```jsx
setUser("Ravi");
```

happens:

```text
State changes
   ↓
App renders again
   ↓
Provider gets new value
   ↓
Context value changes
   ↓
Consumers update
```

---

# 11. Context Uses Reference Identity

This is an important detail.

### Primitive value

```jsx
<UserContext.Provider value={user}>
```

If:

```text
user = "Harsha"
```

and it remains `"Harsha"`, the value is unchanged.

### Object value

```jsx
<UserContext.Provider value={{ user }}>
```

A new object is created every render:

```text
Render 1 → Object A
Render 2 → Object B
```

Even though both contain:

```js
{ user: "Harsha" }
```

they are different references.

```js
Object.is(ObjectA, ObjectB);
// false
```

Therefore the Context value is considered changed.

### Mental model

> **Context consumers respond when the Provider's value changes by identity/reference.**

---

# 12. Context Should Not Automatically Replace Props

Context is not:

> "A better version of props."

Use props when the relationship is simple:

```text
App
 ↓
Profile
```

```jsx
<Profile user={user} />
```

That's clear and explicit.

Consider Context when:

```text
App
 ↓
A
 ↓
B
 ↓
C
 ↓
D
 ↓
Profile
```

and `A`, `B`, `C`, and `D` don't actually need `user`.

Then Context can remove unnecessary prop drilling.

### Rule

> **Use props for explicit/local relationships.**
>
> **Consider Context when deeply nested components need shared data and prop drilling becomes cumbersome.**

---

# 13. Separate Contexts by Responsibility

Don't automatically create one giant Context:

```text
AppContext
 ├── user
 ├── theme
 ├── cart
 ├── language
 └── notifications
```

Instead, unrelated responsibilities can have separate contexts:

```text
UserContext
ThemeContext
CartContext
LanguageContext
NotificationContext
```

The key question is:

> **Do these pieces of data belong to the same responsibility and usually change together?**

If yes, they can reasonably share a Context.

If they represent different responsibilities, separate them.

---

# 14. Context + Custom Hook

Instead of writing this everywhere:

```jsx
const { user, setUser } = useContext(UserContext);
```

create a custom hook:

```jsx
function useUser() {
  return useContext(UserContext);
}
```

Then:

```jsx
function Profile() {
  const { user, setUser } = useUser();

  return <h2>{user}</h2>;
}
```

Now components don't need to know how the User Context is implemented.

---

# 15. Safer Custom Hook

Since the default value can be `null`:

```jsx
const UserContext = createContext(null);
```

we can detect incorrect usage:

```jsx
function useUser() {
  const context = useContext(UserContext);

  if (context === null) {
    throw new Error(
      "useUser must be used inside UserProvider"
    );
  }

  return context;
}
```

Now:

```text
Wrong usage
    ↓
useUser()
    ↓
No Provider
    ↓
Clear error
```

This is a common real-world pattern.

---

# 16. The Complete Mental Model

```text
                State
                  │
                  │ owns
                  ↓
              user data
                  │
                  ↓
              Provider
                  │
                  │ distributes
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Navbar    Profile   Settings
                  │
                  │
             useUser()
                  │
                  ↓
              user data
```

Responsibilities:

```text
useState
   ↓
owns changing data

Context
   ↓
makes data available deeply

useContext / Custom Hook
   ↓
accesses the data
```

---

# 17. The 80/20 Rules

Remember these:

1. **Context solves prop drilling.**
2. **Provider makes a value available to descendants.**
3. **useContext reads the nearest matching Provider's value.**
4. **Context only affects its subtree.**
5. **Nearest Provider wins.**
6. **Context does not own state.**
7. **State owns data; Context distributes access.**
8. **Context consumers respond when the Context value changes.**
9. **Objects/functions can create new references even when their contents look unchanged.**
10. **Don't use Context just because you can. Use it when it solves a real communication problem.**
11. **Separate unrelated responsibilities into separate contexts.**
12. **Custom Hooks can hide Context implementation details and provide a clean API.**

---

# One-Sentence Mental Model

> **Context is a way to make shared data available deep in a component tree without manually passing it through components that don't need it.**

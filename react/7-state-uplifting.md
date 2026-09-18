# React: Lifting State Up

## 1. The Problem

Suppose we have two sibling components:

```text
       App
      /   \
     ↓     ↓
  Button  Display
```

`Button` changes some data.

`Display` needs to show that data.

If the state lives inside `Button`:

```text
Button
  └── count state

Display
  └── needs count
```

The problem is:

> `Display` cannot directly access state owned by its sibling.

---

# 2. The Solution

Move the shared state to their **closest common parent**.

```text
              App
         owns count state
            /       \
           ↓         ↓
       Button      Display
       updates       reads
```

Example:

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <CounterDisplay count={count} />
      <IncrementButton
        onIncrement={() => setCount(prev => prev + 1)}
      />
    </>
  );
}
```

---

# 3. Why Lift State Up?

The important question is not:

> "Should I always keep state in the parent?"

Instead ask:

> **"Which components need this data?"**

If multiple components need the same state, put that state in their **closest common parent**.

Example:

```text
App
 ├── ProductForm
 │      needs to modify products
 │
 └── ProductList
        needs to read products
```

Therefore:

```text
products state → App
```

---

# 4. Data Flow

React follows one-way data flow.

The parent passes data down:

```text
Parent
  ↓
props
  ↓
Child
```

Example:

```jsx
<ProductList products={products} />
```

The child receives the current state as a prop.

---

# 5. How Does a Child Change Parent State?

A child does not directly modify the parent's state.

Instead, the parent passes a callback.

```jsx
<ProductForm onAddProduct={addProduct} />
```

The child calls:

```jsx
onAddProduct(product);
```

The parent owns the actual state update:

```jsx
function addProduct(product) {
  setProducts(prev => [...prev, product]);
}
```

The flow becomes:

```text
Child
  ↓
calls callback
  ↓
Parent
  ↓
updates state
  ↓
Parent re-renders
  ↓
new props flow down
  ↓
Children receive updated data
```

---

# 6. The Important Mental Model

Think of the parent as the **owner of shared data**.

```text
                 App
          owns shared state
             /        \
            ↓          ↓
         Child A     Child B
         modifies      reads
             │
             ↓
          callback
             │
             ↓
            App
       updates state
```

---

# 7. Don't Think "Pass the Setter"

You can technically do:

```jsx
<ProductForm setProducts={setProducts} />
```

But a better component API is often:

```jsx
<ProductForm onAddProduct={addProduct} />
```

Why?

Because the child only needs to know:

> "Tell the parent that a product should be added."

It doesn't need to know how the parent stores products.

This keeps responsibilities clearer.

---

# 8. Break Experiment

We intentionally put state in the wrong component:

```text
       App
      /   \
     ↓     ↓
 Button  Display

Button owns count
Display needs count
```

Questions:

### Who knows the count?

```text
IncrementButton
```

### Who needs the count?

```text
CounterDisplay
```

### What's the problem?

They are siblings.

The state is owned by one sibling but needed by another.

A sibling cannot directly pass its local state to another sibling.

Therefore, the natural solution is:

```text
Move state upward
       ↓
Closest common parent
       ↓
Pass data down as props
       ↓
Pass callbacks down for changes
```

---

# 9. Example: Product Management

```jsx
function App() {
  const [products, setProducts] = useState([]);

  function addProduct(product) {
    setProducts(prev => [...prev, product]);
  }

  return (
    <>
      <ProductForm onAddProduct={addProduct} />
      <ProductList products={products} />
    </>
  );
}
```

Mental model:

```text
                 App
        products state lives here
             /          \
            ↓            ↓
     ProductForm      ProductList
     calls action      receives data
            │
            ↓
      onAddProduct()
            │
            ↓
           App
            │
            ↓
    setProducts(...)
            │
            ↓
       re-render
            │
            ↓
    ProductList gets
    updated products
```

---

# 10. 80/20 Definition

> **Lifting state up means moving shared state to the closest common parent of the components that need it.**

Then:

```text
Parent owns state
     ↓
Data flows down
     ↓
Callbacks flow down
     ↓
Child calls callback
     ↓
Parent updates state
     ↓
New data flows down
```

---

# Retrieval Questions

1. Why can't two sibling components directly share local state?
2. Where should shared state live?
3. What does "closest common parent" mean?
4. How does a child request a state change from its parent?
5. Why is passing a specific callback often better than passing the raw setter?
6. What happens after the parent updates the state?
7. In `App → ProductForm + ProductList`, why should `products` belong to `App`?

### One sentence to remember

> **If multiple components need the same changing data, let their closest common parent own it, pass the data down, and pass callbacks down for children to request changes.**
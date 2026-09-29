# React.memo

## 1. Problem it solves

When a parent re-renders, its child normally renders again even if the child's props haven't changed.

```text
Parent state changes
       ↓
Parent renders
       ↓
Child renders again
```

`React.memo` can skip that unnecessary child render.

---

## 2. Basic usage

```jsx
const ProductList = React.memo(function ProductList({ products }) {
  return <div>...</div>;
});
```

---

## 3. Mental model

```text
Parent renders
      ↓
React.memo checks child's props
      ↓
Props unchanged?
   ↙          ↘
 YES          NO
  ↓            ↓
Skip render   Render
```

By default, React compares props using shallow comparison, so objects/arrays/functions are effectively compared by reference.

---

## 4. Example

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        {count}
      </button>

      <ProductList products={products} />
    </>
  );
}
```

If `count` changes but:

```text
products → same array reference
```

then:

```text
App renders
   ↓
React.memo checks ProductList props
   ↓
products reference unchanged
   ↓
ProductList render is skipped
```

---

## 5. Reference identity matters

This can prevent memoization from helping:

```jsx
<ProductList products={{ ...products }} />
```

because a new object is created:

```text
Previous → Object A
Current  → Object B

A !== B
```

Therefore React considers the prop changed.

Same idea applies to arrays and functions.

---

## 6. Very important distinction

### `React.memo`

Optimizes a **component**.

```jsx
const ProductList = React.memo(ProductListComponent);
```

### `useMemo`

Memoizes a **calculated value**.

```jsx
const filteredProducts = useMemo(
  () => products.filter(...),
  [products]
);
```

Don't confuse them.

---

## 7. What `React.memo` does NOT mean

It does not mean:

> "This component will never render again."

It means:

> "If its props haven't changed, React may skip rendering it when its parent renders."

If the component's **own state** changes, it still needs to render.

If a **Context value it consumes** changes, it can also render.

---

## 8. 80/20 rule

> **Use `React.memo` when a component frequently receives the same props while its parent re-renders for unrelated reasons, especially when the component is expensive to render.**

Don't add `React.memo` everywhere.

First identify an actual unnecessary-render problem.

---

## One-line mental model

> **`React.memo` lets React skip a child component's render when its props are unchanged.**
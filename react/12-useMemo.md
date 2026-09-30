# useMemo Hook

## 1. What problem does it solve?

`useMemo` prevents React from **recalculating the same value unnecessarily** when its dependencies haven't changed.

```jsx
const result = useMemo(
  () => expensiveCalculation(data),
  [data]
);
```

---

## 2. Mental model

> **`useMemo` remembers a calculated VALUE.**

```text
Render 1
data = A
↓
calculate → Result X
↓
remember X

Render 2
data = A
↓
dependency unchanged
↓
reuse X

Render 3
data = B
↓
dependency changed
↓
calculate again → Result Y
```

---

## 3. What does the dependency array mean?

```jsx
useMemo(() => calculate(data), [data]);
```

It means:

> **Reuse the previous result while `data` is unchanged. Recalculate when `data` changes.**

It does NOT mean:

> "Run only when `data` changes."

The calculation runs initially too.

---

## 4. Reference identity matters

For objects, arrays, and functions, React compares references.

```text
Array A === Array A → unchanged
Array A !== Array B → changed
```

So this creates a new dependency every render:

```jsx
const products = [...oldProducts];
```

Even if the contents are identical.

Therefore `useMemo` recalculates.

---

## 5. Common use

### Expensive calculation

```jsx
const filteredProducts = useMemo(
  () => expensiveFilter(products, searchValue),
  [products, searchValue]
);
```

### Stable object value

Useful for Context:

```jsx
const productsContextValue = useMemo(
  () => ({ products, setProducts }),
  [products]
);
```

If `count` changes but `products` doesn't:

```text
count changes
↓
App renders
↓
products unchanged
↓
same context value is reused
```

---

## 6. `useMemo` vs `React.memo` vs `useEffect`

|              | Purpose                                                    |
| ------------ | ---------------------------------------------------------- |
| `useMemo`    | Remember a **calculated value**                            |
| `React.memo` | Skip a **component render** when props are unchanged       |
| `useEffect`  | **Perform/synchronize an effect** when dependencies change |

Mental model:

```text
useMemo     → Remember a VALUE
React.memo → Skip a COMPONENT render
useEffect   → DO something
```

---

## 7. Important warning

Don't use `useMemo` everywhere.

For simple calculations:

```jsx
const total = price * quantity;
```

just calculate it normally.

Use `useMemo` when there is a meaningful reason to avoid recalculation or to preserve a stable reference.

---

## 8. One-line definition

> **`useMemo` caches the result of a calculation and reuses it until one of its dependencies changes.**
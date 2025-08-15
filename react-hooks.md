# 🚀 React Hooks Cheat Sheet (Interview + Production Use Cases)

## 1. `useState`

* **What it does:** Manages state in a functional component.
* **Re-render effect:** Component re-renders when state changes.
* **Production use case:**

  * Form inputs (`username`, `password`).
  * Toggle UI elements (dropdowns, modals).
  * Chat input field.

```jsx
const [count, setCount] = useState(0);
```

---

## 2. `useEffect`

* **What it does:** Runs side effects after render.
* **Re-render effect:** Runs after render. Can cause re-renders if updating state.
* **Production use case:**

  * Fetch API data on mount.
  * Subscribe to WebSocket events.
  * Sync document title with state.

```jsx
useEffect(() => {
  fetchData();
}, []); // Runs once on mount
```

---

## 3. `useLayoutEffect`

* **What it does:** Similar to `useEffect`, but runs **synchronously after DOM updates** and **before paint**.
* **Re-render effect:** Runs before the browser paints. Can block paint if heavy.
* **Production use case:**

  * Measure DOM size/position before paint (tooltips, modals).
  * Scroll to bottom in chat apps after new message.

```jsx
useLayoutEffect(() => {
  scrollToBottom();
}, [messages]);
```

---

## 4. `useCallback`

* **What it does:** Memoizes a function so it isn’t recreated on every render.
* **Re-render effect:** Prevents unnecessary re-renders of child components.
* **Production use case:**

  * Passing event handlers to child components in chat/trading apps.
  * Avoiding performance issues in lists with many items.

```jsx
const sendMessage = useCallback((text) => {
  socket.emit("send_message", text);
}, [socket]);
```

---

## 5. `useMemo`

* **What it does:** Memoizes a computed value.
* **Re-render effect:** Skips recalculation unless dependencies change.
* **Production use case:**

  * Expensive calculations (portfolio value, analytics).
  * Filtering large lists.
  * Caching chart data.

```jsx
const total = useMemo(() => {
  return orders.reduce((sum, o) => sum + o.amount, 0);
}, [orders]);
```

---

## 6. `useReducer`

* **What it does:** Alternative to `useState` for complex state logic. Uses actions + reducer.
* **Re-render effect:** Similar to `useState`, re-renders when state changes.
* **Production use case:**

  * Shopping cart (add/remove/clear items).
  * Chat app message state (send, edit, delete).
  * Form with multiple fields & validations.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "add": return [...state, action.item];
    case "remove": return state.filter(i => i.id !== action.id);
    default: return state;
  }
}
const [cart, dispatch] = useReducer(reducer, []);
```

---

## 7. `useContext`

* **What it does:** Accesses values from a Context provider (avoids prop drilling).
* **Re-render effect:** All consumers re-render when context value changes.
* **Production use case:**

  * Authentication (user data + token).
  * Theme (light/dark).
  * Language/Localization.

```jsx
const { theme } = useContext(ThemeContext);
```

---

# 🔄 Quick Re-render Impact Table

| Hook              | Renders on Change?          | Typical Use Case in Production |
| ----------------- | --------------------------- | ------------------------------ |
| `useState`        | Yes                         | Forms, toggles, chat inputs    |
| `useEffect`       | Indirect (if state updates) | Fetch API, subscriptions       |
| `useLayoutEffect` | Yes (before paint)          | Measure DOM, scroll sync       |
| `useCallback`     | No (memoizes fn)            | Pass handlers to children      |
| `useMemo`         | No (memoizes value)         | Expensive computations         |
| `useReducer`      | Yes                         | Complex state (cart, chat)     |
| `useContext`      | Yes (all consumers)         | Auth, theme, settings          |

---
 make this into a **downloadable `.md` file** so you can keep it in your prep folder and glance before interviews?

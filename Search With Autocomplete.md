# Search with autocomplete 

1. **Input Setup**  
   - `useState` for `searchText`.  
   - `<input value={searchText} onChange={...} />`.  

2. **Make Input Dynamic**  
   - `onChange` updates `searchText` → triggers `useEffect`.

3. **Fetch Data**  
   - Create `fetchData()` → call API with `searchText`.  
   - Store in `result` state.

4. **Show Data**  
   - Map `result` to render suggestions.  
   - Show/hide using `onFocus` / `onBlur`.

5. **Debounce**  
   - In `useEffect`, wrap `fetchData()` with `setTimeout`.  
   - Cleanup with `clearTimeout`.

6. **Cache**  
   - `useState({})` for cache.  
   - If `cache[searchText]` exists → use it.  
   - Else fetch → update cache.

**Flow:** Input → Debounce → Check Cache → Fetch → Store & Display.

### Code
```javascript
import { useEffect, useState } from "react";
import "./styles.css";

export default function App() {
  const [searchText, setSearchText] = useState("");
  const [result, setResult] = useState([]);
  const [showRes, setShowRes] = useState(false);
  const [cache, setCache] = useState({});

  const fetchData = async () => {
    console.log("API Called", searchText);
    if (cache[searchText]) {
      console.log("Cached", searchText);
      setResult(cache[searchText]);
      return;
    }
    const response = await fetch(
      "https://dummyjson.com/recipes/search?q=" + searchText
    );
    const json = await response.json();
    // console.log(json?.recipes);
    setResult(json?.recipes);
    setCache((prev) => ({ ...prev, [searchText]: json?.recipes }));
  };

  useEffect(() => {
    const timer = setTimeout(() => fetchData(), 500);
    return () => {
      clearTimeout(timer);
    };
  }, [searchText]);
  return (
    <div className="App">
      <input
        className="input"
        value={searchText}
        onChange={(e) => setSearchText(e.target.value)}
        onFocus={() => setShowRes(true)}
        onBlur={() => setShowRes(false)}
      />
      {showRes && (
        <div className="res-container">
          {result.map((res) => (
            <span key={res.id}>{res?.name}</span>
          ))}
        </div>
      )}
    </div>
  );
}

```

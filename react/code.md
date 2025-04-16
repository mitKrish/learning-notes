```javascript
import { useRef } from "react";
import { useState } from "react";

export default function App() {
  const ref = useRef(0);
  const [count, setCount] = useState(0);

  function handleRefClick() {
    ref.current = ref.current + 1;
    alert("Clicked " + ref.current + " times!");
  }

  function handleStateClick() {
    setCount(count + 1);
  }

  return (
    <>
      <button type="button" onClick={handleRefClick}>
        Ref: Clicked {ref.current} times
      </button>
      <button type="button" onClick={handleStateClick}>
        State: Clicked {count} times
      </button>
    </>
  );
}
```

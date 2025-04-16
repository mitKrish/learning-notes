## useRef and useState

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

## Passing data between Parent and Child components

```javascript
import { useState } from "react";

export default function App() {
  const [msg, setMsg] = useState("");

  function handleMsgUpdate(newMsg) {
    setMsg(newMsg);
  }

  return (
    <>
      <p>{msg}</p>
      <Child props={msg} onUpdate={handleMsgUpdate} />
    </>
  );
}

function Child(props) {
  const [childMsg, setChildMsg] = useState(props.msg);

  function handleChildMsgUpdate(event) {
    setChildMsg(event.target.value);
  }

  function handleClick() {
    props.onUpdate(childMsg);
  }

  return (
    <div>
      <p>Message from Parent: {props.msg}</p>
      <input
        type="text"
        value={childMsg}
        onChange={handleChildMsgUpdate}
      ></input>
      <button type="button" onClick={handleClick}>
        Update
      </button>
    </div>
  );
}
```

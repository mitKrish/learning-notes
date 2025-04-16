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
      <p>Parent Component: {msg}</p>
      <Child msg={msg} onUpdate={handleMsgUpdate} />
    </>
  );
}

function Child(props) {
  const [childMsg, setChildMsg] = useState(props.msg);

  console.log("props", props, props.msg);
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

![image](https://github.com/user-attachments/assets/59f1e66c-e20b-4dfb-9e79-d5a6d1e25533)


## Context

```javascript
import React, { createContext, useState, useContext } from 'react';

// 1. Create the Context
const ThemeContext = createContext();

// 2. Create the Theme Provider Component
export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light'); // Initial theme is light

  // Define the theme styles
  const themes = {
    light: {
      backgroundColor: 'white',
      color: 'black',
    },
    dark: {
      backgroundColor: 'black',
      color: 'white',
    },
  };

  // Function to toggle the theme
  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, themes, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// 3. Create a Custom Hook to Use the Context
export const useTheme = () => {
  return useContext(ThemeContext);
};

// Example Usage in a Component
const MyThemedComponent = () => {
  const { theme, themes } = useTheme();
  const currentTheme = themes[theme];

  return (
    <div style={{ backgroundColor: currentTheme.backgroundColor, color: currentTheme.color, padding: '20px' }}>
      This text is themed!
    </div>
  );
};

// Example Usage of the Toggle Button
const ThemeToggleButton = () => {
  const { toggleTheme, theme } = useTheme();

  return (
    <button onClick={toggleTheme}>
      Switch to {theme === 'light' ? 'Dark' : 'Light'} Theme
    </button>
  );
};

// Example of how to use the Provider in your App
function App() {
  return (
    <ThemeProvider>
      <div>
        <h1>Simple Theme Example</h1>
        <MyThemedComponent />
        <ThemeToggleButton />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

## useReducer and useState
```javascript
import React, { useState, useReducer } from 'react';

// Example 1: useState
const UseStateCounter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h2>useState Counter</h2>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
      <hr />
    </div>
  );
};

// Example 2: useReducer
// Define the reducer function
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

// Define the initial state
const initialState = { count: 0 };

const UseReducerCounter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  const increment = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const decrement = () => {
    dispatch({ type: 'DECREMENT' });
  };

  return (
    <div>
      <h2>useReducer Counter</h2>
      <p>Count: {state.count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};

const App = () => {
  return (
    <div>
      <UseStateCounter />
      <UseReducerCounter />
    </div>
  );
};

export default App;
```

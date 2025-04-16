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
import { useContext, createContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const themes = {
    light: {
      "background-color": "white",
      color: "black",
    },
    dark: {
      "background-color": "black",
      color: "white",
    },
  };

  function toggleTheme() {
    setTheme((prevTheme) => (prevTheme === "light" ? "dark" : "light"));
  }

  return (
    <ThemeContext.Provider value={{ theme, themes, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ChildComponent() {
  const { theme, themes, toggleTheme } = useContext(ThemeContext);
  const currentTheme = themes[theme];

  return (
    <>
      <div
        style={{
          "background-color": currentTheme["background-color"],
          color: currentTheme.color,
          padding: "20px",
        }}
      >
        Hello World !
      </div>
      <button type="button" onClick={toggleTheme}>
        Switch to {theme === "light" ? "dark" : "light"}
      </button>
    </>
  );
}

export default function App() {
  return (
    <ThemeProvider>
      <ChildComponent />
    </ThemeProvider>
  );
}
```

## useReducer and useState
```javascript
import { useReducer } from "react";

export default function App() {
  const initialState = { count: 0 };

  function counterReducer(state, action) {
    switch (action.type) {
      case "INCREMENT":
        return { count: state.count + 1 };
      case "DECREMENT":
        return { count: state.count - 1 };
      default:
        return state;
    }
  }

  const [state, dispatch] = useReducer(counterReducer, initialState);

  function increment() {
    dispatch({ type: "INCREMENT" });
  }

  function decrement() {
    dispatch({ type: "DECREMENT" });
  }

  return (
    <>
      <p>Count : {state.count}</p>
      <button type="button" onClick={increment}>
        increment
      </button>
      <button type="button" onClick={decrement}>
        decrement
      </button>
    </>
  );
}
```

## useEffect
```javascript
import React, { useState, useEffect } from 'react';

function UserInfo() {
  const [userData, setUserData] = useState(null);
  const apiUrl = 'https://jsonplaceholder.typicode.com/users/1'; // Simple public API for a user

  useEffect(() => {
    const fetchUserData = async () => {
      try {
        const response = await fetch(apiUrl);
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data = await response.json();
        setUserData(data);
      } catch (error) {
        console.error('Error fetching user data:', error);
        // You might want to set an error state here to display an error message
      }
    };

    fetchUserData();
  }, [apiUrl]); // Effect runs once after initial render because dependency array is fixed

  if (!userData) {
    return <div>Loading user information...</div>;
  }

  return (
    <div>
      <h2>User Information</h2>
      <p>User ID: {userData.id}</p>
      <p>Name: {userData.name}</p>
      {/* You can display other user details here if needed */}
    </div>
  );
}

export default UserInfo;
```

## useEffect

```javascript
import { useEffect, useState } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const getUserData = async () => {
      try {
        setLoading(true);
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users/${userId}`
        );
        if (!response.ok)
          throw new Error(`HTTP error! status: ${response.status}`);
        const data = await response.json();
        setUser(data);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };
    getUserData();
  }, [userId]);

  if (loading) return <p>Loading User Data</p>;
  if (error) return <p>Error getting user data</p>;
  if (!user) return <p>No user data found.</p>;

  return (
    <>
      <p>User Data</p>
      <p>Name:{user.name}</p>
      <p>Email:{user.email}</p>
    </>
  );
}

export default function App() {
  return <UserProfile userId={1} />;
}
```

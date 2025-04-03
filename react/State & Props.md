State and props are fundamental concepts in React that manage data and control how components render. 

**Props (Properties)**

* **Purpose:** Props are used to pass data from a *parent* component to a *child* component. 
* **Data Flow:** Props are *read-only* within the child component.  A child component cannot directly modify the props it receives.  The parent component is responsible for changing the prop values.
* **Syntax:** Props are passed as attributes when you render a component:

```javascript
// Parent component
<MyComponent name="Alice" age={30} />

// Child component (functional component)
function MyComponent(props) {
  return (
    <div>
      <p>Name: {props.name}</p>
      <p>Age: {props.age}</p>
    </div>
  );
}

// Child component (class component)
class MyComponent extends React.Component {
  render() {
    return (
      <div>
        <p>Name: {this.props.name}</p>
        <p>Age: {this.props.age}</p>
      </div>
    );
  }
}
```

* **Example:** Imagine a `Profile` component. The parent component might pass the user's name and age as props: `<Profile name="Bob" age={25} />`.

**State**

* **Purpose:** State is used to manage *internal* data within a component. It's data that can change over time and affect how the component renders.  State is what makes components dynamic.
* **Data Flow:** State is *mutable* (changeable) within the component that owns it.  Changes to state trigger a re-render of the component and its children.
* **Syntax:**  State is typically initialized within the constructor (for class components) or using the `useState` hook (for functional components).

```javascript
// Class component
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  handleClick = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.handleClick}>Increment</button>
      </div>
    );
  }
}

// Functional component (using useState hook)
import React, { useState } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
}
```

* **Example:** A counter component might use state to track the current count.  When the button is clicked, the state is updated, and the component re-renders to display the new count.

**Key Differences Summarized**

| Feature | Props | State |
|---|---|---|
| **Data Source** | Passed from parent component | Managed within the component |
| **Mutability** | Read-only (immutable) | Mutable (can be changed) |
| **Purpose** | Configure and pass data to child components | Manage internal data and trigger re-renders |
| **Scope** | External to the component (passed in) | Internal to the component |

**When to Use Props vs. State**

* **Props:** Use props when you want to pass data down the component tree and configure a component's behavior.
* **State:** Use state when you need to manage data that changes within a component, often due to user interactions or asynchronous operations.

**Relationship between Props and State**

Props can be derived from state.  A parent component can have state, and then pass parts of that state as props to its children.  This allows the parent to control the data that the children receive.

**Example: Combining Props and State**

```javascript
// Parent component
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { username: 'Alice' };
  }

  render() {
    return (
      <div>
        <Profile username={this.state.username} />
      </div>
    );
  }
}

// Child component
function Profile(props) {
  return (
    <p>Hello, {props.username}!</p>
  );
}
```

In this example, the `App` component has state (`username`). It then passes this state as a prop to the `Profile` component.  If the `App` component's state changes, the `Profile` component will automatically re-render with the new username.

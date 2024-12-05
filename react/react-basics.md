# React
- React is a  an open-source, component-based, front-end JavaScript library.
- created for building fast and interactive user interfaces for web and mobile applications.
- responsible only for the application view layer.

## how React works?
- Virtual DOM is a DOM tree representation in Javascript. React elements are plain objects and are cheap to create.
- Virtual DOM will try to find the most efficient way to update the browsers DOM to match the React elements.

## Advantages
- Increases the application's performance with Virtual DOM.
- JSX makes code easy to read and write.
- It renders both on client and server side (SSR).
- Easy to integrate with frameworks (Angular, Backbone) since it is only a view library.
- Easy to write unit and integration tests with tools such as Jest.

## Limitations
- React is just a view library, not a full framework.
- There is a learning curve for beginners who are new to web development.
- Integrating React into a traditional MVC framework requires some additional configuration.
- The code complexity increases with inline templating and JSX.
- Too many smaller components leading to over engineering or boilerplate.

## DOM
- __Real DOM__: The Real DOM (Document Object Model) is the standard way browsers represent the structure of a web page. It creates a tree-like structure where each node represents an element, attribute, or piece of text in the HTML document. Changes are slow because the entire DOM is re-rendered.
- __Virtual DOM__: An in-memory representation of the Real DOM used by libraries like React to optimize updates. It is a lightweight copy of the Real DOM that exists in memory. When changes are made to the application state, the Virtual DOM updates first. React then compares the updated Virtual DOM with the previous version to identify the differences (a process called "diffing"). Only the necessary changes are then applied to the Real DOM, making updates more efficient.
- __Shadow DOM__: A way to encapsulate a part of the DOM to create isolated and reusable components, preventing style and structure conflicts. Eg: A custom video player might use the Shadow DOM to encapsulate its controls,

## JSX

JSX ( **JavaScript Expression/ JavaScript XML** ) allows us to write HTML elements in JavaScript and place them in the DOM without any `createElement()` or `appendChild()` methods. JSX converts HTML tags into react elements. React uses JSX for templating instead of regular JavaScript.

* It is faster because it performs optimization while compiling code to JavaScript.
* It is also type-safe and most of the errors can be caught during compilation.
* It makes it easier and faster to write templates.

When JSX compiled, they actually become regular JavaScript objects.

```js
const hello = <h1 className = "greet"> Hello World </h1>
```

will be compiled to

```js
const hello = React.createElement {
    type: "h1",
    props: {
      className: "greet",  
      children: "Hello World"
    }
}
```

# JSX in React

**JSX** (JavaScript XML) is a syntax extension for JavaScript, primarily used with React to describe what the UI should look like. It allows you to write HTML-like syntax directly within JavaScript, making it easier to create and manage React components.

## Key Features of JSX

1. **HTML-like Syntax**:
   JSX allows you to write HTML tags within JavaScript. For example:
```
const element = Hello, world!
```
   This syntax is neither a string nor HTML but a syntax extension that gets compiled to JavaScript.

2. **Embedding Expressions**:
   You can embed JavaScript expressions within JSX by using curly braces `{}`. For example:
 ```
const name = 'John';
const element = <h1>Hello, {name}!</h1>;
```

   Here, `{name}` is a JavaScript expression that gets evaluated and inserted into the resulting HTML.

3. **Attributes**:
   JSX allows you to use attributes similar to HTML. However, some attributes have different names to avoid conflicts with JavaScript keywords. For example, `class` becomes `className`:
``` const element = <div className="container">Content</div>; ``` 

4. **Children**:
   JSX can contain child elements, which can be nested:
 ```
  const element = (
  <div>
    <h1>Hello, world!</h1>
    <p>This is a paragraph.</p>
  </div>
);
 ```

5. **Conditional Rendering**:
   You can use JavaScript conditional statements within JSX to render elements conditionally:
```
;const isLoggedIn = true;
const element = (
  <div>
    {isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign in.</h1>}
  </div>
);

```

6. **Loops**:
   You can use JavaScript loops to generate elements dynamically:
```
const items = ['Item 1', 'Item 2', 'Item 3'];
const list = (
  <ul>
    {items.map(item => <li key={item}>{item}</li>)}
  </ul>
);
```

## How JSX Works

JSX is not understood by browsers directly. It needs to be transformed into regular JavaScript using a compiler like Babel. For example, the JSX:
```
const element = Hello, world!
```

gets compiled to:
```
const element = React.createElement('h1', null, 'Hello, world!');
```
This `React.createElement` function call creates an object representing the element, which React uses to update the DOM efficiently.

## Benefits of Using JSX

- **Readability**: JSX makes it easier to understand the structure of your UI components.
- **Integration with JavaScript**: Since JSX is embedded within JavaScript, you can leverage the full power of JavaScript within your UI code.
- **Error Checking**: JSX provides better error messages and warnings, making debugging easier.

JSX is a powerful feature of React that simplifies the process of creating and managing UI components. It blends the familiarity of HTML with the flexibility of JavaScript, making it a popular choice for building modern web applications.


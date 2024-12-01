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
- __Virtual DOM__: An in-memory representation of the Real DOM used by libraries like React to optimize updates. Only the necessary changes are applied to the Real DOM.
- __Shadow DOM__: A way to encapsulate a part of the DOM to create isolated and reusable components, preventing style and structure conflicts.

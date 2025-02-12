# Functions
- Function is a sequence of statements. Values can be passed as parameters, and the function will return a value.
```
function display(name) {
  return "Hello " + name;
}
display("World"); // "Hello World"
```

## Methods
- Object properties that are functions.
```
var obj = {
  display : function() {
    return "Hello " + this.name;
  },
  name: 'Minali Peri'
}
obj.display();  // "Hello Minali Peri"
```

## Constructors: 
- Constructors are defined with function. Constructor function is to initialize the object.
- Constructor creates a new object and passes it as the value of this, and implicitly returns the new object as its result.

```
function Employee(name, age) {
  this.name = name;
  this.age = age;
}
var emp1 = new Employee('Drishya Sama', 28);
emp1.name; // "Drishya Sama"
emp1.age; // 28
```

## this

- The this keyword refers to the object that is currently executing the function, providing context within which the function is called.
- The value of this depends on in which context it appears: function, class, or global.

```
console.log("this", this); // Window object in browser
```

```
let obj = {
    name : "Alice",
    city: "Connecticut",
    getThis: function(){
        return this;
    }
}

console.log(obj.getThis());
// { name: 'Alice', city: 'Connecticut', getThis: [Function: getThis] }
```

```
function getThisRef(){
    return this;
}

let obj1 = {name: "Krishna", demo: getThisRef}
let obj2 = {name: "Vishnu", demo: getThisRef}

obj1.demo(); // { name: 'Krishna', demo: [Function: getThisRef] }
obj2.demo(); // { name: 'Vishnu', demo: [Function: getThisRef] }
```

- When you declare an object method using the arrow function, the this keyword refers to the global object
- When you declare an object method using the regular function, this keyword refers to the object from which you call the function.

```
let obj = {
    name : "Alice",
    regular: function(){
        console.log(this);
    },
    arrow: ()=>{
        console.log(this);
    }
}

obj.regular();
obj.arrow();
```

- In a regular function, the this keyword refers to the object from which you call the function.
- In an arrow function, the this keyword refers to the object from which you define the function.

```
let obj = {
    name : "Alice",
    skills:["HTML"],
    regular(){
        this.skills.forEach(
        function (skill) {
        console.log(`${this.name} knows ${skill}`); // undefined knows HTML 
        });
    },
    arrow(){
        this.skills.forEach((skill)=>{
        console.log(`${this.name} knows ${skill}`); // Alice knows HTML
        });
    }
}

obj.regular();
obj.arrow();
```

In JavaScript, this is a keyword that refers to the current execution context of a function and its value depends on how the function is called.

#### 1. Global Scope 
When used outside any function or object, this refers to the global object: 
* In browsers, it's the window object. 
* In Node.js, it's the global object. 
```
console.log(this); // window (in browsers)
console.log(this); // global (in Node.js)
```

#### 2. Function Context 

* Regular function call: In non-strict mode, this refers to the global object. In strict mode, it's undefined. 
```
function myFunction() {
  console.log(this === window); // true (non-strict mode)
  console.log(this === undefined); // true (strict mode)
}
myFunction();
```
* Method call: When a function is called as a method of an object, this refers to the object that called the method. 
```
const obj = {
    name:"krishna",
    getName:function(){
        console.log(this.name);  
    },
    getArrowName : ()=>{
        console.log(this.name);
    }
}

obj.getName(); // "Krishna" this refers to obj which called the method.
obj.getArrowName(); //undefined this refers to global object.
```
#### 3. Constructor Function 
When a function is used as a constructor with the new keyword, this refers to the newly created object instance. 
```
function MyConstructor() {
  this.myProperty = 'example';
  console.log(this); // refers to the new object instance
}
const instance = new MyConstructor();
console.log(instance.myProperty); // 'example'
```

#### 4. Event Handlers 
In event handlers, this refers to the HTML element that triggered the event. 
```
<button id="myButton">Click me</button>
document.getElementById('myButton').addEventListener('click', function() {
  console.log(this); // refers to the button element
});
```

#### 5. call, apply, and bind 
These methods allow explicitly setting the value of this: 
* call() and apply() invoke the function immediately, with call taking arguments individually and apply as an array.
* bind() returns a new function with this bound to the specified value, without immediately calling the function. 
```
function sample(a,b){
    console.log(this, a,b);
}

let obj = {z:100};

sample.call(obj, 10,20);    // { z: 100 } 10 20
sample.apply(obj,[10,20]);  // { z: 100 } 10 20

const bindFunc = sample.bind(obj, 10,20);
bindFunc();                 // { z: 100 } 10 20

```

Links: 
* https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3

## functions and arrow functions

| Feature           | Traditional Function                                  | Arrow Function                                      |
|-------------------|------------------------------------------------------|------------------------------------------------------|
| Syntax            | `function functionName(params) { ... }`               | `const functionName = (params) => { ... };`           |
| `this` Binding    | Dynamic, depends on how the function is called.       | Lexical, inherits `this` from surrounding scope.       |
| `arguments` Object | Has `arguments` object (array-like).                 | Does not have `arguments` object. Use rest parameters. |
| Constructor       | Can be used as a constructor with `new`.            | Cannot be used as a constructor.                   |
| Hoisting          | Function declarations are hoisted.                      | Function expressions (including arrow functions) are not hoisted. |
| Return Value      | Explicit `return` statement or implicit return (single-expression functions). | Explicit `return` or implicit return (single-expression functions). |
| Scope             | Creates its own scope for variables declared with `var`. | Inherits scope from the surrounding context. Block-scoped variables (`let`, `const`) behave the same way in both. |
| Use Cases         | Object methods, event handlers (where `this` is dynamic), constructor functions, functions needing `arguments`. | Short, concise functions, callbacks, event handlers where lexical `this` is desired. |


## Callback 
A callback is a function passed into another function as an argument and is called after some operation has been completed.

* **What they are:**  A callback is a function that is passed as an argument to another function and is executed *after* the first function completes its task.  They are the traditional way to handle asynchronous operations in JavaScript.

* **Why they're used:**  JavaScript is single-threaded and non-blocking.  This means that long-running operations (like network requests or file I/O) shouldn't halt the execution of the rest of your code. Callbacks allow you to start an operation and then specify what should happen once it's finished, without blocking the main thread.

```
callback(error, values)
	if error, first parameter - error object
	else first parameter - null & rest - return values.
```

```
const userData = { id: 1, name: "John Doe" };

function getUserData(callback) {
  callback(userData);
}

function printUserData(data){
  console.log("User data:", data);  
}

getUserData(printUserData);
```

* **Callback Hell (Pyramid of Doom):**  A major drawback of callbacks is that nested asynchronous operations can lead to complex and hard-to-read code, often called "callback hell" or the "pyramid of doom."

```javascript
// Example of nested callbacks (callback hell)
getData1((data1) => {
  getData2(data1, (data2) => {
    getData3(data2, (data3) => {
      // ... more nested callbacks ...
    });
  });
});
```

## Promises

* **What they are:** A promise is basically an advancement of callbacks in Node.Promises are a cleaner way to handle asynchronous operations. **A Promise represents the eventual result of an asynchronous operation.** It can be in one of three states:
   
* **Why they're used:** Promises make asynchronous code more readable and manageable, avoiding the problems of callback hell.  They provide a more structured way to handle asynchronous results and errors.

Promises can be chained and provides easier error handling. So, No Callback Hell.

Promise States:
* *Pending:* The initial state, before the operation completes.
* *Fulfilled (Resolved):* The operation completed successfully.
* *Rejected:* The operation failed.

Advantages:
- Chaining: Promises can be chained, improving code readability.
- Error Handling: Promises provide a catch method to handle errors

```
const userData = { id: 1, name: "John Doe" };

function getUserData(userId) {
  return new Promise((resolve, reject) => {
    if (userId === userData.id) {
      resolve(userData);
    } else {
      reject("Invalid User Id");
    }
  });
}

getUserData()
  .then((data) => {
    console.log("User data:", data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

## Async-Await
*  `async` and `await` make asynchronous code look and behave a bit more like synchronous code, making it even easier to read and understand.
*   `async` is used to declare a function as asynchronous.
*   `await` is used inside an `async` function to pause execution until a Promise resolves (or rejects).
*   Async/await simplifies asynchronous code even further than Promises, making it more concise and easier to follow the control flow.
*   It's built on top of Promises.

Advantages:
 - Readability: Makes asynchronous code look like synchronous code, enhancing readability.
 - Error Handling: Uses try/catch blocks for error handling

```
const userData = { id: 1, name: "John Doe" };

async function getUserData(userId) {
  if (userId === userData.id) {
    return new Promise((resolve) => {
      resolve(userData);
    });
  } else {
    return new Error("Invalid User ID");
  }
}

async function processUserData() {
  try {
    const data = await getUserData(0);
    console.log("User data:", data);
  } catch (error) {
    console.error("Error:", error);
  }
}

processUserData();
```

**Key Differences Summarized:**

| Feature         | Callbacks                               | Promises                                   | Async/Await                             |
|-----------------|-------------------------------------------|-------------------------------------------|------------------------------------------|
| Syntax          | Nested functions                        | `.then()`, `.catch()`                      | `async`, `await`                         |
| Readability     | Can become complex (callback hell)        | Improved readability                      | Very readable, synchronous-like syntax    |
| Error Handling  | Can be cumbersome                        | Easier with `.catch()`                     | Easier with `try...catch`                 |
| Chaining        | Difficult                                | Easy                                       | Easy                                      |
| Under the Hood | Basic asynchronous mechanism            | Built on callbacks, more structured       | Built on Promises, syntactic sugar       |

**Advantages & Disadvantages:**

| Feature       | Callbacks                                      | Promises                                       | Async/Await                                    |
|---------------|------------------------------------------------|------------------------------------------------|------------------------------------------------|
| **Advantages**|  Simple to use for small tasks                |  Avoids callback hell                         |  Syntactic sugar over promises                |
|               |  Widely supported                             |  Better error handling with `.catch()`        |  Code looks synchronous, easier to read       |
|               |  No need for additional libraries             |  Chaining for sequential async operations     |  Easier to debug with modern tools            |
|               |                                                |  More control over async flow                |  Reduces boilerplate code                     |
| **Disadvantages**|  Can lead to callback hell (nested callbacks)| - Can be verbose with `.then()` and `.catch()` |  Requires modern JavaScript (ES8)             |
|               |  Harder to read and maintain                  |  Still requires chaining for sequential tasks |  Needs `try/catch` for error handling         |
|               |  Error handling can be cumbersome             |  Slightly more complex than callbacks         |  Not supported in older environments          |

### Summary
- **Callbacks** are simple but can become unwieldy with complex asynchronous operations.
- **Promises** provide a cleaner way to handle asynchronous operations and avoid callback hell.
- **Async/Await** makes asynchronous code look synchronous, improving readability and maintainability.

## Promise Methods:

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const data = { name: "John Doe", age: 30 };
      if (data) {
        resolve(data);
      } else {
        reject("Failed to fetch data");
      }
    }, 500);
  });
}

fetchData()
  .then((data) => {
    console.log("Data received:", data);
    return Promise.resolve(data.age); // Chaining a resolved promise
  })
  .then((age) => {
    console.log("Age:", age);
  })
  .catch((error) => {
    console.error("Error:", error);
  })
  .finally(() => {
    console.log("Fetch operation complete.");
  });
```


1.  **`new Promise()` Constructor:**

    It takes a function called the "executor" as an argument. The executor function receives two arguments: `resolve` and `reject`.
    
2.  **`.then()` Method:**

    The `.then()` method is used to handle the fulfilled state of a Promise. It takes up to two arguments:

    *   A function to handle the resolved value.
    *   (Optional) A function to handle the rejected value (you can also use `.catch()` for this).

3.  **`.catch()` Method:**

    The `.catch()` method is specifically designed to handle the rejected state of a Promise. 

4.  **`.finally()` Method:**

    The `.finally()` method is executed regardless of whether the Promise is fulfilled or rejected.  It's useful for cleanup tasks (e.g., closing connections, stopping loaders).

5.  **`Promise.all()` Method:**

    * `Promise.all()` takes an array of Promises and returns a single Promise that resolves when *all* of the input Promises resolve.
    *  If any of the Promises reject, the resulting Promise immediately rejects with the reason of the first rejected Promise.

    ```javascript
    const promise1 = Promise.resolve(1);
    const promise2 = new Promise((resolve) => setTimeout(resolve, 100, "foo"));
    const promise3 = Promise.resolve(true);

    Promise.all([promise1, promise2, promise3])
      .then((values) => {
        console.log(values); // [1, "foo", true]
      })
      .catch((error) => {
        console.error("A promise rejected:", error);
      });
    ```
6. **`Promise.allSettled()` Method:**
 * Always resolves with an array of objects describing the outcome of each promise, regardless of whether they resolved or rejected.
	```javascript
	const promise1 = Promise.resolve(3);
	const promise2 = Promise.reject('error');
	const promise3 = new Promise((resolve, reject) => {
	  setTimeout(resolve, 100, 'foo');
	});
	
	Promise.allSettled([promise1, promise2, promise3]).then(results => {
	 Each((result) => {
	    if (result.status === 'fulfilled') {
	      console.log('Fulfilled:', result.value);
	    } else {
	      console.log('Rejected:', result.reason);
	    }
	  });
	});
	```

* Use Promise.all when you need all promises to succeed.
* Use Promise.allSettled when you need to handle each promise's outcome individually.

7.  **`Promise.race()` Method:**

    `Promise.race()` takes an array of Promises and returns a single Promise that resolves or rejects as soon as *one* of the input Promises resolves or rejects.  

    ```javascript
    const promiseA = new Promise((resolve) => setTimeout(resolve, 500, "A"));
    const promiseB = new Promise((resolve) => setTimeout(resolve, 100, "B"));

    Promise.race([promiseA, promiseB])
      .then((value) => {
        console.log(value); // "B" (because promiseB resolves first)
      });
    ```

9.  **`Promise.resolve()` Method:**
    * `Promise.resolve(value)` creates a Promise that is already resolved with the given `value`.
    * Shorthand for `new Promise(resolve => resolve(value))`.
    ```javascript
    const resolvedPromise = Promise.resolve(42);
    resolvedPromise.then(value => console.log(value)); // 42
    ```

10.  **`Promise.reject()` Method:**
    * `Promise.reject(reason)` creates a Promise that is already rejected with the given `reason`.
    * Shorthand for `new Promise((resolve, reject) => reject(reason))`.

    ```javascript
    const rejectedPromise = Promise.reject("Something went wrong");
    rejectedPromise.catch(reason => console.error(reason)); // "Something went wrong"
    ```

## Closure
A closure is a feature in JavaScript where an inner function has access to the outer (enclosing) function’s variables. In JavaScript, closures are created every time a function is created, at function creation time.

- Variables declared in the outer function’s scope.
- Parameters of the outer function.
- Variables declared in the global scope.

```
function outer(){
    let x =5;
    
    function inner(){
        console.log(x);
    }
    inner();
}

outer();
```

outerFunction creates a variable outerVariable.
innerFunction is defined inside outerFunction and accesses outerVariable.
Even after outerFunction has finished executing, innerFunction retains access to outerVariable through the closure.


### Practical Uses of Closures
#### Data Privacy:
Closures can be used to create private variables that cannot be accessed from outside the function.

```
function createCounter() {
  let count = 0;

  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

#### Function Factories:

Closures can be used to create functions with preset arguments.
```
function createAdder(x) {
  return function(y) {
    return x + y;
  };
}

const addFive = createAdder(5);
console.log(addFive(2)); // 7
console.log(addFive(10)); // 15
```
Here, createAdder returns a function that adds a specific value to its argument. The returned function remembers the value of x through the closure.

Key Points to Remember
- Closures are created every time a function is created.
- They allow inner functions to access variables from their outer scope.
- They are useful for data privacy and creating function factories.

## Closure
A closure is a function that has access to variables from its surrounding (lexical) scope, even after the outer function has returned. 

Key Concepts:
 * Lexical Scoping: JavaScript uses lexical (or static) scoping.  This means the scope of a variable is determined by its position in the source code, specifically by where the function is defined, not where it's called.
 * Inner Function Access: Inner functions have access to variables from their own scope, as well as the scope of their containing (outer) functions.
 * Persistence: Even after the outer function has finished executing, the inner function retains access to those variables from the outer scope.  These variables are kept alive in memory by the closure.

Examples:
### 1. Counter:
```
function createCounter() {
  let count = 0; 

  return function() {
    count++;
    return count;
  };
}

const counter = createCounter(); // counter holds the inner function

console.log(counter()); // Output: 1 
console.log(counter()); // Output: 2
console.log(counter()); // Output: 3

// assign outer funtion to variable & call that as function call.
```
In this example, createCounter returns the inner function.  That inner function forms a closure. It "remembers" the count variable, even after createCounter has returned. Each call to counter() increments and returns the updated count.

### 2. Encapsulation (Data Hiding):
```
function createPerson(name) {
  let age = 0; // Private variable (accessible only within createPerson)

  return {
    getName: function() {
      return name;
    },
    getAge: function() {
      return age;
    },
    setAge: function(newAge) {
      if (newAge >= 0) {
        age = newAge;
      }
    }
  };
}

const person = createPerson("Alice");

console.log(person.getName()); // Output: Alice
console.log(person.getAge());   // Output: 0

person.setAge(30);
console.log(person.getAge());   // Output: 30

// We cannot directly access person.age from outside. This is encapsulation.
// person.age = 100;  // This would not work as intended (or might even cause an error in strict mode)
```

Here, the age variable is kept private within createPerson. The returned object (containing the getName, getAge, and setAge functions) forms a closure.  It provides controlled access to the age variable, demonstrating encapsulation.

### 3.  Looping and Closures (Common Pitfall):
```
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Output: 5 5 5 5 5 (Not what you might expect!)
```
Because var has function scope (or global scope if declared outside a function), the i in the setTimeout callback refers to the same i variable that's being updated in the loop. By the time the setTimeout callbacks run, the loop has already completed, and i is 5.

Solution using IIFE (Immediately Invoked Function Expression) or let:
```
// Using IIFE:
for (var i = 0; i < 5; i++) {
  (function(currentI) { // Create a new scope for each iteration
    setTimeout(function() {
      console.log(currentI);
    }, 1000);
  })(i); // Pass the current value of i
}
// Output: 0 1 2 3 4
```
```
// Using let (block scope):
for (let i = 0; i < 5; i++) { // let has block scope
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Output: 0 1 2 3 4
```
let creates a new binding for i in each iteration of the loop, so each setTimeout callback has its own i value.  The IIFE achieves a similar result by creating a new function scope for each iteration.

### Summary:
Closures allow inner functions to retain access to variables from their outer scope. They are essential for data encapsulation, creating private variables, and handling asynchronous operations correctly. 

Closures are a powerful feature in JavaScript that allows inner functions to access variables from their outer (enclosing) functions, even after the outer function has returned.

### Advantages:
* ##### Data Encapsulation and Privacy:
  Closures enable you to create private variables and methods within a function. This helps in data hiding and prevents external access to sensitive data, promoting better code organization and security. 
* ##### State Maintenance:
  Closures can be used to maintain state across function calls. The inner function retains access to the variables of the outer function, allowing it to remember and update values between invocations. 
* ##### Partial Application and Currying:
 Closures facilitate partial application and currying techniques, where you can create new functions by pre-filling some arguments of an existing function. This enhances code reusability and flexibility. 
* ##### Module Pattern:
   Closures are fundamental to the module pattern, a common design pattern for creating self-contained and reusable code modules. Modules can encapsulate private data and expose public methods through closures. 
* #### Event Handling and Callbacks:
   Closures are often used in event handling and callbacks to maintain access to relevant variables from the outer scope when the event or callback is triggered.
### Disadvantages:
* #### Memory Consumption:
  Closures can lead to increased memory consumption because they retain references to variables in their outer scope, even if those variables are no longer needed elsewhere. This can potentially cause memory leaks if not managed carefully.
* #### Performance Overhead:
   The process of creating and maintaining closures can introduce some performance overhead, as the JavaScript engine needs to manage the scope chain and variable access. However, this overhead is usually negligible in most cases. 
* #### Debugging Complexity:
  Closures can make debugging more challenging due to the complex scope chain and the interaction between inner and outer functions. Tracing the origin of variables and understanding their values might require careful inspection.
* #### Potential for Unexpected Behavior:
   If not used carefully, closures can sometimes lead to unexpected behavior, especially when dealing with asynchronous operations or shared variables. It's crucial to understand how closures interact with the event loop and variable scope to avoid potential pitfalls.

#### Summary:
* Closures are a valuable tool in JavaScript, offering powerful capabilities for data encapsulation, state management, and code organization.
* However, it's important to be aware of the potential drawbacks related to memory consumption, performance, and debugging complexity. 



** call, apply, and bind **

## call()
 * Purpose: Invokes a function directly, explicitly setting the this value and providing arguments individually.
 * Syntax: function.call(thisArg, arg1, arg2, ...)
```
const person = { name: "Alice" };
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}
greet.call(person, "Hello", "!"); // Output: Hello, Alice!
```
#### Advantages: 
 * Clarity: Arguments are passed individually, making the code readable, especially when dealing with a small, fixed number of arguments.
 * Flexibility: Allows for direct control over the this context.
#### Disadvantages:
 * Cumbersome for many arguments: Passing a large number of arguments individually can become tedious and less maintainable.
## apply()
* Purpose: Invokes a function directly, explicitly setting the this value and providing arguments as an array.\
* Syntax: function.apply(thisArg, [argsArray])
```
const person = { name: "Bob" };
const args = ["Greetings", "!!"];
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}
greet.apply(person, args); // Output: Greetings, Bob!!
```
* #### Advantages:
  * Handles variable number of arguments: Ideal when you don't know the number of arguments beforehand or when you have them stored in an array. This is very useful for functions that accept a variable number of parameters.
  * Useful with array-like objects: You can apply methods to array-like objects (e.g., arguments object, DOM NodeList).
* #### Disadvantages:
  * Less readable for fixed arguments: For a small, fixed number of arguments, call() might be more readable.
  
## bind()
* Purpose: Creates a new function where the this value is permanently bound to the provided value. It does not immediately execute the function.
* Syntax: function.bind(thisArg, arg1, arg2, ...)

```
const person = { name: "Charlie" };
function greet(greeting, punctuation) { 
 console.log(`${greeting}, ${this.name}${punctuation}`);
}
// Creates a new functiongreetCharlie("?");
const greetCharlie = greet.bind(person, "Hi");  // Output: Hi, Charlie? (The "Hi" is pre-filled)
const greetCharlie2 = greet.bind(person);
greetCharlie2("Hello", "!!!"); // Output: Hello, Charlie!!!
```
* #### Advantages:
  * Event Handling: Crucial for event handlers where you need to maintain the correct this context (e.g., within a setTimeout or event listener).
  * Partial Application/Currying: Allows you to create new functions by pre-filling some arguments. This is a powerful functional programming technique.
  * Creating Bound Functions: Useful when you want to pass a method as a callback, ensuring the correct this context.
* #### Disadvantages:
  * Doesn't execute immediately: It creates a new function, which might be a slight overhead if you need to execute it right away (use call or apply in that case).
  * Slightly more complex: The concept of creating a new bound function might be a little harder to grasp initially compared to call and apply.
  
### Summary Table:
| Method | Invokes Function? | Arguments | this Binding | Use Cases |
|---|---|---|---|---|
| call() | Yes | Individually | Explicitly set | Direct invocation, fixed arguments |
| apply() | Yes | Array | Explicitly set | Variable arguments, array-like objects |
| bind() | No (creates new function) | Individually (for pre-filling) | Permanently bound | Event handling, partial application, callbacks |

### Which to use when? 
* call(): Use when you want to call a function immediately with a specific this value and a small, fixed number of arguments.
* apply(): Use when you want to call a function immediately with a specific this value and you have a variable number of arguments (or they're in an array).
* bind(): Use when you want to create a new function with a permanently bound this value, often for event handling, callbacks, or partial application.









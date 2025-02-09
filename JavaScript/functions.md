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

## Callback 
A callback is a function passed into another function as an argument and is called after some operation has been completed.

```
callback(error, values)
	if error, first parameter - error object
	else first parameter - null & rest - return values.
```

callback hell, a situation where callbacks are nested within callbacks, making the code difficult to read and maintain.

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
## Promises

A promise is basically an advancement of callbacks in Node. A Promise means an action will either be completed or rejected. 

Promises can be chained and provides easier error handling. So, No Callback Hell.

Promise States:
- pending: initial state, neither fulfilled nor rejected.
- fulfilled: operation completed successfully.
- rejected: operation failed.

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
- Async functions helps in writing promises looks like synchronous. It operates asynchronously via the event-loop. It automatically wraps it in a promise which is resolved with its value.
- Await: Await function makes the code wait until the promise returns a result. It only makes the async block wait.

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

## Closure?
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

## Scope
## 1. Lexical (Static) Scope:

 * Definition: Lexical scope (also called static scope) is determined by the position of a variable's declaration within the source code.  It's resolved at compile time (or when the code is parsed).  You can visualize it by looking at the nested structure of functions.
 * How it works:  When the JavaScript engine encounters a variable, it first looks for the variable in the current scope. If it doesn't find it, it moves outward to the enclosing (parent) scope, and then to the parent's parent scope, and so on, until it finds the variable or reaches the global scope.
 * Example:
```
function outerFunction() {
  let outerVar = "I'm from the outer function";

  function innerFunction() {
    let innerVar = "I'm from the inner function";
    console.log(innerVar); // Output: I'm from the inner function
    console.log(outerVar); // Output: I'm from the outer function (lexical scoping)
  }

  innerFunction();
  console.log(outerVar); // Output: I'm from the outer function
  // console.log(innerVar); // Error: innerVar is not defined in the outer function's scope
}

outerFunction();
```
In this example:
 * innerFunction has access to both innerVar (its own variable) and outerVar (from its parent scope, outerFunction). This is lexical scoping.
 * outerFunction has access to outerVar but not innerVar.  Scope lookup goes outward, not inward.
 * If you tried to access innerVar outside of innerFunction, you'd get an error.

### 2. Function Scope (Older JavaScript - with var):
 * Definition:  Variables declared with var have function scope.  They are accessible anywhere within the entire function, regardless of where they are declared inside that function.
 * Example:
```
function myFunction() {
  var x = 10;

  if (true) {
    var y = 20;
    console.log(x); // Output: 10
  }

  console.log(y); // Output: 20 (y is still accessible here because of function scope)
}

myFunction();
```
 * Important Note: Function scope with var can lead to unexpected behavior because variables can be accessed even before their declaration (due to hoisting).  It's generally recommended to use let and const (block scope) whenever possible to avoid these issues.

### 3. Block Scope (Introduced with let and const):
 * Definition: Variables declared with let and const have block scope. They are only accessible within the block of code (e.g., inside curly braces {}) where they are defined.
 * Example:
```
function myBlockScopeFunction() {
  let x = 10;

  if (true) {
    let y = 20;
    const z = 30; // const variables are also block-scoped
    console.log(x); // Output: 10
    console.log(y); // Output: 20
    console.log(z); // Output: 30
  }

  console.log(x); // Output: 10
  // console.log(y); // Error: y is not defined (block scope)
  // console.log(z); // Error: z is not defined (block scope)
}

myBlockScopeFunction();
```
 * Benefits of Block Scope: Block scope helps prevent accidental variable overwriting and makes code more predictable and easier to reason about.  It's a best practice to use let and const whenever you can.

### 4. Global Scope:
 * Definition: Variables declared outside of any function or block have global scope. They are accessible from anywhere in your code.
 * Example:
```
var globalVar = "I'm global"; // Using var (generally avoid)
let globalLet = "I'm also global (but better)"; // Using let (preferred)
const globalConst = "I'm a global constant";

function myFunction() {
  console.log(globalVar); // Output: I'm global
  console.log(globalLet); // Output: I'm also global (but better)
  console.log(globalConst); // Output: I'm a global constant
}

myFunction();
console.log(globalVar); // Output: I'm global
```
 * Caution:  Overuse of global variables can make your code harder to maintain and debug. It's best to minimize their use and keep variables within the smallest scope necessary.

Summary Table:
| Scope Type | Keyword(s) | Accessibility |
|---|---|---|
| Lexical (Static) | N/A | Determined by code structure (nested functions) |
| Function | var | Within the entire function |
| Block | let, const | Within the block (e.g., {}) where defined |
| Global | N/A (outside functions/blocks) | Anywhere in the code |











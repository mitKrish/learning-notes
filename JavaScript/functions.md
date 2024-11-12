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

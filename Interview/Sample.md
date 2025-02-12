Let's explore Promise methods in detail with examples.  We'll cover the core methods you'll use most often.
1. Promise.prototype.then(onFulfilled, onRejected)
 * Purpose:  The primary method for handling the eventual result of a Promise.  It returns a new Promise, allowing for chaining.
 * Parameters:
   * onFulfilled (Optional): A function to handle fulfillment (resolution).  Receives the resolved value as an argument.
   * onRejected (Optional): A function to handle rejection. Receives the rejection reason as an argument.
 * Return Value: A new Promise.  Its state depends on the return value of onFulfilled or onRejected:
   * If onFulfilled returns a value (not a Promise), the new Promise is fulfilled with that value.
   * If onFulfilled returns a Promise, the new Promise will adopt the state of that returned Promise (Promise chaining).
   * If onRejected returns a value, the new Promise is fulfilled with that value.
   * If onRejected returns a Promise, the new Promise will adopt the state of that returned Promise.
   * If neither onFulfilled nor onRejected is provided, the new Promise will resolve or reject with the same value as the original promise.
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(42);
  }, 500);
});

myPromise.then(value => {
  console.log("Resolved:", value); // Output: Resolved: 42
  return value * 2; // Returns a value, new promise will be resolved with 84
}).then(doubledValue => {
  console.log("Doubled:", doubledValue); // Output: Doubled: 84
  return Promise.resolve(doubledValue + 10); // Returns a Promise, chaining
}).then(finalValue => {
    console.log("Final Value:", finalValue); // Output: Final Value: 94
}).catch(reason => {
  console.error("Rejected:", reason); // Won't be called in this example
});



const failingPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("Something went wrong!");
    }, 500);
  });
  
  failingPromise.then(value => {
    console.log("Resolved:", value); // Not called
  }).catch(reason => {
    console.error("Rejected:", reason); // Output: Rejected: Something went wrong!
    return 100; // returns a value, the next .then will be resolved with 100
  }).then(handledValue => {
    console.log("Handled Value:", handledValue); // Output: Handled Value: 100
  });


2. Promise.prototype.catch(onRejected)
 * Purpose: A shorthand for promise.then(null, onRejected).  Specifically for handling rejections.
 * Parameter: onRejected: A function to handle rejections.
 * Return Value: A new Promise (just like .then()).
myPromise.then(value => { /* ... */ }).catch(error => { /* ... */ });

// Is equivalent to:

myPromise.then(value => { /* ... */ }, error => { /* ... */ });

3. Promise.prototype.finally(callback)
 * Purpose:  Executes a function regardless of whether the Promise is fulfilled or rejected.  Useful for cleanup tasks.
 * Parameter: callback: A function to be executed.  It receives no arguments.
 * Return Value: A new Promise.  If the original promise resolves or rejects, the finally promise will resolve or reject with the same value. If the callback throws an error, the finally promise will reject with that error.
myPromise.then(() => { /* ... */ }).catch(() => { /* ... */ }).finally(() => {
  console.log("Promise finished."); // Always called
});

// Example with cleanup:
let connection = null;

getConnection() // Returns a Promise for a database connection
.then(conn => {
    connection = conn;
    return connection.query("SELECT * FROM users");
})
.then(results => {
    // Process results
})
.catch(error => {
    console.error("Error:", error);
})
.finally(() => {
    if (connection) {
        connection.close(); // Close the connection, whether successful or not
        console.log("Connection closed.");
    }
});

4. Promise.resolve(value)
 * Purpose: Creates a new Promise that is already fulfilled with the given value.  Useful for returning a Promise when you already have the result synchronously.
 * Parameter: value: The value with which the Promise will be fulfilled.
 * Return Value: A new Promise in the fulfilled state.
Promise.resolve(10).then(value => console.log(value)); // Output: 10
Promise.resolve(Promise.resolve(20)).then(value => console.log(value)); // Output: 20 (flattens nested promises)

5. Promise.reject(reason)
 * Purpose: Creates a new Promise that is already rejected with the given reason. Useful for returning a Promise representing an error.
 * Parameter: reason: The reason for the rejection (usually an error object).
 * Return Value: A new Promise in the rejected state.
Promise.reject("Something went wrong").catch(reason => console.error(reason)); // Output: Something went wrong

6. Promise.all(promises)
 * Purpose: Takes an array of Promises and returns a single Promise that fulfills when all of the input Promises fulfill.  If any of the input Promises reject, the returned Promise immediately rejects with the reason of the first rejected Promise.
 * Parameter: promises: An array of Promises.
 * Return Value: A new Promise.
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.resolve(3);

Promise.all([promise1, promise2, promise3]).then(values => {
  console.log(values); // Output: [1, 2, 3]
}).catch(reason => { /* ... */ });

const promise4 = Promise.reject("Error!");
Promise.all([promise1, promise4, promise3]).catch(reason => {
    console.error(reason); // Output: Error!
});

7. Promise.race(promises)
 * Purpose: Takes an array of Promises and returns a single Promise that fulfills or rejects as soon as one of the input Promises fulfills or rejects.  It "races" the Promises.
 * Parameter: promises: An array of Promises.
 * Return Value: A new Promise.
const promise1 = new Promise(resolve => setTimeout(resolve, 500, 1));
const promise2 = new Promise(resolve => setTimeout(resolve, 100, 2)); // Wins the race

Promise.race([promise1, promise2]).then(value => {
  console.log(value); // Output: 2 (because promise2 resolved first)
});

These Promise methods provide the tools you need to effectively manage asynchronous operations in a clean, readable, and maintainable way.  Understanding them thoroughly is crucial for modern JavaScript development.

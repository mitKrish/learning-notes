
**What is the Event Loop (in Node.js)?**

Node.js is a single-threaded JavaScript runtime. The event loop allows Node.js to perform non-blocking I/O operations by offloading operations to the system kernel whenever possible.Modern kernels are multi-threaded and handle multiple operations executing in the background. When one of these operations completes, the kernel sends appropriate callback to the poll queue.

**Key Concepts from the Node.js Website:**
1.  **Single Thread:** Node.js runs on a single thread.  This means only one piece of code can be executed at a time.
2.  **Non-Blocking I/O:**  Node.js uses asynchronous I/O operations.  When an I/O operation is started, a callback function is registered, and the program continues. When the I/O completes, the callback is executed.
3.  **Event Loop:** The event loop continuously monitors the call stack and the callback queue.  When the call stack is empty, the event loop takes the first callback from the queue and puts it onto the call stack for execution.

**Phases of the Event Loop (Detailed):**
1. **Timers Phase:** `setTimeout()` and `setInterval()` callbacks with expired timers executed.
2. **Pending I/O Callbacks Phase:** Callbacks from some system operations. Eg: TCP Errors(ECONNREFUSED)
3. **Idle, Prepare Phase:** Internal Node.js usage for housekeeping.
4. **Poll Phase:** I/O callbacks (like file operations) are handled.
5. **Check Phase:** `setImmediate()` callbacks are executed here.
6. **Close Callbacks Phase:** Callbacks for closing handles (like file handles).
7. **Next Tick Phase:** `process.nextTick()` callbacks are executed before the loop moves to the next phase.
8. **Microtask Queue:** Microtasks (Promise callbacks – `.then`, `.catch`, `.finally`) are processed before next phase.

**Diagram (Simplified):**

```
+-------+     +-------+     +-------+     +-------+     +-------+     +-------+     +-------+
|Timers | --> |Pending| --> | Idle,  | --> | Poll  | --> | Check | --> | Close | --> | Next  |
|Phase  |     |  I/O  |     |Prepare|     | Phase |     | Phase |     | Call- | --> | Tick  |
+-------+     +-------+     +-------+     +-------+     +-------+     +-------+     +-------+
                                 ^                                                       |
                                 |                                                       |
                                 +-------------------------------------------------------+
```

```javascript

const fs = require('fs');

console.log('Start of the program');

// File operations (I/O)
fs.open('./myfile.txt', 'r', (err, fd) => {
  if (err) {
    console.error('Error opening file:', err);
  } else {
    console.log('File opened');

    fs.close(fd, (err) => {
      if (err) {
        console.error('Error closing file:', err);
      } else {
        console.log('File closed');
      }
    });

    // I/O callback
    fs.readFile('./myfile.txt', 'utf8', (err, data) => {
        if(err) {
            console.error("Error reading file", err);
        } else {
            console.log("File content:", data);
        }
    });
  }
});


// Set Timeout
setTimeout(() => {
  console.log('Timeout callback');
}, 0); // Even with 0 delay, it goes to the next iteration of the event loop

// Set Immediate
setImmediate(() => {
  console.log('SetImmediate callback');
});

// Process.nextTick
process.nextTick(() => {
  console.log('Process.nextTick callback');
});

// Promise
Promise.resolve().then(() => {
  console.log('Promise resolved');
});

console.log('End of the program');

/*  myfile.txt content (create this file in the same directory)
Hello, this is a test file.
*/
```

```
Start of the program
End of the program       
Process.nextTick callback
Promise resolved
File opened 
SetImmediate callback    
Timeout callback
File closed
File content: Hello, this is a test file.
```

1. **Synchronous:** "Start of the program" is logged.
2. **Asynchronous (File Open):** `fs.open` is called, registering a callback to run *later*.
3. **Asynchronous (Timeout):** `setTimeout` is called, scheduling a callback for the next event loop cycle (or later).
4. **Asynchronous (Immediate):** `setImmediate` is called, scheduling a callback to run after I/O.
5. **Asynchronous (Next Tick):** `process.nextTick` is called, scheduling a callback to run *before* the next event loop cycle.
6. **Asynchronous (Promise):** `Promise.resolve().then()` creates a Promise, its callback will run as a microtask.
7. **Synchronous:** "End of the program" is logged.
8. **Next Tick Queue:** The `process.nextTick` callback executes, logging "Process.nextTick callback".
9. **Microtask Queue:** The Promise `.then()` callback executes, logging "Promise resolved".
10. **Poll Phase (I/O):** The `fs.open` callback executes.
11. **Inside fs.open:** "File opened" is logged.
12. **Inside fs.open (File Close):** `fs.close` is called (asynchronous), registering a callback.
13. **Inside fs.open (File Read):** `fs.readFile` is called (asynchronous), registering a callback.
14. **Check Phase:** The `setImmediate` callback executes, logging "SetImmediate callback".
15. **Timers Phase:** The `setTimeout` callback executes, logging "Timeout callback".
16. **Close Callbacks Queue:** The `fs.close` callback executes, logging "File closed".
17. **Poll Phase (I/O):** The `fs.readFile` callback executes.
18. **Inside fs.readFile:** The file content is logged: "File content: Hello, this is a test file.".

**Key Points from the Node.js Website:**
*   The event loop allows Node.js to handle many asynchronous operations concurrently on a single thread.
*   The phases of the event loop define the order in which different types of callbacks are executed.
*   `process.nextTick()` and Promises' microtask queue provide a way to schedule code to run immediately after the current operation, but before the event loop continues.

**setTimeout() vs setImmediate()**

In main module, the order of execution is non-deterministic, as it is bound by the performance of the process

```javascript
setTimeout(() => {
  console.log("timeout")
}, 0)

setImmediate(() => {
  console.log("immediate")
})
```

Within an I/O cycle, the immediate callback is always executed first.

```javascript
const fs = require("fs")
fs.readFile(".prettierc.json", () => {
  setTimeout(() => {
    console.log("timeout")
  }, 0)
  setImmediate(() => {
    console.log("immediate")
  })
})
```
**process.nextTick()**

process.nextTick() will be processed after the current operation is completed, regardless of current phase of the event loop.
Operation is defined as a transition from the underlying C/C++ handler, and handling the JavaScript that needs to be executed.

* process.nextTick() fires immediately on the same phase
* setImmediate() fires on the following iteration or 'tick' of the event loop

**Why use process.nextTick()?**

* Allow users to handle errors, cleanup unneeded resources, or try the request again before the event loop continues.
* At times to allow a callback to run after the call stack has unwound but before the event loop continues.

```javascript
setImmediate(() => {
  console.log("immediate")
})
console.log("console")
process.nextTick(() => {
  console.log("process tick")
})

/*
process tick
immediate
*/
```

* process.nextTick() -> execute code immediately on the same phase
* setImmediate() -> execute code at the end of the current event loop cycle.
* setTimeout()-> execute after a designated amount of milliseconds.
* setInterval() -> execute multiple times

```javascript
const timeoutObj = setTimeout(() => {
  console.log("timeout beyond time")
}, 1500)

const immediateObj = setImmediate(() => {
  console.log("immediately executing immediate")
})

const intervalObj = setInterval(() => {
  console.log("interviewing the interval")
}, 500)

clearTimeout(timeoutObj)
clearImmediate(immediateObj)
clearInterval(intervalObj)
```

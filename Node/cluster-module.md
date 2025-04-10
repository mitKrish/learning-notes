## Cluster Module

The Node.js `cluster` module allows you to create child processes (workers) that share server ports. This is particularly useful for taking advantage of multi-core processors.  Instead of a single Node.js process handling all incoming requests, you can distribute the load across multiple worker processes, improving performance and resilience.

Here's a breakdown of the `cluster` module:

**Why use the `cluster` module?**

* **CPU Utilization:** Node.js runs in a single thread.  On multi-core systems, a single Node.js process can only utilize one core. The `cluster` module lets you create multiple worker processes, each running on a separate core, maximizing CPU usage.

* **Increased Performance:** By distributing incoming requests across multiple workers, you can handle more concurrent connections and improve the overall performance of your application.

* **Fault Tolerance:** If one worker process crashes (due to an error, for example), the other workers can continue to handle requests. The master process can then respawn the failed worker, ensuring that your application remains available.

**How it works:**

1. **Master Process:** When you use the `cluster` module, a master process is created. This master process is responsible for forking worker processes and managing them.

2. **Worker Processes:** The master process creates worker processes. Each worker process is a separate instance of your Node.js application.

3. **Shared Server:** The master process listens on a specific port. When a request comes in, the master process distributes it to one of the worker processes.

4. **Communication (Optional):** You can set up communication between the master process and the worker processes using `process.send()` and the `message` event.  This is useful for sharing data or coordinating tasks.

**Example:**

```javascript
const cluster = require('cluster');
const http = require('http');
const os = require('os');

const numCPUs = os.cpus().length; // Get the number of CPU cores

if (cluster.isMaster) {
  console.log(`Master ${process.pid} is running`);

  // Fork workers.
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`worker ${worker.process.pid} died`);
    // You can choose to respawn the worker here if needed:
    // cluster.fork();
  });

} else {
  // Worker process
  console.log(`Worker ${process.pid} started`);

  // Create an HTTP server.
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('Hello from worker!');
    console.log(`Worker ${process.pid} handling request`);
  }).listen(8000);
}
```

**Explanation:**

* `cluster.isMaster`: Checks if the current process is the master process.
* `cluster.fork()`: Creates a new worker process.
* `cluster.on('exit', ...)`:  An event listener that is called when a worker process exits.  This is useful for restarting workers or logging crashes.
* `os.cpus().length`: Gets the number of CPU cores on the system. It's common to create one worker per core.
* The `else` block contains the code that runs in each worker process. In this example, it creates an HTTP server.

**Key Considerations:**

* **State Management:** Worker processes don't share memory. If your application needs to maintain state, you'll need to use an external store (like a database or Redis).

* **Sticky Sessions:**  By default, the master process distributes requests to workers in a round-robin fashion. If your application relies on sticky sessions (where a user's requests are always handled by the same worker), you'll need to use a load balancer or a session store.


**In summary:** The `cluster` module is a powerful tool for improving the performance and resilience of Node.js applications by utilizing multiple CPU cores. However, it's essential to understand the implications of using multiple processes and manage state and sessions appropriately.

## Streams

```javascript
const fs = require('fs');
const http = require('http');

// 1. Streaming from a file (readable stream)
const server = http.createServer((req, res) => {
  const readStream = fs.createReadStream('./large_file.txt', { encoding: 'utf8' }); // HighWaterMark can be set

  res.writeHead(200, { 'Content-Type': 'text/plain' }); // or appropriate content type

  // Pipe the read stream to the response stream
  readStream.pipe(res);

  // Error handling for the read stream
  readStream.on('error', (err) => {
    console.error('Error reading file:', err);
    res.writeHead(500, { 'Content-Type': 'text/plain' });
    res.end('Internal Server Error');
  });

  // Handle stream finish (optional)
  readStream.on('finish', () => {
    console.log('File stream finished');
  });
});



// 2. Streaming data (writable stream) - Example: writing to a file
const writeStream = fs.createWriteStream('./output.txt', { encoding: 'utf8' });

writeStream.write('First line\n');
writeStream.write('Second line\n');
writeStream.end(); // Important: signal the end of the stream

writeStream.on('finish', () => {
  console.log('File write stream finished');
});

writeStream.on('error', (err) => {
  console.error('Error writing to file:', err);
});



// 3. Piping streams - Example: transform stream (uppercase)
const { Transform } = require('stream');

const uppercaseStream = new Transform({
  transform(chunk, encoding, callback) {
    const uppercaseChunk = chunk.toString().toUpperCase();
    callback(null, uppercaseChunk); // First argument is the error (null if no error)
  }
});

const readStream2 = fs.createReadStream('./input.txt', { encoding: 'utf8' });
const writeStream2 = fs.createWriteStream('./output_uppercase.txt', { encoding: 'utf8' });

readStream2.pipe(uppercaseStream).pipe(writeStream2); // Chain the streams together

readStream2.on('error', (err) => console.error("Error reading input file:", err));
writeStream2.on('error', (err) => console.error("Error writing output file:", err));


// Start the server
server.listen(3000, () => {
  console.log('Server listening on port 3000');
});


```

**Explanation and Key Concepts:**

1. **Readable Streams:**
   - `fs.createReadStream()`: Creates a readable stream to read data from a file (or any other source).  You can specify the `highWaterMark` option to control the buffer size (how much data is read at a time).
   - `readStream.pipe(res)`: Pipes the data from the readable stream (`readStream`) to the writable stream (`res`, the HTTP response).  This is the most efficient way to handle large files because it avoids loading the entire file into memory.
   - `readStream.on('error', ...)`: Essential for handling potential errors during file reading.
   - `readStream.on('finish', ...)`: Optional, but useful for logging or other actions when the stream is finished.

2. **Writable Streams:**
   - `fs.createWriteStream()`: Creates a writable stream to write data to a file (or any other destination).
   - `writeStream.write()`: Writes a chunk of data to the stream.
   - `writeStream.end()`: *Crucial*: Signals the end of the stream.  The 'finish' event will not be emitted until `end()` is called.
   - `writeStream.on('error', ...)` and `writeStream.on('finish', ...)`: Handle errors and the completion of the write operation.

3. **Transform Streams:**
   - `const { Transform } = require('stream');`: Import the `Transform` class.
   - `new Transform({ transform(chunk, encoding, callback) { ... } })`: Create a transform stream. The `transform` function is where you process the incoming data (`chunk`).  The `callback` function is used to pass the transformed data.  The first argument to the callback is an error (or `null` if there's no error), and the second argument is the transformed chunk.
   - Piping with Transform: You can chain multiple streams together using `.pipe()`. In the example, we chain the `readStream2`, the `uppercaseStream` (our transform stream), and the `writeStream2`.  This allows for powerful data processing pipelines.

**Why Streaming is Important:**

* **Memory Efficiency:** Streaming avoids loading entire files or large datasets into memory. This is essential for handling large files or when dealing with real-time data streams.
* **Performance:**  Data can be processed and sent as it becomes available, reducing latency and improving responsiveness.
* **Resource Management:** Streams can be paused and resumed, allowing you to manage resources effectively.

**Key Points:**

* `pipe()` is the preferred way to connect streams.  It handles backpressure automatically (if the destination stream can't keep up, the source stream will be paused).
* Always handle errors on streams to prevent unexpected behavior.
* `end()` must be called on writable streams to signal completion.
* Transform streams are powerful tools for data processing.

This example demonstrates the fundamental concepts of streaming in Node.js.  You can apply these techniques to various scenarios, such as handling file uploads, processing large datasets, or working with network sockets.  Streaming is a core concept in Node.js and is essential for building efficient and scalable applications.

* **Zero-Downtime Deployments:** The `cluster` module can be used to achieve zero-downtime deployments. You can start new workers, then gracefully shut down the old ones.

* **Complexity:** Using the `cluster` module adds some complexity to your application. You need to manage worker processes and handle communication between them.




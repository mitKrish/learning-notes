## Design Patterns

**1. Module Pattern (Functional Approach)**

* **Intent:** Encapsulates functionality by leveraging closures to create private scope and export specific functions.
* **Implementation:** Functions defined within a module's scope are only accessible if explicitly returned or exported.

```javascript
// math.js (functional module)
const createMathModule = () => {
  const add = (a, b) => a + b;
  const subtract = (a, b) => a - b;

  return {
    add,
    subtract,
  };
};

module.exports = createMathModule();

// app.js
const math = require('./math');

console.log(math.add(5, 3)); // Output: 8
console.log(math.subtract(10, 4)); // Output: 6
```

* **Explanation:** `createMathModule` is a factory function that returns an object containing the `add` and `subtract` functions. The internal implementation details are hidden within the closure.

**2. Middleware Pattern (Functional Middleware)**

* **Intent:** Creates a pipeline of pure functions that transform the request and response objects. Each middleware function takes `req`, `res`, and `next` as arguments.
* **Implementation:** Middleware functions perform their operation and then call `next` to pass control to the subsequent middleware.

```javascript
const express = require('express');
const app = express();

// Functional logger middleware
const logger = (req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
};

// Functional authentication middleware (simplified)
const authenticate = (apiKey) => (req, res, next) => {
  if (req.headers['x-api-key'] === apiKey) {
    next();
  } else {
    res.status(401).send('Unauthorized');
  }
};

// Functional route handler
const home = (req, res) => {
  res.send('Welcome home!');
};

// Applying middleware
app.use(logger); // Applied to all routes
app.get('/', authenticate('valid-key'), home); // Applied only to the '/' route

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});
```

* **Explanation:** `logger` and `authenticate` are higher-order functions. `authenticate` takes the `apiKey` as an argument and returns a middleware function. Each middleware function is a pure function that operates on the `req` and `res` objects and calls `next`.

**3. Observer Pattern (Using Higher-Order Functions)**

* **Intent:** Decouples event emitters from their listeners using functions.
* **Implementation:** An object maintains a list of listener functions associated with specific events. When an event occurs, these functions are invoked.

```javascript
const createEventEmitter = () => {
  const listeners = {};

  const on = (event, listener) => {
    listeners[event] = listeners[event] || [];
    listeners[event].push(listener);
  };

  const emit = (event, ...args) => {
    if (listeners[event]) {
      listeners[event].forEach((listener) => listener(...args));
    }
  };

  return { on, emit };
};

const orderProcessor = createEventEmitter();

// Observer 1: Logging function
const logOrder = (order) => {
  console.log(`New order created: ${JSON.stringify(order)}`);
};

// Observer 2: Notification function
const sendNotification = (order) => {
  console.log(`Sending notification for order: ${order.id}`);
};

// Attach observers
orderProcessor.on('orderCreated', logOrder);
orderProcessor.on('orderCreated', sendNotification);

// Trigger the event
orderProcessor.emit('orderCreated', { id: 1, item: 'Laptop' });
orderProcessor.emit('orderCreated', { id: 2, item: 'Mouse' });
```

* **Explanation:** `createEventEmitter` returns an object with `on` (to register listeners) and `emit` (to trigger events) functions. Listeners are pure functions that are executed when their corresponding event is emitted.

**4. Factory Pattern (Functional Factory)**

* **Intent:** Creates objects using factory functions, abstracting away the concrete object creation.
* **Implementation:** A factory function takes parameters and returns a new object (often a plain JavaScript object in a functional context).

```javascript
// Functional payment gateway creators
const createStripeGateway = () => ({
  processPayment: (amount) => {
    console.log(`Processing $${amount} via Stripe.`);
    return { success: true, transactionId: 'stripe_' + Date.now() };
  },
});

const createPayPalGateway = () => ({
  processPayment: (amount) => {
    console.log(`Processing $${amount} via PayPal.`);
    return { success: true, transactionId: 'paypal_' + Date.now() };
  },
});

// Payment Gateway Factory function
const paymentGatewayFactory = (type) => {
  switch (type) {
    case 'stripe':
      return createStripeGateway();
    case 'paypal':
      return createPayPalGateway();
    default:
      throw new Error(`Unsupported payment gateway type: ${type}`);
  }
};

// Client code
const stripe = paymentGatewayFactory('stripe');
const paypal = paymentGatewayFactory('paypal');

stripe.processPayment(100);
paypal.processPayment(50);
```

* **Explanation:** `createStripeGateway` and `createPayPalGateway` are factory functions that return objects with a `processPayment` method. `paymentGatewayFactory` acts as the central factory to create these gateway objects based on the provided type.

**5. Strategy Pattern (Functions as Strategies)**

* **Intent:** Defines a family of algorithms as functions and makes them interchangeable.
* **Implementation:** Different algorithm implementations are represented as pure functions. A context function accepts a strategy function as an argument.

```javascript
// Strategy functions
const percentageDiscount = (percentage) => (price) => price * (1 - percentage / 100);
const fixedAmountDiscount = (amount) => (price) => Math.max(0, price - amount);

// Context function
const calculateTotal = (price, discountStrategy) => discountStrategy(price);

// Client code
const price1 = 100;
const discountedPrice1 = calculateTotal(price1, percentageDiscount(10));
console.log(`Price with 10% discount: $${discountedPrice1}`); // Output: 90

const price2 = 150;
const discountedPrice2 = calculateTotal(price2, fixedAmountDiscount(20));
console.log(`Price with $20 discount: $${discountedPrice2}`); // Output: 130

const discountedPrice3 = calculateTotal(price2, percentageDiscount(20));
console.log(`Price with 20% discount: $${discountedPrice3}`); // Output: 120
```

* **Explanation:** `percentageDiscount` and `fixedAmountDiscount` are higher-order functions that return discount strategy functions. `calculateTotal` takes a price and a discount strategy function as arguments and applies the strategy.

**6. Decorator Pattern (Higher-Order Functions as Decorators)**

* **Intent:** Dynamically adds behavior to functions by wrapping them with other functions.
* **Implementation:** Decorator functions take a function as input and return a new function with added functionality.

```javascript
// Base function
const simpleCoffeeCost = () => 5;
const simpleCoffeeDescription = () => 'Simple coffee';

// Decorator functions
const withMilk = (costFn, descriptionFn) => () => ({
  cost: costFn() + 1,
  description: `${descriptionFn()}, with milk`,
});

const withSugar = (costFn, descriptionFn) => () => ({
  cost: costFn() + 0.5,
  description: `${descriptionFn()}, with sugar`,
});

// Applying decorators
let coffee = {
  cost: simpleCoffeeCost,
  description: simpleCoffeeDescription,
};

const coffeeWithMilk = withMilk(coffee.cost, coffee.description);
console.log(`${coffeeWithMilk().description} - $${coffeeWithMilk().cost}`);
// Output: Simple coffee, with milk - $6

const coffeeWithMilkAndSugar = withSugar(coffeeWithMilk().cost, coffeeWithMilk().description);
console.log(`${coffeeWithMilkAndSugar().description} - $${coffeeWithMilkAndSugar().cost}`);
// Output: Simple coffee, with milk, with sugar - $6.5
```

* **Explanation:** `withMilk` and `withSugar` are decorator functions. They take the original cost and description functions and return new functions that extend the behavior.

**7. Proxy Pattern (Higher-Order Function as Proxy)**

* **Intent:** Controls access to a function using a proxy function.
* **Implementation:** A proxy function wraps the original function and can perform actions before or after invoking it.

```javascript
// Real function (simulating expensive operation)
const loadResource = (filename) => {
  console.log(`Loading resource: ${filename}`);
  return `Content of ${filename}`;
};

// Proxy function for lazy loading
const createLazyLoader = (loadFn) => {
  let resource = null;
  return (filename) => {
    if (!resource) {
      resource = loadFn(filename);
    }
    return resource;
  };
};

// Creating a lazy loading proxy
const lazyLoadResource = createLazyLoader(loadResource);

// Resource is not loaded yet
console.log('Proxy created.');

// Accessing data for the first time will load the resource
console.log(lazyLoadResource('large_file.txt'));
console.log(lazyLoadResource('large_file.txt')); // Subsequent calls don't reload
```

* **Explanation:** `createLazyLoader` is a higher-order function that takes a loading function and returns a proxy function. The proxy function uses a closure to manage the loaded resource and only invokes the original loading function when needed.

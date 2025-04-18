## SOLID principle:



### **1. Single Responsibility Principle (SRP)**

A module should have one, and only one, reason to change. Each function or class should have a single, well-defined purpose.

**Violation:**

  ```javascript
  function userOperations(user, operation, data) {
    if (operation === "save") {
      // Database logic
      console.log(`Saving user: ${user.name}`);
    } else if (operation === "sendEmail") {
      // Email logic
      console.log(`Sending email to: ${user.email}`);
    }
  }

  const myUser = { name: "Alice", email: "alice@example.com" };
  userOperations(myUser, "save");
  userOperations(myUser, "sendEmail");
  ```

**Solution:**

  ```javascript
  function saveUser(user) {
    // Database logic
    console.log(`Saving user: ${user.name}`);
  }

  function sendEmail(user) {
    // Email logic
    console.log(`Sending email to: ${user.email}`);
  }

  const myUser = { name: "Alice", email: "alice@example.com" };
  saveUser(myUser);
  sendEmail(myUser);
  ```

  * Each function has a single responsibility.

### **2. Open/Closed Principle (OCP)**

Software entities should be open for extension, but closed for modification. You should be able to add new functionality without changing existing code.

**Violation:**

  ```javascript
  function calculateArea(shape, type) {
    if (type === "rectangle") {
      return shape.width * shape.height;
    } else if (type === "circle") {
      return Math.PI * shape.radius * shape.radius;
    }
    // ... more shapes
  }

  console.log(calculateArea({ width: 5, height: 10 }, "rectangle"));
  console.log(calculateArea({ radius: 3 }, "circle"));
  ```

**Solution:**

  ```javascript
  function rectangleArea(shape) {
    return shape.width * shape.height;
  }

  function circleArea(shape) {
    return Math.PI * shape.radius * shape.radius;
  }

  function calculateArea(shape, areaFunction) {
    return areaFunction(shape);
  }

  console.log(calculateArea({ width: 5, height: 10 }, rectangleArea));
  console.log(calculateArea({ radius: 3 }, circleArea));
  ```

  * We pass area calculation functions as arguments, making it open for extension.

### **3. Liskov Substitution Principle (LSP)**
Subtypes must be substitutable for their base types without altering program correctness. Derived classes should be able to replace their base classes without unexpected behavior.

**Violation (Ostrich Problem):**

```javascript
function makeFly(animal) {
  animal.fly(); // Assumes all animals passed have a fly method
}

const bird = { fly: () => console.log("Flying!") };
const ostrich = { fly: () => console.log("Cannot fly!") }; // Misleading

makeFly(bird);    // Output: Flying!
makeFly(ostrich); // Output: Cannot fly! (Behavior altered)
```

**Solution (Separate Behaviors):**

```javascript
function attemptFly(flyer) {
  if (flyer.canFly) flyer.fly();
}

const flyingBird = { canFly: true, fly: () => console.log("Flying!") };
const ostrichCorrect = { canFly: false };

attemptFly(flyingBird);    // Output: Flying!
attemptFly(ostrichCorrect); // No output, correct behavior
```

### **4. Interface Segregation Principle (ISP)**
 
 Clients should not be forced to depend on interfaces they do not use. Smaller, focused interfaces are better than large, general ones.

 **Violation:**

  ```javascript
  function processDevice(device, document) {
    device.print(document);
    device.scan(document);
    device.fax(document);
  }

  const simplePrinter = {
    print: (doc) => console.log(`Printing: ${doc}`),
    scan: (doc) => { throw new Error("Scan not supported"); },
    fax: (doc) => { throw new Error("Fax not supported"); },
  };

  processDevice(simplePrinter, "Document.txt");
  ```

**Solution:**

  ```javascript
  function printDocument(printer, document) {
    printer.print(document);
  }

  function scanDocument(scanner, document) {
    scanner.scan(document);
  }

  function faxDocument(faxer, document) {
    faxer.fax(document);
  }

  const simplePrinter = {
    print: (doc) => console.log(`Printing: ${doc}`),
  };

  printDocument(simplePrinter, "Document.txt");
  ```

    * Separate functions for each operation.

### **5. Dependency Inversion Principle (DIP)**

High-level modules should not depend on low-level modules; both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

 **Violation:**

```javascript
function switchLightBulb(lightBulb, action) {
  if (action === "on") {
    lightBulb.turnOn();
  } else if (action === "off") {
    lightBulb.turnOff();
  }
}

const lightBulb = {
  turnOn: () => console.log("Light bulb on"),
  turnOff: () => console.log("Light bulb off"),
};

switchLightBulb(lightBulb, "on");
```

**Solution:**

```javascript
function switchDevice(device, action) {
  if (action === "on") {
    device.turnOn();
  } else if (action === "off") {
    device.turnOff();
  }
}

const lightBulb = {
  turnOn: () => console.log("Light bulb on"),
  turnOff: () => console.log("Light bulb off"),
};

const fan = {
  turnOn: () => console.log("fan on"),
  turnOff: () => console.log("fan off"),
};

switchDevice(lightBulb, "on");
switchDevice(fan, "on");
```

* `switchDevice` depends on an abstraction (an object with `turnOn` and `turnOff` methods).


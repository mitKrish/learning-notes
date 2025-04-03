
Generics in TypeScript allow you to write reusable code that can work with a variety of types without sacrificing type safety. They enable you to create components that can adapt to different data types while maintaining strong type checking.

**Key Concepts:**

* **Type Parameters:** Generics use type parameters, which are placeholders for specific types that will be determined when the generic component is used.
* **Flexibility and Reusability:** Generics make your code more flexible and reusable, reducing the need to write separate versions for each data type.
* **Type Safety:** Generics preserve type safety by ensuring that the types used with a generic component are consistent.

**Examples:**

**1. Generics in Functions:**

```typescript
// Generic function to reverse an array
function reverseArray<T>(items: T[]): T[] {
  return items.slice().reverse();
}

const numberArray = [1, 2, 3, 4, 5];
const stringArray = ["apple", "banana", "cherry"];

const reversedNumbers = reverseArray(numberArray); // reversedNumbers: number[]
const reversedStrings = reverseArray(stringArray); // reversedStrings: string[]

console.log(reversedNumbers); // Output: [5, 4, 3, 2, 1]
console.log(reversedStrings); // Output: ["cherry", "banana", "apple"]

// Generic function with multiple type parameters.
function merge<U, V>(obj1: U, obj2: V): U & V {
    return {...obj1, ...obj2};
}

const person = {name: "John"};
const additionalInfo = {age: 30};

const mergedObject = merge(person, additionalInfo); // mergedObject: {name: string, age: number}

console.log(mergedObject); // Output: {name: "John", age: 30}
```

**2. Generics in Classes:**

```typescript
// Generic class to create a simple data storage
class DataStorage<T> {
  private data: T[] = [];

  addItem(item: T): void {
    this.data.push(item);
  }

  getItems(): T[] {
    return [...this.data];
  }
}

const numberStorage = new DataStorage<number>();
numberStorage.addItem(10);
numberStorage.addItem(20);
console.log(numberStorage.getItems()); // Output: [10, 20]

const stringStorage = new DataStorage<string>();
stringStorage.addItem("hello");
stringStorage.addItem("world");
console.log(stringStorage.getItems()); // Output: ["hello", "world"]
```

**3. Generics in Type Aliases:**

```typescript
// Generic type alias for a pair of values
type Pair<T, U> = [T, U];

const stringNumberPair: Pair<string, number> = ["age", 30];
const booleanStringPair: Pair<boolean, string> = [true, "valid"];

console.log(stringNumberPair); // Output: ["age", 30]
console.log(booleanStringPair); // Output: [true, "valid"]

//Generic type alias for a custom array type.
type StringArray<T> = Array<T>;

const names : StringArray<string> = ["John", "Jane"];
const ages : StringArray<number> = [25, 30, 35];

console.log(names); //Output: ["John", "Jane"]
console.log(ages); //output: [25, 30, 35]

//Generic type alias used in a function.
type ProcessData<T> = (input: T) => string;

const numberToString: ProcessData<number> = (num) => `Number: ${num}`;
const stringToString: ProcessData<string> = (str) => `String: ${str}`;

console.log(numberToString(123)); //Output: Number: 123
console.log(stringToString("test")); //Output: String: test

```

**Benefits of Generics:**

* **Improved Code Quality:** Generics help catch type errors at compile time, leading to more robust and reliable code.
* **Reduced Code Duplication:** Generics allow you to write reusable components that can work with different data types, reducing the need for repetitive code.
* **Enhanced Code Readability:** Generics make your code more expressive and easier to understand by clearly indicating the types being used.

Generics are a vital tool in TypeScript for creating flexible, reusable, and type-safe code.
In TypeScript, `Partial` is a utility type that transforms an existing type by making all of its properties optional. This is incredibly useful in situations where you want to work with objects that might have only some of their properties defined.

Here's a breakdown:

**What `Partial<T>` Does:**

* It takes a type `T` as its type parameter.
* It constructs a new type where all properties of `T` are set to optional.
* Essentially, it adds the `?` modifier to each property of the original type.

**Why It's Useful:**

* **Updating Objects:** When updating an object, you often don't want to provide values for all its properties. `Partial` allows you to define an update object with only the properties you need to change.
* **Optional Parameters:** In functions, you might want to accept an object where some properties are optional. `Partial` makes this easy to achieve.
* **Flexibility:** It increases the flexibility of your code by allowing you to work with objects that might have incomplete data.

**Example:**

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

function updateUser(id: number, updates: Partial<User>): User {
  // ... logic to update the user ...
  // in this function, updates may only contain some of the user properties.
  return {} as User;
}

let myUser: User = {
  id: 1,
  name: "John Doe",
  email: "john.doe@example.com",
};

// We only want to update the name:
myUser = updateUser(1, { name: "Jane Doe" });

// We only want to update the email:
myUser = updateUser(1, { email: "jane.doe@newexample.com" });

// We want to update the name and email:
myUser = updateUser(1, { name: "test user", email: "test@test.com" });
```

**Explanation:**

* The `User` interface defines three required properties: `id`, `name`, and `email`.
* The `updateUser` function accepts a `Partial<User>` object as the `updates` parameter. This means that the `updates` object can have any combination of the `User` properties, and they are all optional.
* This allows us to call `updateUser` with only the properties we want to change, without having to provide values for all the other properties.

In essence, `Partial` simplifies working with objects where not all properties are always required, making your TypeScript code more adaptable and maintainable.
In TypeScript, the `Required` utility type is the opposite of `Partial`. It takes a type and makes all of its properties required. This is useful when you want to ensure that all properties of a type must be present.

**What `Required<T>` Does:**

* It takes a type `T` as its type parameter.
* It constructs a new type where all properties of `T` are made required, removing any optional (`?`) modifiers.

**Why It's Useful:**

* **Enforcing Property Presence:** When you need to ensure that all properties of an object are provided, `Required` helps enforce this constraint.
* **Data Validation:** It can be used to validate data coming from external sources or APIs, ensuring that all necessary fields are present.
* **Function Parameters:** When a function requires all properties of an object to be present, `Required` can be used to define the parameter type.

**Example:**

```typescript
interface Config {
  apiUrl?: string;
  timeout?: number;
  retries?: number;
}

function processConfig(config: Required<Config>): void {
  console.log(`API URL: ${config.apiUrl}`);
  console.log(`Timeout: ${config.timeout}`);
  console.log(`Retries: ${config.retries}`);
  // all the properties are guaranteed to exist.
}

let partialConfig: Config = {
  apiUrl: "https://api.example.com",
};

// processConfig(partialConfig); // Error: Argument of type 'Config' is not assignable to parameter of type 'Required<Config>'.
// Property 'timeout' is missing in type 'Config' but required in type 'Required<Config>'.

let fullConfig: Required<Config> = {
    apiUrl: "https://api.example.com",
    timeout: 5000,
    retries: 3,
};

processConfig(fullConfig); // This works fine.
```

**Explanation:**

* The `Config` interface defines three optional properties: `apiUrl`, `timeout`, and `retries`.
* The `processConfig` function takes a `Required<Config>` object as its parameter, meaning that all three properties are required.
* When we try to call `processConfig` with `partialConfig`, which only has the `apiUrl` property, TypeScript throws an error because `timeout` and `retries` are missing.
* When a full config object is passed in, the function executes correctly.
* Therefore, `Required` ensures that the functions input config object contains all necessary values.
Alright, let's dive deeper into each of the TypeScript utility types, providing more detailed explanations and usage scenarios:

**1. `Partial<T>`:**

* **Purpose:** Makes all properties of a type `T` optional.
* **Detailed Explanation:** When you apply `Partial<T>`, TypeScript transforms the type `T` by adding the `?` modifier to each of its properties. This means that any property of the resulting type can be present or absent.
* **Use Cases:**
    * Updating objects: When you need to update an object but only want to modify certain properties, `Partial` allows you to define an update object with only the properties you're changing.
    * Function parameters: When a function accepts an object where some properties are optional, `Partial` can be used to define the parameter type.
    * Building forms: When dealing with forms where not all fields are required, `Partial` simplifies the type definitions.
* **Example:**

    ```typescript
    interface User {
      id: number;
      name: string;
      email: string;
    }

    function updateUser(id: number, updates: Partial<User>): User {
      // ... logic to update user ...
      return {} as User;
    }

    updateUser(1, { name: "Jane" }); // Valid; email is optional here.
    ```

**2. `Required<T>`:**

* **Purpose:** Makes all properties of a type `T` required.
* **Detailed Explanation:** `Required<T>` removes the optional (`?`) modifier from all properties of `T`, ensuring that all properties must be present.
* **Use Cases:**
    * Data validation: When receiving data from an external source or API, `Required` can ensure that all necessary fields are present.
    * Configuration settings: When a function requires complete configuration settings, `Required` ensures that all options are provided.
    * Ensuring completeness: When you need to guarantee that an object has all its properties defined.
* **Example:**

    ```typescript
    interface Config {
      apiUrl?: string;
      timeout?: number;
    }

    function processConfig(config: Required<Config>): void {
      console.log(config.apiUrl, config.timeout);
    }

    processConfig({ apiUrl: "test", timeout: 1000 }); // Valid; both apiUrl and timeout are required.
    ```

**3. `Readonly<T>`:**

* **Purpose:** Makes all properties of a type `T` readonly.
* **Detailed Explanation:** `Readonly<T>` prevents any modification of the properties of `T` after the object is created.
* **Use Cases:**
    * Immutable data: When you want to ensure that an object's properties cannot be changed after initialization.
    * Configuration objects: When dealing with configuration settings that should not be modified after loading.
    * Data transfer objects (DTOs): When passing data between components and ensuring that it remains unchanged.
* **Example:**

    ```typescript
    interface Point {
      x: number;
      y: number;
    }

    const p: Readonly<Point> = { x: 10, y: 20 };
    // p.x = 30; // Error: Cannot assign to 'x' because it is a read-only property.
    ```

**4. `Record<K, T>`:**

* **Purpose:** Constructs a type with a set of properties `K` of type `T`.
* **Detailed Explanation:** `Record<K, T>` creates a type where the property keys are of type `K` (typically string or number literals) and the property values are of type `T`.
* **Use Cases:**
    * Dictionaries or maps: When you need to create a data structure with key-value pairs.
    * Configuration objects: When dealing with configuration settings where the keys are known in advance.
    * Lookup tables: When you need to create a table for looking up values based on keys.
* **Example:**

    ```typescript
    type FruitPrices = Record<string, number>;

    const prices: FruitPrices = {
      apple: 1.5,
      banana: 0.75,
      orange: 2.0,
    };
    ```

**5. `Pick<T, K>`:**

* **Purpose:** Constructs a type by picking the set of properties `K` from `T`.
* **Detailed Explanation:** `Pick<T, K>` creates a new type by selecting only the properties specified in `K` from the original type `T`.
* **Use Cases:**
    * Creating subsets of objects: When you only need a subset of properties from a larger object.
    * Defining data transfer objects (DTOs): When you only want to send specific properties to an API.
    * Reducing complexity: When you want to work with a simplified version of a complex type.
* **Example:**

    ```typescript
    interface Todo {
      title: string;
      description: string;
      completed: boolean;
    }

    type TodoPreview = Pick<Todo, "title" | "completed">;

    const preview: TodoPreview = {
      title: "Clean room",
      completed: false,
    };
    ```

**6. `Omit<T, K>`:**

* **Purpose:** Constructs a type by omitting the set of properties `K` from `T`.
* **Detailed Explanation:** `Omit<T, K>` creates a new type by excluding the properties specified in `K` from the original type `T`.
* **Use Cases:**
    * Creating subsets of objects: Similar to `Pick`, but instead of selecting properties, you exclude them.
    * Defining data transfer objects (DTOs): When you want to send all properties except a few.
    * Simplifying complex types: When you want to remove unnecessary properties from a type.
* **Example:**

    ```typescript
    interface Todo {
      title: string;
      description: string;
      completed: boolean;
    }

    type TodoPreview = Omit<Todo, "description">;

    const preview: TodoPreview = {
      title: "Clean room",
      completed: false,
    };
    ```

**7. `Exclude<T, U>`:**

* **Purpose:** Constructs a type by excluding from `T` all union members that are assignable to `U`.
* **Detailed Explanation:** `Exclude<T, U>` removes any types from the union type `T` that are also present in the union type `U`.
* **Use Cases:**
    * Filtering union types: When you want to remove specific types from a union.
    * Creating conditional types: When you need to create types that depend on the presence or absence of other types.
    * Working with discriminated unions: When you want to narrow down the possible types in a union.
* **Example:**

    ```typescript
    type T0 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"
    ```

**8. `Extract<T, U>`:**

* **Purpose:** Constructs a type by extracting from `T` all union members that are assignable to `U`.
* **Detailed Explanation:** `Extract<T, U>` creates a new union type containing only the types that are present in both `T` and `U`.
* **Use Cases:**
    * Filtering union types: Similar to `Exclude`, but instead of removing types, you extract them.
    * Creating conditional types: When you need to create types that depend on the presence of other types.
    * Working with discriminated unions: When you want to narrow down the possible types in a union.
* **Example:**

    ```typescript
    type T0 = Extract<"a" | "b" | "c", "a" | "b">; // "a" | "b"
    ```

**9. `NonNullable<T>`:**

* **Purpose:** Constructs a type by excluding `null` and `undefined` from `T`.
* **Detailed Explanation:** `NonNullable<T>` removes `null` and `undefined` from the union type `T`, ensuring that the resulting type cannot be `null` or `undefined`.
* **Use Cases:**
    * Data validation: When you want to ensure that a value is not `null` or `undefined`.
    * Working with optional values: When you want to remove the possibility of `null` or `undefined` from an optional value.
    * Sanitizing input.
* **Example:**

    ```typescript
    type T0 = NonNullable<string | null | undefined>; // string
    ```

You're right, let's continue with `Parameters<T>` and the remaining utility types:

**10. `Parameters<T>` (Continued):**

* **Purpose:** Constructs a tuple type from the types of a function's parameters.
* **Detailed Explanation:** `Parameters<T>` takes a function type `T` and returns a tuple type containing the types of the function's parameters.
* **Use Cases:**
    * Dynamically inspecting function parameter types.
    * Creating generic functions or decorators that work with functions of various parameter types.
    * Type checking function arguments at compile time.
* **Example:**

    ```typescript
    function greet(name: string, age: number): string {
      return `Hello, ${name}! You are ${age} years old.`;
    }

    type GreetParams = Parameters<typeof greet>; // [string, number]

    function logParameters(...args: GreetParams): void {
        console.log("Parameters are: ", args);
    }

    logParameters("Alice", 30);
    ```

**11. `ConstructorParameters<T>`:**

* **Purpose:** Constructs a tuple or array type from the types of a constructor function type's parameters.
* **Detailed Explanation:** `ConstructorParameters<T>` takes a constructor function type `T` (e.g., a class constructor) and returns a tuple type containing the types of the constructor's parameters.
* **Use Cases:**
    * Dynamically inspecting constructor parameter types.
    * Creating generic factory functions or dependency injection systems.
    * Type checking class constructor arguments at compile time.
* **Example:**

    ```typescript
    class Person {
      constructor(public name: string, public age: number) {}
    }

    type PersonParams = ConstructorParameters<typeof Person>; // [string, number]

    function createPerson(...args: PersonParams): Person {
        return new Person(args[0], args[1]);
    }

    const alice = createPerson("Alice", 25);
    console.log(alice);
    ```

**12. `ReturnType<T>`:**

* **Purpose:** Constructs a type consisting of the return type of function `T`.
* **Detailed Explanation:** `ReturnType<T>` takes a function type `T` and returns the type of the value that the function returns.
* **Use Cases:**
    * Dynamically inspecting function return types.
    * Creating generic functions or decorators that work with functions of various return types.
    * Type checking function return values at compile time.
* **Example:**

    ```typescript
    function greet(name: string): string {
      return `Hello, ${name}!`;
    }

    type GreetReturn = ReturnType<typeof greet>; // string

    function processGreeting(greeting: GreetReturn): void {
        console.log("Processed Greeting: ", greeting);
    }
    processGreeting(greet("Bob"));
    ```

**13. `InstanceType<T>`:**

* **Purpose:** Constructs a type consisting of the instance type of a constructor function type `T`.
* **Detailed Explanation:** `InstanceType<T>` takes a constructor function type `T` (e.g., a class constructor) and returns the type of the instances that the constructor creates.
* **Use Cases:**
    * Dynamically inspecting the instance type of a class.
    * Creating generic factory functions or dependency injection systems.
    * Type checking class instances at compile time.
* **Example:**

    ```typescript
    class Person {
      constructor(public name: string, public age: number) {}
    }

    type PersonInstance = InstanceType<typeof Person>; // Person

    function logPerson(person: PersonInstance): void {
        console.log("Person: ", person);
    }
    logPerson(new Person("Charlie", 35));
    ```

These utility types are essential for writing flexible, reusable, and type-safe TypeScript code. They enable you to perform complex type transformations and inspections, making your code more robust and maintainable.
Decorators in TypeScript are a powerful feature that allows you to add metadata and modify the behavior of classes, methods, accessors, properties, or parameters. They are a form of metaprogramming, enabling you to write code that operates on other code.

Here's a breakdown of decorators in TypeScript:

**Key Concepts:**

* **Metadata and Modification:** Decorators provide a way to attach metadata to declarations and modify their behavior at runtime.
* **Syntax:** Decorators use the `@expression` syntax, where `expression` must evaluate to a function that will be called at runtime with information about the decorated declaration.
* **Types of Decorators:**
    * **Class Decorators:** Apply to class constructors.
    * **Method Decorators:** Apply to method declarations.
    * **Accessor Decorators:** Apply to accessor (getter/setter) declarations.
    * **Property Decorators:** Apply to property declarations.
    * **Parameter Decorators:** Apply to parameter declarations.

**How Decorators Work:**

* When a decorator is applied to a declaration, TypeScript calls the decorator function with information about the declaration, such as the target object, the property name, and the descriptor.
* The decorator function can then use this information to modify the declaration's behavior or add metadata.

**Example:**

```typescript
function logClass(constructor: Function) {
  console.log(`Class ${constructor.name} was created.`);
}

@logClass
class MyClass {
  constructor() {
    console.log("MyClass constructor called.");
  }
}

const myInstance = new MyClass();
```

In this example:

* `logClass` is a class decorator that logs a message when the `MyClass` constructor is called.
* `@logClass` applies the decorator to the `MyClass` class.

**Types of Decorators and their parameters:**

1.  **Class Decorators:**
    * Takes a single argument: the class constructor function.
    * Allows you to modify the class constructor or add metadata to the class.

2.  **Method Decorators:**
    * Takes three arguments:
        * `target`: The prototype of the class (for instance methods) or the class constructor function (for static methods).
        * `propertyKey`: The name of the method.
        * `descriptor`: The property descriptor for the method.
    * Allows you to modify the method's behavior or add metadata to the method.

3.  **Accessor Decorators:**
    * Takes the same arguments as method decorators.
    * Allows you to modify the getter or setter's behavior.

4.  **Property Decorators:**
    * Takes two arguments:
        * `target`: The prototype of the class (for instance properties) or the class constructor function (for static properties).
        * `propertyKey`: The name of the property.
    * Allows you to add metadata to the property.

5.  **Parameter Decorators:**
    * Takes three arguments:
        * `target`: The prototype of the class (for instance methods) or the class constructor function (for static methods).
        * `propertyKey`: The name of the method.
        * `parameterIndex`: The index of the parameter in the parameter list.
    * Allows you to add metadata to the parameter.

**Use Cases:**

* **Logging:** Logging method calls or class creation.
* **Validation:** Validating method parameters or object properties.
* **Dependency Injection:** Injecting dependencies into classes.
* **Routing:** Defining routes in web applications.
* **Metadata:** Adding metadata to classes or methods for use by other tools or libraries.

**Important Notes:**

* Decorators are an experimental feature in TypeScript, and you need to enable them in your `tsconfig.json` file by setting `"experimentalDecorators": true`.
* Decorators can greatly increase the readability and maintainability of code by separating cross cutting concerns.
* Frameworks like Angular heavily utilize decorators.
* Decorator factories are functions that return decorators, and allow for decorator customization.

Decorators provide a powerful way to enhance and modify TypeScript code, enabling you to write more concise and expressive code.
Absolutely! Let's delve into each of the TypeScript topics I mentioned, providing more detailed explanations and examples to solidify your understanding.

**1. Difference between `interface` and `type`:**

* **`interface`:**
    * Primarily used to define the shape of an object.
    * Can be extended using `extends`.
    * Can be merged (declaration merging).
    * Cannot represent union or intersection types directly.
    * Best for defining contracts for objects.
    * Example:

        ```typescript
        interface Person {
          name: string;
          age: number;
        }

        interface Employee extends Person {
          employeeId: string;
        }

        const employee: Employee = {
          name: "Alice",
          age: 30,
          employeeId: "123",
        };
        ```

* **`type`:**
    * More versatile; can represent any kind of type, including primitives, unions, intersections, and tuples.
    * Cannot be merged.
    * Can be used to create type aliases.
    * Best for defining complex type relationships.
    * Example:

        ```typescript
        type Point = { x: number; y: number };
        type StringOrNumber = string | number;
        type Tuple = [string, number];

        const point: Point = { x: 10, y: 20 };
        const value: StringOrNumber = "hello";
        const tuple: Tuple = ["test", 1];
        ```

* **Key Differences:**
    * Declaration merging: Interfaces can be merged; types cannot.
    * Union and intersection types: Types can represent these directly; interfaces cannot.
    * Versatility: Types are more versatile; interfaces are more specific to object shapes.

**2. Ambient Declarations (`.d.ts` files):**

* **Purpose:** To describe the shape of existing JavaScript code to the TypeScript compiler.
* **Use Cases:**
    * Typing JavaScript libraries that don't have built-in type definitions.
    * Describing global variables or functions.
    * Working with non-TypeScript code in a TypeScript project.
* **Example:**

    ```typescript
    // myLibrary.d.ts
    declare function myLibraryFunction(message: string): void;

    // myScript.ts
    myLibraryFunction("Hello"); // TypeScript knows the type of myLibraryFunction.
    ```

**3. Enums:**

* **Numeric Enums:**
    * Values are automatically assigned numeric values (starting from 0).
    * Reverse mapping is available (you can access enum members by their values).
    * Example:

        ```typescript
        enum Direction {
          Up, // 0
          Down, // 1
          Left, // 2
          Right, // 3
        }

        console.log(Direction.Up); // 0
        console.log(Direction[1]); // "Down"
        ```

* **String Enums:**
    * Values are explicitly assigned string literals.
    * Reverse mapping is not available.
    * Example:

        ```typescript
        enum LogLevel {
          Error = "ERROR",
          Warning = "WARNING",
          Info = "INFO",
        }

        console.log(LogLevel.Error); // "ERROR"
        ```

* **Constant Enums:**
    * Are fully removed during compilation.
    * Can only contain constant enum expressions.
    * Improves performance by inlining values.
    * Example:

        ```typescript
        const enum Color {
          Red = 0xff0000,
          Green = 0x00ff00,
          Blue = 0x0000ff,
        }

        const red = Color.Red; // Replaced with 0xff0000 during compilation.
        ```

**4. Type Assertion vs. Type Casting:**

* **Type Assertion (`as`):**
    * Tells the compiler "I know more about the type than you do."
    * Does not perform runtime type checking.
    * Example:

        ```typescript
        const input = document.getElementById("myInput") as HTMLInputElement;
        console.log(input.value);
        ```

* **Type Casting (`<Type>`):**
    * Older syntax.
    * Equivalent to type assertion using `as`.
    * Example:

        ```typescript
        const input = <HTMLInputElement>document.getElementById("myInput");
        console.log(input.value);
        ```

* **Differences:**
    * `as` is generally preferred because it avoids ambiguity with JSX syntax.
    * They are functionally the same. They do not perform runtime checks.

**5. Type Inference:**

* **How it works:** TypeScript infers types based on the context in which a variable or expression is used.
* **Example:**

    ```typescript
    let message = "Hello"; // TypeScript infers message as string.
    let number = 10; // TypeScript infers number as number.

    function add(a: number, b: number) {
      return a + b; // TypeScript infers return type as number.
    }
    ```

* **When to use explicit type annotations:**
    * When the compiler cannot infer the correct type.
    * When you want to be explicit about the type.
    * When dealing with complex types.

**6. Discriminated Unions (Tagged Unions):**

* **Purpose:** To create types that can be narrowed down based on a common discriminant property.
* **Example:**

    ```typescript
    interface Circle {
      kind: "circle";
      radius: number;
    }

    interface Square {
      kind: "square";
      sideLength: number;
    }

    type Shape = Circle | Square;

    function getArea(shape: Shape) {
      switch (shape.kind) {
        case "circle":
          return Math.PI * shape.radius ** 2;
        case "square":
          return shape.sideLength ** 2;
      }
    }
    ```

**7. Conditional Types:**

* **Purpose:** To create types that depend on other types.
* **Example:**

    ```typescript
    type NonNullable<T> = T extends null | undefined ? never : T;

    type T0 = NonNullable<string | null>; // string
    ```

**8. Mapped Types:**

* **Purpose:** To transform existing types into new types.
* **Example:**

    ```typescript
    type Readonly<T> = {
      readonly [P in keyof T]: T[P];
    };

    interface Todo {
      title: string;
      description: string;
    }

    type ReadonlyTodo = Readonly<Todo>;
    ```

**9. Modules and Namespaces:**

* **Modules:**
    * Preferred way to organize code in TypeScript.
    * Use `import` and `export` statements.
    * Follow the ECMAScript module standard.
    * Example:

        ```typescript
        // math.ts
        export function add(a: number, b: number) {
          return a + b;
        }

        // main.ts
        import { add } from "./math";
        console.log(add(1, 2));
        ```

* **Namespaces:**
    * Older way to organize code.
    * Use the `namespace` keyword.
    * Can be used to group related code.
    * Example:

        ```typescript
        namespace MathUtils {
          export function add(a: number, b: number) {
            return a + b;
          }
        }

        console.log(MathUtils.add(1, 2));
        ```

Alright, let's dive deeper into those advanced and practical TypeScript topics with more examples and explanations:

**1. TypeScript's Configuration (`tsconfig.json`):**

* **Purpose:** `tsconfig.json` is the configuration file for the TypeScript compiler. It controls how TypeScript compiles your code.
* **Key Compiler Options:**
    * `"target"`: Specifies the ECMAScript target version (e.g., "ES5", "ES2015", "ESNext"). This determines the JavaScript output.
        * Example: `"target": "ES2017"`
    * `"module"`: Specifies the module code generation (e.g., "commonjs", "esnext", "amd").
        * Example: `"module": "esnext"`
    * `"strict"`: Enables all strict type-checking options, which are highly recommended for robust code.
        * Example: `"strict": true`
    * `"noImplicitAny"`: Disallows implicit `any` types, forcing you to explicitly annotate types.
        * Example: `"noImplicitAny": true`
    * `"esModuleInterop"`: Enables interoperability between CommonJS and ES modules.
        * Example: `"esModuleInterop": true`
    * `"moduleResolution"`: Specifies how TypeScript resolves modules.
        * Example: `"moduleResolution": "node"`
    * `"outDir"`: Specifies the output directory for compiled JavaScript files.
        * Example: `"outDir": "./dist"`
    * `"lib"`: Specifies the libraries to include in the compilation (e.g., "es2015", "dom").
        * Example: `"lib": ["es2017", "dom"]`
* **Configuration for Different Environments:**
    * You might have different `tsconfig.json` files for development, production, and testing. For example, you might enable source maps in development but disable them in production.
    * You can use `extends` in `tsconfig.json` to inherit configurations from a base file.
* **Example `tsconfig.json`:**

    ```json
    {
      "compilerOptions": {
        "target": "ES2020",
        "module": "esnext",
        "strict": true,
        "esModuleInterop": true,
        "moduleResolution": "node",
        "outDir": "./dist",
        "sourceMap": true,
        "lib": ["es2020", "dom"]
      },
      "include": ["src/**/*.ts"],
      "exclude": ["node_modules"]
    }
    ```

**2. Working with Libraries and Frameworks:**

* **TypeScript with React:**
    * React projects created with `create-react-app` can be configured to use TypeScript.
    * Type definitions for React components and props are crucial.
    * Example:

        ```typescript
        interface MyComponentProps {
          name: string;
          age: number;
        }

        const MyComponent: React.FC<MyComponentProps> = ({ name, age }) => {
          return <div>{name}, {age}</div>;
        };
        ```

* **Typing Third-Party Libraries:**
    * Libraries often have type definitions available on DefinitelyTyped (`@types/*`).
    * If not, you can create your own ambient declarations (`.d.ts` files).
    * Example: using a library that manipulates dates.

        ```typescript
        // if no @types/date-manipulation exist.
        // date-manipulation.d.ts
        declare module 'date-manipulation' {
            export function formatDate(date: Date, format: string): string;
        }

        // myFile.ts
        import { formatDate } from 'date-manipulation';

        const myDate = new Date();
        const formattedDate = formatDate(myDate, 'YYYY-MM-DD');
        console.log(formattedDate);
        ```

**3. Asynchronous Programming:**

* **`async/await` with TypeScript:**
    * TypeScript supports `async/await` for asynchronous operations.
    * Promises are typed, and `async` functions return Promises.
    * Example:

        ```typescript
        async function fetchData(url: string): Promise<any> {
          const response = await fetch(url);
          const data = await response.json();
          return data;
        }

        fetchData("https://api.example.com/data").then((data) => {
          console.log(data);
        });
        ```

* **Typing Promises and Observables:**
    * Promises and Observables can be typed with generic type parameters.
    * Example:

        ```typescript
        const promise: Promise<string> = new Promise((resolve) => {
          setTimeout(() => resolve("Hello"), 1000);
        });
        ```

**4. Performance Considerations:**

* **Complex Type Definitions:**
    * Excessively complex type definitions can increase compile time.
    * Avoid deeply nested conditional types or mapped types when possible.
* **Techniques for Optimization:**
    * Use `interface` when appropriate; it's often faster than complex `type` aliases.
    * Use `const enum` for performance gains.
    * Optimize `tsconfig.json` settings (e.g., incremental builds).
    * Use type narrowing to decrease the amount of work the compiler must do.
* **Example of type narrowing:**

    ```typescript
    function processValue(value: string | number) {
      if (typeof value === "string") {
        // TypeScript knows value is a string here.
        console.log(value.toUpperCase());
      } else {
        // TypeScript knows value is a number here.
        console.log(value * 2);
      }
    }
    ```

**5. TypeScript and JavaScript Interoperability:**

* **Working with JavaScript Codebases:**
    * TypeScript can gradually be introduced into existing JavaScript projects.
    * Use `.allowJs` in `tsconfig.json` to include JavaScript files.
    * Use `.d.ts` files to type JavaScript code.
* **Migrating JavaScript to TypeScript:**
    * Start by adding type annotations to critical parts of the code.
    * Gradually refactor JavaScript files to TypeScript files.
    * Use tools like `ts-migrate` to automate parts of the migration.
* **Example using allowJS:**

    ```json
    {
        "compilerOptions": {
            "allowJs": true,
            "outDir": "./dist",
            //... other options.
        },
        "include": ["src/**/*"]
    }
    ```

**6. Testing TypeScript Code:**

* **Unit and Integration Tests:**
    * Use testing frameworks like Jest or Mocha with TypeScript.
    * Type definitions for testing libraries are essential.
    * Example using Jest:

        ```typescript
        // math.ts
        export function add(a: number, b: number): number {
          return a + b;
        }

        // math.test.ts
        import { add } from "./math";

        describe("math", () => {
          it("should add two numbers", () => {
            expect(add(1, 2)).toBe(3);
          });
        });
        ```

These examples and explanations should provide a more comprehensive understanding of these advanced and practical TypeScript topics.



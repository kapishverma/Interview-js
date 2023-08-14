[Basics of variables(var, let, const)](#basic-of-variables-var-let-const)
[Hoisting](#hoisting)
[Datatypes](#datatypes)
[undefined vs null](#undefined-vs-null)
[symbol](#symbol-datatype)
[setTimeout vs setInterval](#setinterval-and-settimeout)
[Synchronous and Asynchronous](#synchronous-and-asynchronous-execution)
[try/catch and .then/.catch](#trycatch-and-thencatch)
[why cant you use .catch() after another .catch()?](#why-cant-you-use-catch-after-another-catch)
[async/await]()
[Promises](#promises)
[fetch API (either promise or async await)](#fetch-api)
[concept of closures](#closure)
[Nested Functions](#nested-function)
[Callback and Callback hell](#callback-and-callback-hell)
[ states of promises, how to create promise](#states-of-promises-how-to-create-promise-how-to-promise)
[The rest and spread parameters (...)](#the-rest-and-spread-parameters)
[ call, bind and apply](#call-apply-and-bind)
[bind vs closure](#bind-vs-closure)
[IIFE](#iife-immediately-invoked-function-expression)
[web storges (cookies, local storage, session storage)](#web-storage-cookies-local-storage-session-storage)
[Classes and Modules](#classes-and-modules)
[print 1 after 1 sec, print 2 after 2 sec, .... 5 after sec (using setTimeout)]()


---
> # Basic of variables (var, let, const)
In JavaScript, variables are used to store data values. The three ways to declare variables are var, let, and const. Each has its own scope and behavior regarding reassignment. Let's dive into each one in detail:

* **var:**
The var keyword was traditionally used to declare variables in JavaScript. Variables declared with var have either function-level scope or global scope, depending on where they are declared.Variables declared with var are "hoisted," meaning they are moved to the top of their scope during compilation. This can lead to unexpected behavior.
```js
var x = 10;
console.log(x); // Output: 10

if (true) {
    var x = 20;
    console.log(x); // Output: 20 (the same variable is modified)
}

console.log(x); // Output: 20 (the value was modified in the if block)
```
In this code, both instances of the variable x refer to the same variable because var declarations are not block-scoped, but rather function-scoped or globally scoped. The if block does not create a new scope for x, so when you modify x within the if block, it affects the outer scope as well.

This behavior is due to the way var variables are hoisted and scoped. When you use var, the variable is hoisted to the top of the function or global scope, so the entire code block behaves as if the variable declarations and assignments are moved to the top of the code.

* **let:**
The let keyword introduced in ECMAScript 6 (ES6) provides block-scoped variables that can be reassigned.

```js
let y = 30;
console.log(y); // Output: 30

if (true) {
    let y = 40;
    console.log(y); // Output: 40 (a new variable y is created within the block)
}

console.log(y); // Output: 30 (the original variable y is unchanged)
```
Variables declared with let are block-scoped, meaning they are confined to the block they are declared in.

* **const:**
The const keyword is used to declare variables that can't be reassigned after their initial value is assigned.
```js
const z = 50;
console.log(z); // Output: 50

// Uncommenting the following line will result in an error
// z = 60;
```
Variables declared with const are also block-scoped, just like let.

**Here's a summary:**

* Use **var** if you want function or global scope with hoisting **(not recommended in modern JavaScript)**.
* Use **let** if you want block scope and reassignment.
* Use **const** if you want block scope and a constant value (cannot be reassigned).

Modern JavaScript codebases typically prefer using let and const over var due to their more predictable scoping and behavior. It's a good practice to use const whenever the value of a variable will not change, and use let when the variable may need to be reassigned.

---
> # Hoisting
 **Hoisting** is a JavaScript behavior where variable and function declarations are moved to the top of their containing scope during the compilation phase. This means that you can use variables and call functions before they are actually declared in your code. However, <u>it's important to understand that only the declarations are hoisted, not the assignments</u>

---

```js
console.log(x); // Output: undefined
var x = 10;
console.log(x); // Output: 10
```
Here's what happens during the execution:

* The var x = 10; declaration is hoisted to the top of the scope.
* The first console.log(x); is executed. At this point, x is declared but not assigned a value, so it's undefined.
* The assignment x = 10; takes place.
* The second console.log(x); is executed. Now, x has the value 10.

To avoid unexpected behavior due to hoisting, it's a good practice to declare variables at the top of their scope and initialize them before using them. Here's how hoisting works with functions:

```js
foo(); // Output: "Hello"

function foo() {
    console.log("Hello");
}
```
In this case:

* The function foo() declaration is hoisted to the top of the scope.
* The foo() function is called, and it outputs "Hello".

**Hoisting** can lead to confusion and bugs, especially when using var. That's why modern JavaScript development prefers using let and const for variables, as they have block scope and are not hoisted in the same way as var. Function declarations are still hoisted, but using function expressions (e.g., const func = function() {}) or arrow functions can help make the code behavior more predictable.

---

># Closure

A closure in JavaScript is a function that "remembers" the variables and parameters from the scope in which it was created, even after that scope has finished executing. This allows the function to retain access to those variables and parameters, even if they are not in scope anymore. Closures are powerful because they enable data encapsulation and the creation of functions that can maintain state between invocations.

Here's a simple example to illustrate closures:
```js
function outerFunction() {
    const outerVariable = "I'm from the outer function";

    function innerFunction() {
        console.log(outerVariable); // Accesses outerVariable from the outer scope
    }

    return innerFunction; // Returns the inner function as a closure
}

const closureExample = outerFunction(); // Returns the inner function
closureExample(); // Logs "I'm from the outer function"
```

In this example:

* outerFunction defines a local variable outerVariable and a nested function innerFunction.
* innerFunction is returned from outerFunction, forming a closure.
* The closureExample variable holds the returned innerFunction.

When closureExample() is called, it logs the value of outerVariable even though outerFunction has already finished executing. This is because innerFunction still "remembers" the scope in which it was created, and it retains access to the outerVariable.

Closures are often used to create functions with private data, encapsulation, and data hiding:

```js
function counter() {
    let count = 0;

    return {
        increment: function() {
            count++;
        },
        get: function() {
            return count;
        }
    };
}

const myCounter = counter();
myCounter.increment();
console.log(myCounter.get()); // Outputs: 1
```

In this example, the counter function returns an object containing two methods: increment and get. The count variable is "closed over" by these methods, allowing them to access and modify its value even though it's not accessible from outside the counter function. This demonstrates the concept of closures in a practical way.

#### Nested Function:

A nested function is a function that is defined within the scope of another function. The inner (nested) function has access to variables and parameters of the outer (enclosing) function. This concept is often referred to as "closure," as the inner function retains access to the enclosing function's scope even after the outer function has finished executing.

Example of a nested function:
```js
function outerFunction() {
    const outerVariable = "I'm from the outer function";

    function innerFunction() {
        console.log(outerVariable); // Accesses outerVariable from the outer scope
    }

    return innerFunction; // Returns the inner function as a closure
}

const closureExample = outerFunction(); // Returns the inner function
closureExample(); // Logs: I'm from the outer function
```

In this example, the innerFunction is nested within the outerFunction. When closureExample() is called, it logs the value of outerVariable from the outer scope of outerFunction.

Nested functions are particularly useful when you need to encapsulate functionality within a specific context and maintain access to that context's data even after the context has finished executing. They are often used to create closures, which can be powerful for data encapsulation, private data, and creating custom behaviors.

---

> # DataTypes

In JavaScript, data types define the kind of value a variable can hold. JavaScript has several built-in data types that cover a wide range of values, from numbers to strings, and even more complex structures like objects and arrays. Understanding JavaScript data types is crucial for effective programming. Let's go through the main data types in JavaScript:

* **Number:**
Represents both integer and floating-point numbers. For example: 42, 3.14.

*  **String:**
Represents a sequence of characters enclosed in single or double quotes. For example: 'Hello', "JavaScript".

* **Boolean:**
Represents a logical value - either true or false.

* **Undefined:**
Represents a variable that has been declared but has not been assigned a value. Variables are initialized with undefined by default.

* **Null:**
Represents an intentional absence of any value or object.

* **Symbol (ES6):**
Represents a unique and immutable value used for object properties. Symbols are often used as keys for object properties to prevent name collisions.

* **Object:**
Represents a collection of key-value pairs. Objects can hold various types of data, including other objects, arrays, functions, and more.

* **Array:**
Represents an ordered collection of values, which can be of any data type. Arrays are defined using square brackets. For example: [1, 2, 3].

* **Function:**
Represents a block of reusable code that can be executed when called.

* **BigInt (ES11):**
Represents arbitrary-precision integers, which are larger than what the Number type can safely represent. BigInts are created by appending n to the end of an integer literal. For example: 1234567890123456789012345678901234567890n.

***JavaScript is a dynamically typed language, which means you don't need to explicitly declare the data type of a variable. The data type is determined automatically based on the value assigned to the variable. For example:***

```js
let age = 25;         // number
let name = "Alice";   // string
let isAdmin = true;   // boolean
let person = null;    // null
let personInfo;       // undefined

console.log(typeof age);      // Output: "number"
console.log(typeof name);     // Output: "string"
console.log(typeof isAdmin);   // Output: "boolean"
console.log(typeof person);    // Output: "object" (Note: This is a known quirk in JavaScript)
console.log(typeof personInfo); // Output: "undefined"
```
---

> # undefined vs null
Both ***undefined*** and ***null*** in JavaScript represent the absence of a value, but they are used in slightly different contexts. Understanding their differences is important for writing clean and bug-free code.

**undefined** is a value that a variable has when it is declared but not initialized with any value. It is automatically assigned to variables when they are declared without an explicit value. <u>Functions that do not have a return statement also implicitly return undefined.</u>
```js
let x; // x is undefined because it has not been assigned a value

function greet(name) {
    console.log("Hello, " + name);
    // The function implicitly returns undefined if no return statement is used
}

console.log(x); // Output: undefined
console.log(greet("Alice")); // Output: Hello, Alice \n undefined
```
**null** is a value that represents the intentional absence of any value. It is often used to explicitly indicate that a variable does not have a value. It is different from undefined in that it is assigned by the programmer to represent a missing value.

```js
let person = null; // Explicitly assigning null to indicate that no person exists

function findPerson(name) {
    // Code to find the person, if not found, return null
    return null;
}

console.log(person); // Output: null
console.log(findPerson("Bob")); // Output: null
```

**In summary:**

* Use undefined when a variable has been declared but not assigned a value.
* Use null when you want to explicitly indicate that a variable intentionally has no value.
* It's often recommended to use null rather than undefined for indicating the absence of a value, as it's more explicit.

In practice, it's common to see null being used to represent the absence of a value in scenarios where the absence is expected and intentional, while undefined may appear when variables have not been properly initialized.

---

> # Symbol datatype

The Symbol data type in JavaScript was introduced in ECMAScript 6 (ES6) and is used to create unique and immutable values. Symbols are often used as keys for object properties to avoid naming collisions in object properties and provide a level of privacy.

Here are the key features of the Symbol data type:

* **Uniqueness:** Each Symbol value is unique. Even if you create multiple symbols with the same description, they will be distinct values.

* **Immutability:** Symbols are immutable, meaning their value cannot be changed after creation.

* **Private Property Keys:** Symbols are often used to create private property keys for objects. Since they are unique and not exposed through iteration, they provide a way to create "hidden" properties that won't be accidentally accessed.

* **Description (Optional):** Symbols can be created with an optional description, which is mainly used for debugging and identification purposes.

Here's how you can create and use Symbols:

```javascript

// Creating a symbol
const mySymbol = Symbol();

// Creating a symbol with a description
const namedSymbol = Symbol("myNamedSymbol");

// Using symbols as object property keys
const obj = {};
obj[mySymbol] = "Value for mySymbol";
obj[namedSymbol] = "Value for myNamedSymbol";

console.log(obj[mySymbol]);       // Output: "Value for mySymbol"
console.log(obj[namedSymbol]);    // Output: "Value for myNamedSymbol"

// Symbols are not exposed through iteration
console.log(Object.keys(obj));    // Output: []
console.log(Object.getOwnPropertySymbols(obj)); // Output: [ Symbol(), Symbol(myNamedSymbol) ]

// Checking if a property with a symbol key exists
console.log(obj[Symbol()]);       // Output: undefined
console.log(obj[Symbol("test")]); // Output: undefined
console.log(obj[namedSymbol]);    // Output: "Value for myNamedSymbol"
```
Think of a Symbol like a special, unique identifier that you can use in your JavaScript code. It's like having a secret code that only you know about. Symbols are useful because they help you avoid accidentally using the same name for different things in your code.

Here's a simple example:

```javascript

// Imagine you're creating a game character object
const character = {};

// You want to give the character a health property, but you want it to be unique
// so it doesn't accidentally conflict with other properties
const healthSymbol = Symbol("health");

// Now, you can use this symbol as a special key for the health property
character[healthSymbol] = 100;

// When you want to access the health property, you use the symbol as the key
console.log(character[healthSymbol]); // Output: 100
```
In this example, healthSymbol is a unique Symbol that you use as a key for the character's health property. Since Symbols are unique, you can be confident that you won't accidentally overwrite or conflict with other properties.

 The term "symbol" in the context of programming languages like JavaScript refers to a specific concept that's different from the symbols you mentioned like !, @, #, etc.

<u>In programming, a "symbol" is a data type that represents a unique and immutable value. It's often used to create special identifiers, like keys for object properties, in a way that guarantees uniqueness and avoids conflicts. While the symbols you mentioned (!, @, #, etc.) are special characters used in various contexts, they aren't directly related to the Symbol data type in programming languages like JavaScript.</u>

Symbols are often used in advanced programming scenarios, like creating private properties or adding special behaviors to objects.Symbols are particularly useful when you want to create private properties in objects or when you need to create unique identifiers. They are commonly used in scenarios where you need to ensure that the property keys won't collide with other keys or be accidentally modified.They might seem a bit complex at first, but they're a powerful tool once you get the hang of them.

---
> # setInterval and setTimeout

**setTimeout** and **setInterval** are both functions in JavaScript that are used to schedule the execution of a piece of code at a specific time or after a specific interval. However, they have different use cases and behavior:

* **setTimeout:**
The setTimeout function is used to execute a function (or evaluate an expression) after a specified delay, measured in milliseconds. After the specified delay, the function will be executed once.

syntax:
```js
setTimeout(function, delay);
```
example:
```js
console.log("Start");
setTimeout(() => {
    console.log("Delayed message");
}, 2000); // Executes after 2000 milliseconds (2 seconds)
console.log("End");
```
output:
```js
Start
End
Delayed message
```
* **setInterval:**
The setInterval function is used to repeatedly execute a function (or evaluate an expression) at a specified interval. The function will be executed at the specified interval until the interval is cleared using clearInterval.

Syntax:

```javascript
setInterval(function, interval);
```
Example:
```javascript
let count = 0;
const intervalId = setInterval(() => {
    console.log("Interval message");
    count++;
    if (count === 3) {
        clearInterval(intervalId); // Stop the interval after 3 executions
    }
}, 1000); // Executes every 1000 milliseconds (1 second)
```
Output:
```js
mathematica
Copy code
Interval message
Interval message
Interval message
```

**Key differences:**

* setTimeout triggers the execution of a function once after a specified delay.
* setInterval triggers the execution of a function repeatedly at a specified interval until it's explicitly stopped using clearInterval.
* It's important to note that using setInterval can lead to unexpected behavior if the execution time of the function takes longer than the interval itself. To avoid this, it's often better to use setTimeout inside the function to create a more controlled interval.

Both setTimeout and setInterval are powerful tools for scheduling tasks in JavaScript, and choosing the appropriate one depends on your specific use case.

---

> # Synchronous and Asynchronous Execution

**Synchronous Execution:** refers to the way code is executed line by line, one instruction at a time, in the order they appear. Each instruction must complete before the next one starts. In other words, each operation blocks the execution of subsequent operations until it's finished. This is how most traditional programming languages work.

Example of synchronous code:

```javascript
console.log("Step 1");
console.log("Step 2");
console.log("Step 3");
```
In this example, the code will log "Step 1", then "Step 2", and finally "Step 3" in that order, without any interruptions.

***Asynchronous Execution:*** allows multiple operations to be performed concurrently without blocking each other. This is especially useful when dealing with time-consuming tasks like network requests or file operations. Instead of waiting for an operation to complete before moving on, asynchronous code initiates an operation and continues with the next instruction without waiting.

In JavaScript, this is often achieved using callbacks, Promises, or the async/await syntax.

Example of asynchronous code using callbacks:

```javascript
console.log("Step 1");

setTimeout(() => {
    console.log("Step 2 (after 2000 ms)");
}, 2000);

console.log("Step 3");
```
In this example, "Step 1" and "Step 3" will be logged first, and then "Step 2" will be logged after a 2-second delay, even though the delay operation was initiated earlier.

***async/await*** is a way to write asynchronous code in a more synchronous style:

```javascript
async function example() {
    console.log("Step 1");

    await new Promise(resolve => setTimeout(resolve, 2000));

    console.log("Step 2 (after 2000 ms)");
    console.log("Step 3");
}

example();
```
In this example, the await keyword pauses the execution of the function until the promise (representing the 2-second delay) is resolved. This makes the code appear more like synchronous code, even though asynchronous operations are happening.

***In summary:***

* Synchronous execution processes code line by line, blocking the next operation until the current one is finished.
* Asynchronous execution allows multiple operations to be initiated and continued concurrently, often using callbacks, Promises, or async/await syntax. This is useful for non-blocking tasks like network requests and timers.
---

> ## try/catch and .then/.catch

Both ***try/catch*** and ***.then/.catch*** are used for error handling in JavaScript, but they have different syntax and are often associated with different ways of handling asynchronous operations.

* **Using try/catch:**

try/catch is used to catch errors in synchronous and asynchronous code blocks. It's particularly useful when dealing with synchronous code or asynchronous code wrapped in promises.

1. try/catch is used for both synchronous and asynchronous code.
2. It's suitable for catching errors that occur within the same block of code.
3. In synchronous code, errors can be caught directly using try and handled in the catch block.
4. In asynchronous code using async/await, try/catch can be used to catch errors within the same function.

Example with synchronous code:

```javascript
try {
    // Synchronous code that may throw an error
    const result = 10 / 0; // This will throw an error (division by zero)
} catch (error) {
    console.error("An error occurred:", error);
}
```
Example with asynchronous code wrapped in a promise:
```js
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Fetch error:', error);
    }
}

fetchData();
```

* **Using .then/.catch with Promises:**

.then and .catch are used with Promises to handle asynchronous operations. .then is used to handle the resolved value of a Promise, and .catch is used to handle any errors that occur during the Promise's execution.

1. .then/.catch are used specifically with Promises to handle asynchronous operations.
2. .then is used to handle successful (resolved) outcomes of the Promise.
3. .catch is used to handle errors that occur during the Promise's execution.

Example with Promises:
```js
fetch('https://api.example.com/data')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('Fetch error:', error);
    });
```

**In summary:**

* try/catch is used for error handling in both synchronous and asynchronous code, including async code inside async functions.
* .then/.catch are used specifically for handling asynchronous operations that return Promises. .then is used for handling resolved values, and .catch is used for handling errors.
* When working with async/await, it's common to use try/catch for error handling within the async function and within the .catch block of Promise chains.
* Both error handling approaches have their own use cases, and the choice depends on the context of your code and the style you prefer.

try/catch is a general error-handling mechanism for both synchronous and asynchronous code blocks, while .then/.catch are specific to Promises and are used to handle asynchronous operations that return Promise objects. The choice between them depends on the context of your code and whether you're dealing with synchronous or asynchronous operations.

---

> # why cant you use .catch() after another .catch()?

When you use .catch() with a Promise, it returns a new Promise. This new Promise will be in the "resolved" state if there was no error caught by the .catch(). If an error was caught by the .catch(), the new Promise will be in the "resolved" state with the value returned by the .catch().

In other words:

1. If no error occurred before the .catch(), the new Promise will be resolved with whatever value was passed to the last .then() before the .catch().
2. If an error occurred before the .catch(), the new Promise will be resolved with whatever value is returned by the .catch().

<u>When you use .catch() in a Promise chain, it's meant to handle errors that might occur in the previous part of the chain. The .catch() function itself returns a new Promise. If an error occurs before the .catch(), the .catch() will handle it and resolve the new Promise with the value you return within the .catch().

Once an error is caught and handled by a .catch(), the Promise chain continues as if the error never happened. Any subsequent .then() block receives the value that was returned from the .catch() instead of an error.

So, when you chain a .catch() after another .catch() directly, the second .catch() is handling the resolved value of the new Promise created by the first .catch(). This resolved value is not an error anymore; it's the value that you returned within the first .catch(). The second .catch() will treat this value as a regular resolved value, not an error.</u>

Here's a simple example to demonstrate this:

```js
function example() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject(new Error('Something went wrong!'));
        }, 1000);
    });
}

example()
    .then(result => console.log('Resolved:', result))
    .catch(error => {
        console.error('First .catch() caught:', error.message);
        return 'Error handled';
    })
    .catch(result => console.log('Second .catch() caught:', result));
```

In this example, the first .catch() handles the error and returns the string 'Error handled'. The second .catch() receives this value as its argument and logs 'Second .catch() caught: Error handled'.

The key takeaway is that once an error is caught and handled by a .catch(), the subsequent parts of the Promise chain treat it as a resolved value rather than an error.

---

> ##  async/await

using  along with a try/catch block is a best practice for handling asynchronous operations in JavaScript. async/await makes asynchronous code look more like synchronous code, and the try/catch block helps you handle any errors that might occur during the execution of the asynchronous code.

Here's an example that illustrates this best practice:

```js
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Fetch error:', error);
    }
}

fetchData();
```

In this example:

* The fetchData function is defined as an async function, allowing you to use await inside it.
* The try block wraps the asynchronous code where you use await.
* Inside the try block, you use await to pause the execution of the function until  the asynchronous operation (e.g., fetching data) completes.
* If an error occurs during the fetch or while processing the response, the catch block will handle the error and log an error message.
Benefits of using async/await with try/catch:

Readable Code: async/await makes asynchronous code more readable and resembles synchronous code, making it easier to understand.
Error Handling: try/catch provides a clean and centralized way to catch and handle errors that might occur during the asynchronous operations.
Avoiding Callback Hell: Combining async/await and try/catch helps you avoid nested callback structures.
By using async/await and try/catch together, you create more maintainable and robust asynchronous code that's easier to work with and debug.

---

> ## Promises

.then and .catch are methods associated with Promises in JavaScript. A Promise is an object that represents a value that might be available now, or in the future, or never. Promises are used for handling asynchronous operations in a more structured and readable way.

***Promise:*** A Promise is an object that represents the eventual completion or failure of an asynchronous operation. It has three states: pending (initial state), fulfilled (resolved with a value), and rejected (resolved with an error).

***.then:*** This method is used with Promises to attach a callback function that will be executed when the Promise is fulfilled (resolved) successfully. It receives the resolved value as an argument.

***.catch:*** This method is used with Promises to attach a callback function that will be executed when the Promise is rejected (resolved with an error). It receives the error as an argument.

***Using try/catch with Promises:***

You can use try/catch with Promises to catch synchronous errors that occur inside a Promise's .then block or when using await inside an async function. This is helpful when you're working with synchronous code inside the asynchronous context of a Promise.

***Example using try/catch with a Promise's .then:***

```js
fetch('https://api.example.com/data')
    .then(response => {
        try {
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
            return response.json();
        } catch (error) {
            console.error('An error occurred:', error);
            throw error;
        }
    })
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('Fetch error or later error:', error);
    });
```

***Using .then/.catch with Promises:***

.then and .catch methods are used with Promises to handle asynchronous operations. .then is used to handle the resolved value of a Promise, and .catch is used to handle any errors that occur during the Promise's execution.

Example using .then/.catch with Promises:
```js
fetch('https://api.example.com/data')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('Fetch error:', error);
    });
```

In this example, the .catch is specifically used to catch errors that occur during the asynchronous fetch operation. It's the preferred way to handle errors for asynchronous code that returns Promises.

**In summary:**
<u>
* try/catch is used to catch synchronous errors within asynchronous contexts (e.g., inside a .then block or an async function).
* .then/.catch methods are used to handle asynchronous operations and their outcomes (resolved values and errors) when working with Promises. They are specific to asynchronous code and Promises.
</u>

---
> ##  states of promises, how to create promise, how to promise

Promises in JavaScript represent the eventual completion (or failure) of an asynchronous operation, and they have three possible states: pending, fulfilled (resolved), and rejected.

**States of Promises:**

***Pending:*** The initial state when a Promise is created and hasn't been resolved or rejected yet.
***Fulfilled (Resolved):*** The state when the asynchronous operation has successfully completed, and the Promise's result value is available.
***Rejected:*** The state when the asynchronous operation fails or encounters an error, and the Promise contains an error reason.

**Creating a Promise:**

You can create a Promise using the Promise constructor. The constructor takes a function that receives two arguments: resolve and reject. These are functions you call to indicate whether the Promise is fulfilled (resolved) or rejected.

Here's a basic example of creating and using a Promise:
```js
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const randomNumber = Math.random();
        if (randomNumber > 0.5) {
            resolve(randomNumber); // Fulfilled with a value
        } else {
            reject(new Error("Random number is too small")); // Rejected with an error
        }
    }, 1000);
});

myPromise
.then(result => {
        console.log("Fulfilled:", result);
    })
    .catch(error => {
        console.error("Rejected:", error.message);
    });
```


**Promise Chain:**

Promises are often used in chains to perform a sequence of asynchronous operations. The .then method is used to handle the fulfilled state, and the .catch method is used to handle the rejected state. The value returned from one .then can be passed to the next .then, allowing you to chain multiple asynchronous operations together.

Example of a Promise chain:
```js
function asyncOperation(value) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(value * 2);
        }, 1000);
    });
}

asyncOperation(5)
    .then(result => {
        console.log("Result after first async operation:", result); // 10
        return asyncOperation(result);
    })
    .then(result => {
        console.log("Result after second async operation:", result); // 20
    })
    .catch(error => {
        console.error("An error occurred:", error.message);
    });
```

In this example, the result of the first asyncOperation is passed to the second one, creating a promise chain.

Promises are a powerful tool for handling asynchronous operations in a structured and manageable way. They help avoid callback hell and make your code more readable and maintainable.

---

> # fetch API

fetch API asynchronously using the async/await syntax.

***Using Fetch with Promises:***

Here's an example of using the Fetch API with Promises:

```js
fetch('https://api.example.com/data')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('Fetch error:', error);
    });
```

In this example:

* We use the fetch function to make a GET request to the specified URL.
* The response from the server is returned as a Promise.
* We use the first .then to check if the response is not OK (status code outside of 200-299 range) and throw an error if needed.
* If the response is OK, we use the second .then to parse the response body using .json() to get the actual data.
* Finally, the .catch is used to handle any errors that occur during the fetch or parsing process.
Using Fetch with async/await:

***Fetching Data Asynchronously with fetch and async/await syntax:***

```js
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Fetch error:', error);
    }
}

fetchData();
```
In this example, the fetchData function is defined with the async keyword, which means it will work asynchronously. Here's how the code works:

1. The fetch function initiates an asynchronous network request to the specified URL.
2. We use the await keyword to pause the execution of the function until the fetch operation is complete and the response is received.
3. We check if the response is not OK (status code outside of 200-299 range) and throw an error if it's not.
4. We use await again to parse the response body using the .json() method to get the actual data.
5. We log the data if everything is successful.
6. If any errors occur during the fetch or parsing, the catch block will handle them and log an error message.

Using async/await with fetch makes the asynchronous code more readable and easier to understand, as it allows you to write asynchronous code in a way that resembles synchronous code. It simplifies error handling and avoids the "callback hell" that can occur when using callbacks for asynchronous operations.

---

> # CallBack and Callback Hell
**Callbacks** are functions that are passed as arguments to other functions and are intended to be executed later, typically after an asynchronous operation completes. They are a common pattern in JavaScript for managing asynchronous code. Callbacks help ensure that specific code runs at the right time, often after a task has been completed.

Example of using a callback:

```js
function fetchData(callback) {
    // Simulating an asynchronous operation
    setTimeout(() => {
        const data = { message: "Data fetched successfully" };
        callback(data);
    }, 1000);
}

function processData(data) {
    console.log(data.message);
}

fetchData(processData); // Output: Data fetched successfully
```

In this example, fetchData is an asynchronous function that simulates fetching data. It takes a callback function (processData) as an argument and executes it after the asynchronous operation is complete.

***Callback Hell:***

Callback hell, also known as the "Pyramid of Doom," occurs when multiple asynchronous operations are nested within each other using callbacks. This leads to deeply nested and hard-to-read code, as the indentation increases with each nested level. It can become challenging to manage and debug such code.

Here's a simplified example of callback hell:
```js
getDataA(function(resultA) {
    processA(resultA, function(resultB) {
        processB(resultB, function(resultC) {
            processC(resultC, function(resultD) {
                // ... and so on
            });
        });
    });
});
```

Asynchronous operations such as fetching data, processing data, or performing any asynchronous task can quickly lead to complex and hard-to-maintain code when too many callbacks are used in this manner.

**Solution to Callback Hell:**

To mitigate callback hell and improve code readability, you can use techniques like Promises, async/await, and modularization. These modern approaches help organize and manage asynchronous code more effectively, making it easier to understand and maintain.

Example using Promises:
```js
getDataA()
    .then(processA)
    .then(processB)
    .then(processC)
    .then(resultD => {
        // ... handle resultD
    })
    .catch(error => {
        console.error("An error occurred:", error);
    });
```

Example using async/await:
```js
try {
    const resultA = await getDataA();
    const resultB = await processA(resultA);
    const resultC = await processB(resultB);
    const resultD = await processC(resultC);
    // ... handle resultD
} catch (error) {
    console.error("An error occurred:", error);
}
```

Both Promises and async/await offer a more readable and structured way to handle asynchronous operations compared to deeply nested callbacks.

---
> # The rest and spread parameters (...)

The rest and spread parameters (...) allow you to work with variable numbers of arguments in functions and arrays. The rest parameter gathers individual arguments into an array, while the spread operator unpacks elements from an array or object.

Example of using rest and spread in functions and arrays:
```js
// Rest Parameter
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // Output: 10

// Spread Operator for Arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combinedArray = [...arr1, ...arr2];

console.log(combinedArray); // Output: [1, 2, 3, 4, 5, 6]

// Spread Operator for Objects
const person = { name: 'Alice', age: 30 };
const updatedPerson = { ...person, age: 31 };

console.log(updatedPerson); // Output: { name: 'Alice', age: 31 }
```

In the examples above, the rest parameter (...numbers) gathers the individual arguments into an array in the sum function. The spread operator is used to combine arrays and objects by unpacking their elements into a new array or object.

---
> # Classes and Modules

1. **Classes:**
In JavaScript, classes provide a way to create blueprints for creating objects. They define a template for objects that share common properties and methods. Classes are a fundamental part of object-oriented programming (OOP).

Example of defining a class and creating objects from it:

```js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, my name is ${this.name} and I'm ${this.age} years old.`);
    }
}

const person1 = new Person('Alice', 30);
const person2 = new Person('Bob', 25);

person1.greet(); // Output: Hello, my name is Alice and I'm 30 years old.
person2.greet(); // Output: Hello, my name is Bob and I'm 25 years old.
```

2. **Modules:**
Modules help you organize and structure your code by allowing you to split your program into separate files. Each file can export specific values, functions, or classes, which can then be imported and used in other files.

Example of exporting and importing from modules:

```js
// person.js (Module)
class Person {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello, my name is ${this.name}.`);
    }
}

export default Person;

// main.js (Main Program)
import Person from './person.js';

const person = new Person('Alice');
person.greet(); // Output: Hello, my name is Alice.
```
---

> # call, apply and bind

1. **call:**

The call method allows you to call a function and explicitly specify the value of this that the function will have when it's executed. It also allows you to pass arguments to the function individually, one by one.

2. **apply:**

The apply method is similar to call, but it accepts an array of arguments instead of individual arguments. It also allows you to explicitly set the this value for the function when calling it.

3. **bind:**

The bind method returns a new function that has its this value permanently set to a specific object. It prepares the function to be called later with the predefined this context.

***In summary:***

* call and apply are used to immediately invoke a function with a specific this value and arguments.
* bind creates a new function with a specific this value and prepares it to be invoked later.
* These methods are useful when you need to control the value of this within a function or when you want to reuse a function with different contexts.

```js
const person = {
    name: 'Alice',
    introduce: function(age, className) {
        console.log(`Hello, my name is ${this.name} and I'm ${age} years old. I study in class ${className}.`);
    }
};

// Example 1: Using call
person.introduce.call(person, 30, 'XII');

// Example 2: Using apply
person.introduce.apply(person, [25, 'IX']);

// Example 3: Using bind
const introduceFunction = person.introduce.bind(person, 28);
introduceFunction('VII');
```
**Output:**

```js
Hello, my name is Alice and I'm 30 years old. I study in class XII.
Hello, my name is Alice and I'm 25 years old. I study in class IX.
Hello, my name is Alice and I'm 28 years old. I study in class VII.
```
In these examples:

1. ***Using call:***
   * We're calling the introduce method of the person object using call.
   * The first argument (person) is the this value inside the function.
   * The following arguments (30 and 'XII') are passed as individual arguments to the introduce function.

2. ***Using apply:***
   *    We're calling the introduce method of the person object using apply.
   *   The first argument (person) is the this value inside the function.
   *    The second argument is an array containing the arguments (25 and 'IX')     to be passed to the introduce function.

3. ***Using bind:***
   *  We're using the bind method on the introduce function to create a new function introduceFunction.
   * The first argument (person) is the this value for the new function.
   * We've partially set the arguments of the introduce function to 28 using bind.
   * When we call introduceFunction('VII'), we provide the remaining argument 'VII', and it's combined with the previously set 28 to call the introduce method.

In all cases, we're controlling the value of this and passing arguments to the introduce function using call, apply, and bind. The key difference is how arguments are passed: individually with call, as an array with apply, and partially with bind.

---

> ## bind vs closure

**Bind:**

* The bind method is used to create a new function with a fixed value for this and optionally pre-filled arguments.
* It prepares a new function that will always have the specified this value when it's invoked.
* bind is particularly useful when you want to create a function with a specific context that you can call later.

**Closure:**

* A closure is a function that "closes over" variables from its surrounding lexical scope. It captures the variables and keeps them accessible even after the outer function has finished executing.
* Closures are often used to encapsulate state and behavior within a function, creating private variables and allowing functions to "remember" their context.
* Unlike bind, closures don't explicitly set the this value; they capture the context in which they were created.

<u>While both bind and closures involve manipulating function contexts, the primary difference is that bind sets the this value for a function, while closures capture and remember variables from their lexical scope.</u>

---
> # IIFE (Immediately Invoked Function Expression)
An IIFE is a JavaScript design pattern that allows you to define and immediately execute a function. It's often used to create a private scope for variables and avoid polluting the global scope with unnecessary variables and functions.

Here's the basic structure of an IIFE:
```js
(function() {
    // Code enclosed within the IIFE's scope
})();
```

Key points about IIFE:

**Encapsulation:** An IIFE creates a new scope for the code inside it. This helps in preventing naming conflicts and keeping variables private.

**Invocation:** The function is immediately invoked by placing an extra set of parentheses () at the end. This immediately executes the function as soon as it's defined.

**No Global Pollution:** Variables declared within an IIFE are not exposed to the global scope. They are limited to the scope of the IIFE, preventing them from interfering with other code.

**Passing Arguments:** You can pass arguments to the IIFE by placing them inside the first set of parentheses.

Example of using an IIFE:
```js
(function() {
    const privateVariable = 'This is private';

    function privateFunction() {
        console.log(privateVariable);
    }

    privateFunction();
})();
// privateVariable and privateFunction are not accessible outside the IIFE
```

* In this example, the variable privateVariable and the function privateFunction are encapsulated within the IIFE's scope. They cannot be accessed from outside the IIFE, which helps in maintaining a clean and organized global scope.

IIFEs are often used in scenarios where you need to execute some code immediately without creating long-lasting variables in the global scope. They were more common before ES6 introduced block-scoped variables (let and const), but they still have their uses, especially when working with older code or when you need to explicitly create a new scope.

---

> ## Web storage (Cookies, Local Storage, Session Storage)

**Web storage** provides a way to store data on the client's side (within the browser) to persist information across sessions or pages. There are three commonly used web storage options: cookies, local storage, and session storage. Each has its own characteristics and use cases.

1. **Cookies:**
Cookies are small pieces of data stored in the user's browser. They are primarily used to store user-specific information like authentication tokens, preferences, or tracking data. Cookies have an expiration time and can be sent to the server with each request.

Example of creating a cookie:

```js
document.cookie = "username=John Doe; expires=Thu, 18 Aug 2023 12:00:00 UTC; path=/";
```

2. **Local Storage:**
Local Storage provides a larger storage space compared to cookies, and the data stored in local storage remains available even after the browser is closed. This makes it useful for storing non-sensitive, application-specific data like user preferences or cached content.

Example of using local storage:
```js
// Saving data to local storage
localStorage.setItem('username', 'Alice');
localStorage.setItem('theme', 'dark');

// Retrieving data from local storage
const username = localStorage.getItem('username');
const theme = localStorage.getItem('theme');
```

3. **Session Storage:**
Session Storage is similar to local storage, but the data stored in session storage is limited to a single session. It's accessible only within the same tab or window and is cleared when the session ends (when the tab/window is closed).

Example of using session storage:
```js
// Saving data to session storage
sessionStorage.setItem('cartItems', JSON.stringify(['item1', 'item2', 'item3']));

// Retrieving data from session storage
const cartItems = JSON.parse(sessionStorage.getItem('cartItems'));
```

***In summary:***

* Cookies: Used for small, user-specific data, and can be sent to the server with requests.
* Local Storage: Offers more storage space, persists data across sessions, and is good for application-specific data.
* Session Storage: Limited to a single session, available only within the same tab/window.
* Each web storage option has its advantages and use cases, so choose the one that best fits your requirements.

---

> # Question 2

*Write a JavaScript program that prints the numbers 1 to 5 to the console, each with a delay. The delay for printing each number should be equal to the number itself in seconds. For example, the number 1 should be printed after 1 second, the number 2 after 2 seconds, and so on. You can use the setTimeout function to introduce the delays.*

Your program should produce the following output:
```js
1 (after 1 second)
2 (after 2 seconds)
3 (after 3 seconds)
4 (after 4 seconds)
5 (after 5 seconds)
```
**Solution**
```js
function printNumber(number, delay) {
    setTimeout(() => {
        console.log(number);
    }, delay);
}

printNumber(1, 1000); // Print 1 after 1 second (1000 milliseconds)
printNumber(2, 2000); // Print 2 after 2 seconds
printNumber(3, 3000); // Print 3 after 3 seconds
printNumber(4, 4000); // Print 4 after 4 seconds
printNumber(5, 5000); // Print 5 after 5 seconds
```

```js
function printNumber(number, delay) {
    setTimeout(() => {
        console.log(number);
    }, delay);
}

printNumber(1, 1000); // Print 1 after 1 second (1000 milliseconds)
printNumber(2, 2000); // Print 2 after 2 seconds
printNumber(3, 3000); // Print 3 after 3 seconds
printNumber(4, 4000); // Print 4 after 4 seconds
printNumber(5, 5000); // Print 5 after 5 seconds
```

---

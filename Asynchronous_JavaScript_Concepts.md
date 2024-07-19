---
title: "Understanding Async Concepts in JavaScript..."
seoTitle: "Understanding Async Concepts in JavaScript"
seoDescription: "JavaScript runs in a single-threaded, synchronous manner, meaning it executes one task at a time in a sequential order."
datePublished: Fri Jul 19 2024 06:15:37 GMT+0000 (Coordinated Universal Time)
cuid: clysb3hs0000j0aig5uvz0o4w
slug: understanding-async-concepts-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721368527835/be669184-fb85-4fda-93ca-65d095a99094.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1721368626542/47ef0714-ab10-42fa-b5b9-85278ae1044d.png
tags: javascript, asynchronous, promises, callback-functions

---

### How JavaScript Executes Code

JavaScript is a single-threaded language that can only execute one task at a time. When you write JavaScript code, it's run by the JavaScript engine in a top-down manner. This means the code is executed line by line, and each line must be completed before moving on to the next. This sequential execution is what makes JavaScript synchronous by default.

### The Difference Between Synchronous and Asynchronous Code

**Synchronous code** is executed sequentially, with each line of code waiting for the previous one to finish. This can lead to delays, especially if a task takes a long time to complete, like fetching data from an API.

```powershell
console.log('Start');
console.log('End');
```

This will log:

```powershell
Start
End
```

**Asynchronous code** allows JavaScript to perform other tasks while waiting for long-running operations to complete. This helps avoid blocking the main thread and keeps the application responsive.

```powershell
console.log('Start');
setTimeout(() => {
  console.log('End');
}, 1000);
```

This will log:

```powershell
Start
End
```

after a 1-second delay.

### Ways to Make Code Asynchronous

1. **Callbacks**: Functions passed as arguments to be executed once an operation is complete.
    
2. **Promises**: Objects representing the eventual completion or failure of an asynchronous operation.
    
3. **Async/Await**: Syntactic sugar over promises that make asynchronous code look and behave like synchronous code.
    

### Web Browser APIs

Web Browser APIs provide a way to interact with the browser and perform various tasks such as manipulating the DOM, fetching data, and handling events. The browser offers these APIs and can be used to create more dynamic and interactive web applications.

Example:

* `fetch` API to make network requests.
    
* `setTimeout` to delay execution.
    

### The Event Loop

The event loop is the mechanism that JavaScript uses to handle asynchronous operations. It allows JavaScript to perform non-blocking operations by offloading tasks to the browser (like fetching data) and then picking up the results once they are ready.

When an asynchronous operation is completed, the callback associated with it is placed in the task queue. The event loop continuously checks if the call stack is empty, and if it is, it takes the first task from the queue and places it on the stack for execution.

## Moving on to Promises

### Callback Hell

Callback hell occurs when callbacks are nested within other callbacks, leading to code that is difficult to read and maintain. This usually happens when multiple asynchronous operations are performed in sequence.

```powershell
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log(finalResult);
    });
  });
});
```

### Inversion of Control in Callbacks

Inversion of control refers to the practice of passing control of the execution of your code to another function, typically a callback. This can lead to issues where you lose track of the flow of your program and make it harder to debug.

### Promises

A **Promise** is an object representing the eventual completion or failure of an asynchronous operation. Promises provide a cleaner and more robust way to handle asynchronous operations compared to callbacks.

### Creating a New Promise

You can create a new promise using the `Promise` constructor, which takes a function with `resolve` and `reject` parameters.

```powershell
let myPromise = new Promise((resolve, reject) => {
  let success = true;
  if (success) {
    resolve('Operation successful');
  } else {
    reject('Operation failed');
  }
});
```

### Promise States

* **Pending**: Initial state, neither fulfilled nor rejected.
    
* **Fulfilled**: Operation completed successfully.
    
* **Rejected**: Operation failed.
    

### Consuming an Existing Promise

To consume a promise, you use the `.then` method, which takes two arguments: a callback for when the promise is fulfilled and a callback for when it is rejected.

```powershell
myPromise.then(
  (value) => console.log(value),
  (error) => console.log(error)
);
```

### Chaining Promises with `.then`

Promises can be chained to perform a series of asynchronous operations in sequence.

```powershell
myPromise
  .then((value) => {
    console.log(value);
    return anotherPromise;
  })
  .then((newValue) => {
    console.log(newValue);
  });
```

### Handling Errors with `.catch`

The `.catch` method is used to handle errors in the promise chain.

```powershell
myPromise
  .then((value) => {
    throw new Error('Something went wrong');
  })
  .catch((error) => {
    console.log(error);
  });
```

### The `finally` Block in a Promise Chain

The `finally` method is used to execute code after the promise has been settled, regardless of whether it was fulfilled or rejected.

```powershell
myPromise
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log('Promise has been settled');
  });
```

### Error Handling in Promise Chains

When an error is thrown inside a `.then` handler, it is caught by the next `.catch` in the chain. If there is no `.catch`, the error will propagate up the chain until it is either handled or reaches the top level, resulting in an unhandled promise rejection.

### Why Must `.catch` Be Placed Towards the End of the Promise Chain?

The `.catch` method should be placed towards the end of the promise chain to ensure that any errors that occur in any of the preceding `.then` handlers are caught. If you place a `.catch` in the middle of the chain, any errors that occur after it will not be caught by that `.catch`.

### What Happens When an Error Gets Thrown Inside `.then` When There Is a `.catch`

When an error is thrown inside a `.then` handler, the error is passed down to the next `.catch` in the promise chain. This allows you to handle errors gracefully and keep your code robust.

Example:

```powershell
myPromise
  .then((value) => {
    throw new Error('Something went wrong');
  })
  .catch((error) => {
    console.log('Caught an error:', error);
  });
```

### What Happens When an Error Gets Thrown Inside `.then` When There Is No `.catch`

If an error is thrown inside a `.then` handler and there is no `.catch` in the promise chain, the error will propagate up the chain until it reaches the top level. This results in an unhandled promise rejection, which can cause the application to crash or behave unexpectedly.

### Error Handling in Promise Chains

When an error is thrown inside a `.then` handler, it is caught by the next `.catch` in the chain. If there is no `.catch`, the error will propagate up the chain until it is either handled or reaches the top level, resulting in an unhandled promise rejection.

### Consuming Multiple Promises

You can handle multiple promises in two main ways:

1. **Chaining Promises**: This is useful when the promises need to be executed in sequence.
    
    ```powershell
    firstPromise
      .then((result) => secondPromise)
      .then((result) => thirdPromise)
      .then((result) => console.log('All promises completed'));
    ```
    
2. `Promise.all`: This method takes an array of promises and returns a single promise that resolves when all the promises in the array have been resolved or rejected when any one of them rejects.
    
    ```powershell
    Promise.all([promise1, promise2, promise3])
      .then((results) => {
        console.log(results); // Array of results
      })
      .catch((error) => {
        console.log(error);
      });
    ```
    

### Promisifying Callback-based Functions

To promisify a callback-based function, you can wrap it in a promise.

Example with `setTimeout`:

```powershell
function delay(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

delay(1000).then(() => console.log('1 second delay'));
```

### Using Promise Utility Methods

* `Promise.resolve`: Creates a promise that is resolved with a given value
    
    ```powershell
    Promise.resolve('Resolved value').then((value) => console.log(value));
    ```
    
* `Promise.reject`: Creates a promise that is rejected for a given reason.
    
    ```powershell
    Promise.reject('Rejected value').catch((reason) => console.log(reason));
    ```
    
* `Promise.all`: Waits for all promises to be resolved or any to be rejected.
    
    ```powershell
    Promise.all([promise1, promise2]).then((values) => console.log(values));
    ```
    
* `Promise.allSettled`: Waits for all promises to be either resolved or rejected.
    
    ```powershell
    Promise.allSettled([promise1, promise2]).then((results) => console.log(results));
    ```
    
* `Promise.any`: Resolves when any of the promises in the array fulfills or rejects if all of the promises are rejected.
    
    ```powershell
    Promise.any([promise1, promise2]).then((value) => console.log(value));
    ```
    
* `Promise.race`: Resolves or rejects as soon as one of the promises is resolved or rejected.
    
    ```powershell
    Promise.race([promise1, promise2]).then((value) => console.log(value));
    ```
    
    **Resources:** Better understanding of concepts of Asynchronous JavaScript
    
    [Youtube Link](https://youtube.com/playlist?list=PLlasXeu85E9eWOpw9jxHOQyGMRiBZ60aX&si=Yi6R7HVpx2JJnnz5)
    
    **Note:** Understanding and practicing these concepts will give you a solid foundation in working with asynchronous code in JavaScript, and help you write more efficient and readable code.

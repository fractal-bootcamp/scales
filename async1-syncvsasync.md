# Step-by-Step Guide to Building Async vs Sync Functions in JavaScript

Follow these instructions to create and understand various async and sync operations in JavaScript.

## Example 1: Simple Synchronous vs Asynchronous Function

1. Open your JavaScript environment (e.g., a .js file or browser console).
2. Create a synchronous function:
   ```javascript
   function syncFunction() {
     console.log("Sync function executed");
   }
   ```
3. Create an asynchronous function using setTimeout:
   ```javascript
   function asyncFunction() {
     setTimeout(() => {
       console.log("Async function executed");
     }, 1000);
   }
   ```
4. Call both functions and add a console.log after:
   ```javascript
   syncFunction();
   asyncFunction();
   console.log("Code after function calls");
   ```
5. Run the code and observe the order of execution.

## Example 2: Callback Hell vs Promises

1. Create a function demonstrating callback hell:
   ```javascript
   function callbackHell() {
     setTimeout(() => {
       console.log("First callback");
       setTimeout(() => {
         console.log("Second callback");
         setTimeout(() => {
           console.log("Third callback");
         }, 1000);
       }, 1000);
     }, 1000);
   }
   ```
2. Create a function using Promises to achieve the same result:
   ```javascript
   function promiseChain() {
     return new Promise((resolve) => {
       setTimeout(() => {
         console.log("First promise");
         resolve();
       }, 1000);
     })
       .then(() => {
         return new Promise((resolve) => {
           setTimeout(() => {
             console.log("Second promise");
             resolve();
           }, 1000);
         });
       })
       .then(() => {
         return new Promise((resolve) => {
           setTimeout(() => {
             console.log("Third promise");
             resolve();
           }, 1000);
         });
       });
   }
   ```
3. Call both functions and compare the readability and structure.

## Example 3: Async/Await

1. Create an async function using the async/await syntax:
   ```javascript
   async function asyncAwaitExample() {
     console.log("Start of async function");
     
     await new Promise(resolve => setTimeout(resolve, 1000));
     console.log("After 1 second");
     
     await new Promise(resolve => setTimeout(resolve, 1000));
     console.log("After 2 seconds");
     
     console.log("End of async function");
   }
   ```
2. Call the function and add a console.log after:
   ```javascript
   asyncAwaitExample();
   console.log("Code after async function call");
   ```
3. Run the code and observe how async/await makes asynchronous code look more synchronous.

## Example 4: Parallel vs Sequential Async Operations

1. Create a function for sequential fetch operations (note: replace with actual API endpoints):
   ```javascript
   async function sequentialFetch() {
     const start = Date.now();
     const response1 = await fetch('https://api.example.com/data1');
     const response2 = await fetch('https://api.example.com/data2');
     console.log(`Sequential fetch took ${Date.now() - start} ms`);
   }
   ```
2. Create a function for parallel fetch operations:
   ```javascript
   async function parallelFetch() {
     const start = Date.now();
     const [response1, response2] = await Promise.all([
       fetch('https://api.example.com/data1'),
       fetch('https://api.example.com/data2')
     ]);
     console.log(`Parallel fetch took ${Date.now() - start} ms`);
   }
   ```
3. Call both functions and compare the execution times.

## Example 5: Error Handling in Async Operations

1. Create a function that simulates an async operation with a success/fail condition:
   ```javascript
   function simulateAsyncOperation(shouldSucceed) {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         if (shouldSucceed) {
           resolve("Operation successful");
         } else {
           reject(new Error("Operation failed"));
         }
       }, 1000);
     });
   }
   ```
2. Create an async function to handle errors:
   ```javascript
   async function handleAsyncErrors() {
     try {
       console.log(await simulateAsyncOperation(true));
       console.log(await simulateAsyncOperation(false));
     } catch (error) {
       console.error("Caught an error:", error.message);
     }
     console.log("Continuing after error");
   }
   ```
3. Call the function and observe how errors are handled.

## Example 6: Race Conditions and Promise.race()

1. Create a function that returns a promise resolving after a given timeout:
   ```javascript
   function asyncOperationWithTimeout(timeout) {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         resolve(`Operation completed in ${timeout}ms`);
       }, timeout);
     });
   }
   ```
2. Create a function using Promise.race():
   ```javascript
   async function raceExample() {
     const result = await Promise.race([
       asyncOperationWithTimeout(2000),
       asyncOperationWithTimeout(1000),
       asyncOperationWithTimeout(3000)
     ]);
     console.log(result);
   }
   ```
3. Call the function and observe which promise resolves first.

## Example 7: Async Iterators and Generators

1. Create an async generator function:
   ```javascript
   async function* asyncNumberGenerator() {
     yield await Promise.resolve(1);
     yield await Promise.resolve(2);
     yield await Promise.resolve(3);
   }
   ```
2. Create a function to use the async generator:
   ```javascript
   async function useAsyncGenerator() {
     for await (const num of asyncNumberGenerator()) {
       console.log(num);
     }
   }
   ```
3. Call the function and observe how async generators work.

## Example 8: Async Event Handling

1. Create an AsyncEventEmitter class:
   ```javascript
   class AsyncEventEmitter {
     constructor() {
       this.listeners = {};
     }

     on(event, callback) {
       if (!this.listeners[event]) {
         this.listeners[event] = [];
       }
       this.listeners[event].push(callback);
     }

     async emit(event, data) {
       if (this.listeners[event]) {
         for (const callback of this.listeners[event]) {
           await callback(data);
         }
       }
     }
   }
   ```
2. Use the AsyncEventEmitter:
   ```javascript
   const emitter = new AsyncEventEmitter();

   emitter.on('asyncEvent', async (data) => {
     await new Promise(resolve => setTimeout(resolve, 1000));
     console.log('Async event handled:', data);
   });

   emitter.emit('asyncEvent', 'test data');
   console.log('After emitting event');
   ```
3. Run the code and observe how async events are handled.

## Example 9: Async Iteration with Symbol.asyncIterator

1. Create an AsyncRange class with an async iterator:
   ```javascript
   class AsyncRange {
     constructor(start, end) {
       this.start = start;
       this.end = end;
     }

     [Symbol.asyncIterator]() {
       let current = this.start;
       const end = this.end;
       return {
         async next() {
           await new Promise(resolve => setTimeout(resolve, 1000));
           if (current <= end) {
             return { value: current++, done: false };
           }
           return { done: true };
         }
       };
     }
   }
   ```
2. Create a function to iterate over the AsyncRange:
   ```javascript
   async function iterateAsyncRange() {
     for await (const num of new AsyncRange(1, 3)) {
       console.log(num);
     }
   }
   ```
3. Call the function and observe how async iteration works.

## Example 10: Web Workers for CPU-Intensive Tasks

Note: This example typically runs in a browser environment.

1. Create a main script:
   ```javascript
   function runWorker() {
     const worker = new Worker('worker.js');
     
     worker.postMessage({ number: 42 });
     
     worker.onmessage = function(event) {
       console.log('Result from worker:', event.data);
     };
   }
   ```
2. Create a separate 'worker.js' file:
   ```javascript
   self.onmessage = function(event) {
     const number = event.data.number;
     let result = 0;
     
     // Simulate a CPU-intensive task
     for (let i = 0; i < 1000000000; i++) {
       result += Math.sqrt(number);
     }
     
     self.postMessage(result);
   };
   ```
3. Run the main script in a browser environment and observe how the worker performs the CPU-intensive task without blocking the main thread.

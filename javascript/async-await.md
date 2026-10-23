# Async and Await
1. `async` is a keyword which makes a function to return a promise and the promise may `resolve` or `reject`.
2. The promise includes an `asynchronous operation` which can take time to execute.
3. `await` keyword stops the execution and waits for the promise to resolve or reject only upto the `async function not the whole program`.
4. If the promise is resolved then the `next line of code` after async operation is executed or else an exception is thrown.

```js
async function test() {
    console.log("A");

    await new Promise(resolve => setTimeout(resolve, 1000));

    console.log("B");
}
test();
console.log("C");
//Output: A C B
```

- In the above program we observe that when `test()` is `console.log("A");` is executed and await keyword on asynchronous operation `waits` to get the promise resolve or reject.
- In the mean time the `console.log("C");` is executed and after 1 sec the promise is resolved then the `console.log("B");` is executed.

## Key Takeaway: `await` pauses the `current async function`, not the entire JavaScript program.
# Promise.withResolvers

> a static method returns an object containing a new `Promise` object and two functions to resolve or reject it, corresponding to the two parameters passed to the executor of the Promise() constructor.

```js
- Such usage can improve code readability 
- and make it easier to manage asynchronous operations, 
- especially when you need to resolve or reject the Promise 
- from different parts of your code.
```


```js
export default function promiseWithResolvers() {
  let resolve, reject;
  const promise = new Promise((res, rej) => {
    resolve = res;
    reject = rej;
  });

  return { promise, resolve, reject };
}
```


----

```js
// Usage

function delayedGreeting(name) {
  const { promise, resolve, reject } = Promise.withResolvers();

  setTimeout(() => {
    if (name) {
      resolve(`Hello, ${name}!`);
    } else {
      reject(new Error('Name is required.'));
    }
  }, 1000);

  return promise;
}

delayedGreeting('Alice')
  .then((message) => console.log(message)) // Output: Hello, Alice!
  .catch((error) => console.error(error));

delayedGreeting()
  .then((message) => console.log(message))
  .catch((error) => console.error(error)); // Output: Error: Name is required.
```
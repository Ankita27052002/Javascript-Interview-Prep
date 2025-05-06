# Promisify

> - Implement a function promisify that takes a function following the common callback-last error-first style, i.e. taking a (err, value) => ... callback as the last argument, and returns a version that returns promises.

```js
// Example function with callback as last argument

// The callback has the signature `(err, value) => any`
function foo(url, options, callback) {
  apiCall(url, options)
    .then((data) => callback(null, data))
    .catch((err) => callback(err));
}

const promisify = (func)=>{
    // Write code here...
}

const promisifiedFoo = promisify(foo);
const data = await promisifiedFoo('example.com', { foo: 1 });
```


----

```js
// SOLUTION

export default function promisify(func) {
  return function (...args) {
    // Return a new function that wraps the original function in a Promise
    return new Promise((resolve, reject) => {
      // Call the original function with the provided arguments
      // Append a callback to handle success and failure cases
      func.call(this, ...args, (err, result) =>
        err ? reject(err) : resolve(result), // If there's an error, reject the Promise; otherwise, resolve it
      );
    });
  };
}
```
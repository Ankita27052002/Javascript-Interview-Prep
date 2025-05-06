# Promise.reject

> The promise.reject() static method returns a Promise object that is rejected with a given reason.

- Promise.reject() **always wraps reason in a new Promise object**, even when reason is already a Promise.

## Solution

```js
export default function promiseReject(reason) {
  return new Promise((_, reject) => reject(reason));
}
```
> Implement a function setCancellableInterval, that acts like setInterval but instead of returning a timer ID, it returns a function that when called, cancels the interval. The setCancellableInterval function should have the exact same signature as setInterval:

```js
//Example Syntax
setCancellableInterval(callback);
setCancellableInterval(callback, delay);
setCancellableInterval(callback, delay, param1);
setCancellableInterval(callback, delay, param1, param2);
setCancellableInterval(callback, delay, param1, param2, /* … ,*/ paramN);
```

---

```js
// Solution

function setCancellableInterval(callback, delay, ...args) {
  const timerId = setInterval(callback, delay, ...args);

  return () => {
    clearInterval(timerId);
  };
}
```

---

```js
// Usage in real-time

const cancelInterval = setCancellableInterval(() => {
  console.log('This runs every second.');
}, 1000);

// Later in the code, when you want to stop it:
setTimeout(() => {
  cancelInterval(); // Cancels the interval on or after 5 seconds
  console.log('Interval cancelled.');
}, 5000);
```

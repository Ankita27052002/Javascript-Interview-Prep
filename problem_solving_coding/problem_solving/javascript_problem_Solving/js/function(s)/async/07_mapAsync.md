# mapAsync

> Implement a function mapAsync that accepts an array of items and maps each element with an asynchronous mapping function. The function should return a Promise which resolves to the mapped results.

---

```js
// Info

 * mapAsync - An async version of Array.prototype.map().
 * It applies an async callback to each item in the iterable array
 * and resolves once all promises complete, preserving the original order.
```

---

```js
// Solution 1

function mapAsync(iterable, callbackFn) {
  return new Promise((resolve, reject) => {
    const results = new Array(iterable.length); // Placeholder for final results
    let unresolved = iterable.length; // Track how many items are still processing

    // Handle empty input early (edge case)
    if (unresolved === 0) {
      resolve(results); // Nothing to do, resolve immediately with an empty array
      return;
    }

    iterable.forEach((item, index) => {
      // Apply the async callback to each item
      callbackFn(item)
        .then((value) => {
          results[index] = value; // Store the resolved value at the correct index
          unresolved -= 1; // Decrease the pending counter

          // If all have resolved, resolve the main promise
          if (unresolved === 0) {
            resolve(results);
          }
        })
        .catch((err) => reject(err)); // If any async call fails, reject the whole promise
    });
  });
}
```

---

```js
// Usage

// Simulate async API call (e.g., fetch user by ID)
const fakeFetchUser = async (userId) => {
  return new Promise((resolve) =>
    setTimeout(
      () => resolve({ id: userId, name: `User ${userId}` }),
      100 * userId
    )
  );
};

const userIds = [1, 2, 3, 4];

mapAsync(userIds, fakeFetchUser).then((users) => {
  console.log(users);

  // Output (order is preserved!):
  // [
  //   { id: 1, name: 'User 1' },
  //   { id: 2, name: 'User 2' },
  //   { id: 3, name: 'User 3' },
  //   { id: 4, name: 'User 4' }
  // ]
});
```

---

```js
//  SOLUTION 2

function mapAsync(iterable, callbackFn) {
  // .map creates an array of promises by applying callbackFn to each item
  // Promise.all waits for all promises to resolve and returns a new array of results
  return Promise.all(iterable.map(callbackFn));
}
```

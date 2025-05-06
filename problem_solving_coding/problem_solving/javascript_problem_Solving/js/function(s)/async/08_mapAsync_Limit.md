# mapAsyncLimit

> - Implement a mapAsyncLimit that takes in an optional parameter size,
> - the maximum number of ongoing async tasks so that the input array can be processed in chunks of size,
>   achieving parallelism and staying within the provided limit.
>   If size is not specified, the chunk size is unlimited.

---

```js
//INFO

* mapAsyncLimit lets you run async operations on an array, but only a limited number at a time
(ex: in chunks).

* You don't want to overload an API server.

* You want to limit memory usage.

* You need controlled concurrency instead of blasting all async calls at once.
```

---

```js
// Solution 1

async function mapAsyncLimit(iterable, callbackFn, size = Infinity) {
  const results = []; // This will hold all the final results

  // Go through the iterable array in chunks of "size"
  for (let i = 0; i < iterable.length; i += size) {
    // Slice a chunk of `size` items
    const chunk = iterable.slice(i, i + size);

    // Run all async tasks in this chunk in parallel
    const chunkResults = await Promise.all(chunk.map(callbackFn));

    // Add the chunk's results to the final results array
    results.push(...chunkResults);
  }

  return results; // Return all the results in order
}
```

---

```js
// Solution 2

function mapAsyncLimit(iterable, callbackFn, size = Infinity) {
  return new Promise((resolve, reject) => {
    const results = []; // Stores the final results
    let nextIndex = 0; // Index of the next item to start processing
    let resolved = 0; // Counter to track how many tasks have finished

    // Edge case: empty input array
    if (iterable.length === 0) {
      resolve(results);
      return;
    }

    // This async function processes one item at the given index
    async function processItem(index) {
      nextIndex++; // Prepare the next index to schedule

      try {
        const result = await callbackFn(iterable[index]); // Run the async task
        results[index] = result; // Store the result at the correct position
        resolved++; // Mark this item as resolved

        // If all tasks are done, resolve the main promise
        if (resolved === iterable.length) {
          resolve(results);
          return;
        }

        // If there are more items to process, start the next one
        if (nextIndex < iterable.length) {
          processItem(nextIndex);
        }
      } catch (err) {
        // If any task fails, reject the whole operation
        reject(err);
      }
    }

    // Start initial batch of tasks (up to the concurrency limit)
    for (let i = 0; i < Math.min(iterable.length, size); i++) {
      processItem(i);
    }
  });
}
```

---

```js
// USAGE

// Simulated async task (e.g., fetch user from server)
const fakeFetchUser = async (id) => {
  console.log(`Fetching user ${id}...`);
  await new Promise((resolve) => setTimeout(resolve, 500)); // Simulate delay
  return { id, name: `User ${id}` };
};

const userIds = [1, 2, 3, 4, 5, 6];

// Only fetch 2 users at a time
mapAsyncLimit(userIds, fakeFetchUser, 2).then((users) => {
  console.log('All users:', users);
});
```

## MapAsyncLimit

> - Implement a mapAsyncLimit that takes in an optional parameter size, 
> - the maximum number of ongoing async tasks so that the input array can be processed in chunks of size, 
> - achieving parallelism and staying within the provided limit. 
> - If size is not specified, the chunk size is unlimited.


```js
// Example Use Case

async function fetchAPI(id: number) {
  const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${id}`);
  return await res.json();
}

// Only a maximum of 2 pending requests at any one time.
// Example usage: Fetching multiple TODOs with a concurrency limit of 2
const results = await mapAsyncLimit([1, 2, 3, 4], fetchAPI, 2);
console.log(results);
```
# chunk

```js
// Solution using Slice method
export default function chunk(array, size = 1) {
  if (!Array.isArray(array) || size < 1) {
    return [];
  }

  const result = [];

  for (let i = 0; i < array.length; i += size) {
    const chunk = array.slice(i, i + size);
    result.push(chunk);
  }
  return result;
}
```

----

```js
// Reason for using i += size)

chunk([1, 2, 3, 4, 5], 2);
// Steps:
// i = 0 → chunk [1, 2]
// i = 2 → chunk [3, 4]
// i = 4 → chunk [5]
// Output: [[1, 2], [3, 4], [5]]
```

-----

```js
//  If we use i++
chunk([1, 2, 3, 4, 5], 2);

// Steps:
// i = 0 → chunk [1, 2]
// i = 1 → chunk [2, 3]
// i = 2 → chunk [3, 4]
// i = 3 → chunk [4, 5]
// i = 4 → chunk [5]
// Output: [[1, 2], [2, 3], [3, 4], [4, 5], [5]]
```


----


## Solution 2 (push method)

```js
export default function chunk(array, size = 1) {
  // Check if the input is a valid array and if size is a positive number
  if (!Array.isArray(array) || size < 1) {
    return []; // Return an empty array if input is invalid
  }

  const result = []; // Initialize the result array to store the chunks
  let chunk = []; // Temporary array to collect elements for the current chunk

  // Loop through each element of the array
  for (let i = 0; i < array.length; i++) {
    chunk.push(array[i]); // Add the current element to the chunk

    // If the chunk has reached the desired size OR it's the last element, store it in result
    if (chunk.length === size || i === array.length - 1) {
      result.push(chunk); // Push the completed chunk into the result array
      chunk = []; // Reset the chunk for the next set of elements
    }
  }

  return result; // Return the final array containing all chunks
}
```





----

```js
// Example Invoke Usage

chunk(['a', 'b', 'c', 'd']); // => [['a'], ['b'], ['c'], ['d']]
chunk([1, 2, 3, 4], 2); // => [[1, 2], [3, 4]]
chunk([1, 2, 3, 4], 3); // => [[1, 2, 3], [4]]
```
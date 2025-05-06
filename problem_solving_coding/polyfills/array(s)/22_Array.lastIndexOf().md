## Array.lastIndexOf()

**Definition**: returns the last index at which a given element can be found in the array, or -1 if it is not present.

- The array is searched backwards starting at fromIndex

```js
//syntax:
lastIndexOf(searchElement, fromIndex);
```

<strong>Approach Taken:</strong>

1. by default fromIndex param will be this.length - 1
2. if fromIndex is less than 0 then fromIndex = this.length + fromIndex
3. basic for loop, looping from reverse order
   Ex: let i = fromIndex; i>=0;i--
4. core logic is : `this[i] === searchElement` then `return i`
5. outside the for loop return -1

```js
if (!Array.prototype.customLastIndexOf) {
  Array.prototype.customLastIndexOf = function (
    searchElement,
    fromIndex = this.length - 1
  ) {
    // Handle negative indices
    if (fromIndex < 0) {
      fromIndex = this.length + fromIndex;
    }

    // Loop backwards starting from fromIndex
    for (let i = fromIndex; i >= 0; i--) {
      if (this[i] === searchElement) {
        return i;
      }
    }

    // If element not found, return -1
    return -1;
  };
}

// Tests
const arr1 = [2, 5, 9, 2];
console.log(arr1.customLastIndexOf(2)); // 3
console.log(arr1.customLastIndexOf(7)); // -1
console.log(arr1.customLastIndexOf(2, 3)); // 3
console.log(arr1.customLastIndexOf(2, 2)); // 0
console.log(arr1.customLastIndexOf(2, -2)); // 0
console.log(arr1.customLastIndexOf(2, -1)); // 3
```

----

## Solution 2:

```js
export default function customLastIndexOf(
  array,
  predicate,
  fromIndex = array.length - 1,
) {
  let index =
    fromIndex < 0
      ? Math.max(fromIndex + array.length, 0)
      : Math.min(fromIndex, array.length - 1);

  while (index >= 0) {
    if (predicate(array[index], index, array)) {
      return index;
    }
    index--;
  }
  return -1;
}
```

```js
// Examples

const arr = [5, 4, 3, 2, 1];

// Search for the last value in the array that is greater than 3 and return the index.
findLastIndex(arr, (num) => num > 3); // => 1

// Start searching from index 3 (inclusive).
findLastIndex(arr, (num) => num > 1, 3); // => 3

// Start searching from index 3 (inclusive).
findLastIndex(arr, (num) => num < 1, 3); // => -1
```

```js
// Edge Cases

const arr = [5, 4, 3, 2, 1];

// Start searching from index 2.
findLastIndex(arr, (num) => num > 2, -3); // => 2

findLastIndex(arr, (num) => num % 2 === 0, -3); // => 1

// Start from the last index if fromIndex >= array.length.
findLastIndex(arr, (num) => num > 0, 10); // => 4

// Negative and out of bounds, start searching from the first item in the array.
findLastIndex(arr, (num) => num > 2, -10); // => 0
```
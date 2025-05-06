### Lodash Difference

```js
// input example
difference([1,2,3], [2,3]);
=> [1]

difference([1,2,3,4], [2,3,1]);
=> [4]

difference([1,2,3], [2,3,1,4]);
=> []

difference([1, ,3], [1]);
=> [3]

```

```js
// Working code
function difference(array1, array2) {
  return array1.filter((item) => !array2.includes(item));
}

// Example usage
console.log(difference([1, 2, 3], [2, 3])); // [1]
console.log(difference([1, 2, 3, 4], [2, 3, 1])); // [4]
console.log(difference([1, 2, 3], [2, 3, 1, 4])); // []
console.log(difference([1, 2, 3], [1])); // [2, 3]
```

----

## Solution 2 (Best approach)

```js
function difference(array, values) {
  // Core logic is to check if valuesSet doesn't value && value!==undefined ||  i in array 
  // then push in the result array

  const result = [];
  const valuesSet = new Set(values);
  console.log("valuesSet", valuesSet); // for output, check below screenshot

  for (let i = 0; i < array.length; i++) {
    const value = array[i];

    if (!valuesSet.has(value) && (value !== undefined || i in array)) {
      result.push(value);
    }
  }
  return result;
}
```

<img src="./imagesUsed/Lodash_difference.png">

----
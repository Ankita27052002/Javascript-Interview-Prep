## Array.map()

**Definition**: creates a new array with the results of calling a provided function on every element in the calling array.

```js
//syntax:
map(callbackFn, thisArg);
```

<strong>Approach Taken:</strong>

1. maintain an new result array to return in the final.
2. perform a basic for loop, with this.length
3. `main logic is` : resultArr.push(callbackFn(arrayElement, iterationVariable, arrayItself))
4. outside the for loop return the array

```js
Array.prototype.myMap = function (callbackFn, thisArg) {
  var arr = [];
  for (var i = 0; i < this.length; i++) {
    arr.push(callbackFn.call(thisArg, this[i], i, this));
  }
  return arr;
};

const arr = [2, 3, 4, 5, 6];

const myMapArr = arr.myMap((item) => item * 5);
console.log('myMapArr', myMapArr); //[10, 15, 20, 25, 30]

```

----

## Solution 2: 

- **Preserve Sparse Array Slots Instead of Treating Them as Undefined**

Example

```js
[1, 2, , 4].myMap(square); // Output: [1, 4, NaN, 16] (wrong)
```

```js
[1, 2, , 4].myMap(square); // Output: [1, 4, , 16] (correct)
```


```js
// Solution to preserve sparse array slots

Array.prototype.myMap = function (callbackFn, thisArg) {
  // Case 1: maintain a result array
  const result = new Array(this.length); // Pre-allocate the array to maintain sparsity

  // Case 2: For loop based on this.length
  for (let i = 0; i < this.length; i++) {
    // Check if the element exists in the sparse array
    if (Object.prototype.hasOwnProperty.call(this, i)) {
      result[i] = callbackFn.call(thisArg, this[i], i, this);
    }
  }

  // Case 3: return result
  return result;
};
```
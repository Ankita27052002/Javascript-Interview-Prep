## Array.filter()

**Definition**: filtered the given array based on the logic, if the logic is true it simply filters it out.

```js
//syntax:
filter(callbackFn, thisArg);
```

<strong>Approach Taken:</strong>

1. maintain an new result array to return in the final.
2. perform a basic for loop, with this.length
3. `main logic is` : if this condition is true `callbackFn.call(array, iterationVariable, arrayItself)` then finalArr.push(arrayElement)
4. outside the for loop return the array

```js
Array.prototype.myFilter = function (callbackFn, thisArg) {
  // Case 1: Maintain a result array
  const result = [];

  // Case 2: For loop based on this.length
  for (let i = 0; i < this.length; i++) {
    // Case 2(i): Correctly pass thisArg to callbackFn.call()
    if (callbackFn.call(thisArg, this[i], i, this)) {
      // Case 2(ii): Inside if condition, result.push(this[i])
      result.push(this[i]);
    }
  }

  // Case 3: return result array
  return result;
};


const arr = [2, 3, 4, 5, 6];

const filteredArr = arr.myFilter((item) => item > 5);
console.log('filteredArr', filteredArr);
```

## Array.reduce()

**Definition**: is to return the all the elements in an array as a single value.

```js
//syntax:
reduce(callbackFn, initialValue);
```

<strong>Approach Taken:</strong>

1. declare accumulator variable with the initialValue
2. this initialValue can be undefined or any provided value
3. So in the basic for loop, check if(accumulator) then callback.call(undefined, accumulator, arraySpecificElement, iterationVariable, array)
4. else accumulator = arraySpecificElement
5. return the accumulator at the end of for loop

```js
if (!Array.prototype.myReduce) {
  Array.prototype.myReduce = function (callback, initialValue) {
    // Variable that accumulates the result
    // after performing operation one-by-one
    // on the array elements
    let accumulator = initialValue;

    for (let i = 0; i < this.length; i++) {
      // If the initialValue exists, we call
      // the callback function on the existing
      // element and store in accumulator
      if (accumulator) {
        //first argument in call sets the this context for the callback function. undefined says that it is referred to window in non-strict mode or undefined in strict mode
        accumulator = callback.call(undefined, accumulator, this[i], i, this);

        // If initialValue does not exist,
        // we assign accumulator to the
        // current element of the array
      } else {
        accumulator = this[i];
      }
    }

    // We return the overall accumulated response
    return accumulator;
  };

  // Code to calculate sum of array elements
  // using our own reduce method
  const arr = [1, 2, 3, 4];
  console.log(arr.myReduce((total, elem) => total + elem, 10)); //10

  // Code to calculate multiplication of all array elements
  console.log(arr.myReduce((prod, elem) => prod * elem)); //24
}
```

-----

## SOLUTION 2 (Edge cases scenarios)

```js
Array.prototype.myReduce = function (callbackFn, initialValue) {
  if (typeof callbackFn !== "function") {
    throw new TypeError(`${callbackFn} is not a function`);
  }
  // Case 1: Check if there is an initialValue (with the help of arguments.length > 1)
  let hasInitialValue = arguments.length > 1;

  // Case 2: Store Accumulator and Start Index variables with the help of initialValue
  // Case 2(i): Accumulator (ex: hasInitialValue ? initialValue: undefined)
  let accumulator = hasInitialValue ? initialValue : undefined;
  // Case 2 (ii): startIndex (ex: hasInitialValue ? 0 : 1)
  let startIndex = hasInitialValue ? 0 : 1;

  // Handle case where no initial value is provided and the array is empty
  if (!hasInitialValue && this.length === 0) {
    throw new TypeError("Empty array with no initial value");
  }

  // Case 3: if(!hasInitialValue)
  // If no initialValue, set accumulator to the first available element
  // ✅ If initialValue is provided, then this below for loop is skipped ❗️ totally
  if (!hasInitialValue) {
    // Case 3 (i): for loop based on this.length,
    for (let i = 0; i < this.length; i++) {
      // Case 3 (ii): Explicitly check if index exists with the help of Object.prototype.hasOwnProperty.call(this, i)
      if (Object.prototype.hasOwnProperty.call(this, i)) {
        // Case 3 (iii): store accumulator = this[i]; startIndex = i+1;
        accumulator = this[i];
        startIndex = i + 1;
        break;
      }
    }
  }

  // Case 4: Outside to the condition, perform another for loop based on this.length, with i = startIndex;
  // If initialValue is provided, then startIndex = 0
  // If initialValue is not provided, then below for loop executes from startIndex (ex: i = 1) for the second iteration
  for (let i = startIndex; i < this.length; i++) {
    // Case 4 (i): if(Object.prototype.hasOwnProperty.call(this, i)), then
    if (Object.prototype.hasOwnProperty.call(this, i)) {
      // Case 4 (ii): accumulator = callbackFn.call(accumulator, this[i], i, this)
      accumulator = callbackFn(accumulator, this[i], i, this);
    }
  }

  // Case 5: return accumulator;
  return accumulator;
};
```

```js
✅ Both the first and second for loops execute when initialValue is NOT provided.

- The first loop finds the initial accumulator.
- The second loop performs the reduction.
```
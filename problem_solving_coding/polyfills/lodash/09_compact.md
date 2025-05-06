# compact
> Q) Implement a function compact(array) that creates an array with all falsey values removed. 

```js
- The values false, null, 0, '', undefined, and NaN are falsey
```

----

## Solution

```js
export default function compact(array) {
  const result = [];

  for (let i = 0; i < array.length; i++) {
    const arrayItem = array[i];

    if (arrayItem) {
      result.push(arrayItem);
    }
  }
  return result;
}
```

----

```js
// Example usage

compact([0, 1, false, 2, '', 3, null]); // => [1, 2, 3]
compact(['hello', 123, [], {}, function () {}]); // => ['hello', 123, [], {}, function() {}]
```
# deepOmit

> - Implement a function deepOmit(obj, keys) that removes specified keys and their corresponding values from an object, including nested objects or arrays. 
> - It works recursively to traverse through the entire object structure, ensuring that all occurrences of the specified keys are removed at all levels. 
> - The function takes in an object (obj) and an array of string keys (keys).


```js
// Expected Output
deepOmit({ a: 1, b: 2, c: 3 }, ['b']); // { a: 1, c: 3 }
```

----

```js
// Input Obj and Usage
const obj = {
  a: 1,
  b: 2,
  c: {
    d: 3,
    e: 4,
  },
  f: [5, 6],
};
deepOmit(obj, ['b', 'c', 'e']); // { a: 1, f: [5, 6] }
```

----

```js
// Solution

export default function deepOmit(inputObj, keys) {
  // Handle arrays.
  if (Array.isArray(inputObj)) {
    return inputObj.map((item) => deepOmit(item, keys));
  }

  // Handle objects.
  if (isPlainObject(inputObj)) {
    const newObj = {};
    for (const key in inputObj) {
      if (!keys.includes(key)) {
        newObj[key] = deepOmit(inputObj[key], keys);
      }
    }

    return newObj;
  }

  // Other values can be returned directly.
  return inputObj;
}

function isPlainObject(value) {
  if (value == null) {
    return false;
  }

  const prototype = Object.getPrototypeOf(value);
  return prototype === null || prototype === Object.prototype;
}
```
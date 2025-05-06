# countBy

>Q) Implement a function countBy(array, iteratee) that creates an object composed of keys generated from the results of running each element of array through iteratee. The corresponding value of each key is the number of times the key was returned by iteratee.



## Solution
```js
export default function countBy(array, iteratee) {
  const resultObj = {};

  for (let arrayItem of array) {
    const key = String(iteratee(arrayItem));

    if (!Object.prototype.hasOwnProperty.call(resultObj, key)) {
      resultObj[key] = 0;
    }

    resultObj[key]++;

  }

  return resultObj;
}
```

---

## Solution 2 (nullish coalescing operator)

```js
export default function countBy(array, iteratee) {
  const result = Object.create(null);

  for (const element of array) {
    const key = String(iteratee(element));
    result[key] ??= 0;
    result[key]++;
  }

  return result;
}
```


----

```js
// Usage

countBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': 1, '6': 2 }

countBy([{ n: 3 }, { n: 5 }, { n: 3 }], (o) => o.n);
// => { '3': 2, '5': 1 }
```
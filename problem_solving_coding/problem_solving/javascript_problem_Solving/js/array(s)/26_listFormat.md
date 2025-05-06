## List Format

> - Given a list of strings, implement a function listFormat that returns the items concatenated into a single string. 
> - A common use case would be in summarizing the reactions for social media posts.


```js
// Expected Output

listFormat([]); // ''

listFormat(['Bob']); // 'Bob'
listFormat(['Bob', 'Alice']); // 'Bob and Alice'

listFormat(['Bob', 'Ben', 'Tim', 'Jane', 'John']);
// 'Bob, Ben, Tim, Jane and John'

listFormat(['Bob', 'Ben', 'Tim', 'Jane', 'John'], {
  length: 3,
}); // 'Bob, Ben, Tim and 2 others'

listFormat(['Bob', 'Ben', 'Tim', 'Jane', 'John'], {
  length: 4,
}); // 'Bob, Ben, Tim, Jane and 1 other'

listFormat(['Bob', 'Ben', 'Tim', 'Jane', 'John'], {
  length: 3,
  sorted: true,
}); // 'Ben, Bob, Jane and 2 others'

listFormat(['Bob', 'Ben', 'Tim', 'Jane', 'John', 'Bob'], {
  length: 3,
  unique: true,
}); // 'Bob, Ben, Tim and 2 others'

listFormat(['Bob', 'Ben', 'Tim', 'Jane', 'John'], {
  length: 3,
  unique: true,
}); // 'Bob, Ben, Tim and 2 others'

listFormat(['Bob', 'Ben', '', '', 'John']); // 'Bob, Ben and John'
```





```js
// SOLUTION

const SEPARATOR = ", ";
const OTHERS_SEPARATOR = " and ";
const OTHERS_LABEL = "other";

function listFormat(inputArr, options = {}) {
  // Filter falsey values.
  let items = inputArr.filter((item) => !!item);

  if (!items || items.length === 0) {
    return ""; // // Ex: inputArr is []
  }

  // No processing is needed if there's only one item.
  if (items.length === 1) {
    return items[0]; // Ex: inputArr is ['Bob']
  }

  // Sort values.
  if (options.sorted) {
    items.sort();
  }

  // Remove duplicate values.
  if (options.unique) {
    items = Array.from(new Set(items));
  }

  // Length is specified and valid.
  // options object is having length property which is user specified, items.length is inputArr items.length
  if (options.length && options.length > 0 && options.length < items.length) {

    const firstSection = items.slice(0, options.length).join(SEPARATOR); // Ex: "Bob, Ben, Tim" as single string item

    const count = items.length - options.length; // Ex: 5 - 3 = 2

    const secondSection = `${count} ${OTHERS_LABEL + (count > 1 ? "s" : "")}`; //Ex: "2 others"

    return [firstSection, secondSection].join(OTHERS_SEPARATOR);
  }

  // Case where length is not specified.
  const firstSection = items.slice(0, items.length - 1).join(SEPARATOR); // Ex: 'Bob'

  const secondSection = items[items.length - 1]; // Simply the very last element (ex: 'Alice' or 'John')

  return [firstSection, secondSection].join(OTHERS_SEPARATOR); // Ex: 'Bob and Alice'
}
```

```js
// OUTPUT

console.log(listFormat([])); // ''

console.log(listFormat(["Bob"])); // 'Bob'
console.log(listFormat(["Bob", "Alice"])); // 'Bob and Alice'

console.log(listFormat(["Bob", "Ben", "Tim", "Jane", "John"]));
// 'Bob, Ben, Tim, Jane and John'

console.log(
  listFormat(["Bob", "Ben", "Tim", "Jane", "John"], {
    length: 3,
  })
); // 'Bob, Ben, Tim and 2 others'

console.log(
  listFormat(["Bob", "Ben", "Tim", "Jane", "John"], {
    length: 4,
  })
); // 'Bob, Ben, Tim, Jane and 1 other'

console.log(
  listFormat(["Bob", "Ben", "Tim", "Jane", "John"], {
    length: 3,
    sorted: true,
  })
); // 'Ben, Bob, Jane and 2 others'

console.log(
  listFormat(["Bob", "Ben", "Tim", "Jane", "John", "Bob"], {
    length: 3,
    unique: true,
  })
); // 'Bob, Ben, Tim and 2 others'

console.log(
  listFormat(["Bob", "Ben", "Tim", "Jane", "John"], {
    length: 3,
    unique: true,
  })
); // 'Bob, Ben, Tim and 2 others'

console.log(listFormat(["Bob", "Ben", "", "", "John"])); // 'Bob, Ben and John'
```
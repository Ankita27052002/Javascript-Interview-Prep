# Lodash _get

```js 
// Syntax
get(object, path, [defaultValue]);
```


```js
// Example Object
const john = {
  profile: {
    name: { firstName: 'John', lastName: 'Doe' },
    age: 20,
    gender: 'Male',
  },
};

const jane = {
  profile: {
    age: 19,
    gender: 'Female',
  },
};

function getFirstName(user) {
  return user.profile.name.firstName;
}
```

```js
// Use cases
get(john, 'profile.name.firstName'); // 'John'
get(john, 'profile.gender'); // 'Male'
get(jane, 'profile.name.firstName'); // undefined
```

```js
// Handle Edge cases too
get({ a: [{ b: { c: 3 } }] }, 'a.0.b.c'); // 3
```
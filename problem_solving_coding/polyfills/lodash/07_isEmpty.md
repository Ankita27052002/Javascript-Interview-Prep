# isEmpty

```js
export default function isEmpty(value) {
  if (value == null) {
    return true;
  }

  if (Array.isArray(value) || typeof value === "string") {
    return value.length === 0;
  }

  if (value instanceof Map || value instanceof Set) {
    return value.size === 0;
  }

  const prototype = Object.getPrototypeOf(value);
  if (prototype === null || Object.prototype === prototype) {
    return Object.keys(value).length === 0;
  }

  return true;
}
```


-------

```js
const prototype = Object.getPrototypeOf(value);
if (prototype === null || Object.prototype === prototype) {
```

### What does it do?
- It checks whether the `value` is a **plain object** (an object that is not created from a custom prototype).
- It also ensures that `value` is not an object with a prototype chain leading to a custom prototype.

### Why is it needed?
1. **Handle Objects Correctly:**
   - The function aims to check if an object is empty (`{}`).
   - However, JavaScript allows objects to be created from prototypes other than `Object.prototype`.
   - This condition ensures only plain objects (`{}`) and objects created via `Object.create(null)` are checked.

2. **Allow Objects Created with `Object.create(null)`:**
   - Objects created using `Object.create(null)` do **not** inherit from `Object.prototype` and lack common methods like `.hasOwnProperty()`.

   ```js
   const obj = Object.create(null);
   console.log(Object.getPrototypeOf(obj)); // null
   console.log(Object.keys(obj).length === 0); // true
   ```

3. **Avoid Checking Objects with Custom Prototypes:**
   - If `value` is an instance of a class, it will have a prototype different from `Object.prototype`, and we don’t want to treat such objects as empty.
   - Example:
     ```js
     class User {
       constructor(name) {
         this.name = name;
       }
     }
     const user = new User("Alice");
     console.log(Object.getPrototypeOf(user) === Object.prototype); // false
     ```

### Summary:
- `prototype === null` → Allows objects created with `Object.create(null)`.
- `Object.prototype === prototype` → Ensures only plain objects (`{}`) are checked, excluding class instances and objects with custom prototypes.

This prevents mistakenly treating class instances or objects with custom prototypes as "empty" when they might have meaningful non-enumerable properties.
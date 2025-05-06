## call & apply

```js
// syntax
function.call(thisArg, arg1, arg2, ...);
```

- **Approach Taken for call**

1. Function.prototype.call is a function that takes context and `rest of the arguments`
2. create a unique temporary function using symbol
3. temporarily assign the function to the context (ex: context[tempFn] = this)
4. store into a variable and Invoke the function with rest of the args
5. delete the context[tempFn]
6. return the variable

```js
Function.prototype.myCall = function (context, ...args) {
  // Both globalThis and window are same
  context = context || globalThis; // Use the global object if no context is provided.

  // Create a unique temporary function name using Symbol
  const tempFn = Symbol('temp');

  // Temporarily assign the function to the context
  context[tempFn] = this;

  // Invoke the function using the context
  const result = context[tempFn](...args);

  // Clean up by deleting the temporary function reference
  delete context[tempFn];

  return result;
};

// Example:
function introduce(name, age) {
  console.log(`Hi, I am ${name}, ${age} years old from ${this.city}.`);
}

const person = {
  city: 'Los Angeles',
};

introduce.myCall(person, 'John', 30);
// Outputs: Hi, I am John, 30 years old from Los Angeles.
```

---

**Approach Taken for apply**

1. Function.prototype.call is a function that takes context and `just arguments`
2. create a unique temporary function using symbol
3. temporarily assign the function to the context (ex: context[tempFn] = this)
4. store into a variable and Invoke the function with rest of the args
5. delete the context[tempFn]
6. return the variable

```js
Function.prototype.myApply = function (context, args) {
  context = context || window; // Use the global object if no context is provided.

  // Create a unique temporary function name to avoid overriding existing properties
  const tempFn = Symbol('temp');

  // Temporarily assign the function to the context
  context[tempFn] = this;

  // Invoke the function using the context
  // We're using the spread operator here to spread the elements of the args array
  const result = context[tempFn](...args);

  // Clean up by deleting the temporary function reference
  delete context[tempFn];

  return result;
};

//Example
function greet(name, age) {
  console.log(
    `Hello, my name is ${name} and I am ${age} years old, and I live in ${this.city}.`
  );
}

const person = {
  city: 'New York',
};

greet.myApply(person, ['Alice', 25]); // Outputs: Hello, my name is Alice and I am 25 years old, and I live in New York.
```


------

## Solution 2 

```js
Function.prototype.myApply = function (thisArg, argArray = []) {
  // Create a unique Symbol to temporarily store the function
  const sym = Symbol();

  // Convert `thisArg` to an object to handle `null` and primitive values
  const wrapperObj = Object(thisArg);

  // Assign the function (`this`) to the wrapper object using the symbol key
  wrapperObj[sym] = this;

  // Call the function with the correct `this` and arguments
  const result = wrapperObj[sym](...argArray);

  // Clean up: Remove the temporary function property
  delete wrapperObj[sym];

  // Return the function result
  return result;
};
```




----

### Solution 3 - Using defineProperty (myApply)

```js
Function.prototype.myApply = function (thisArg, argArray = []) {
  // Create a unique Symbol to store the function temporarily
  const sym = Symbol();

  // Convert `thisArg` to an object (ensuring it isn't null/undefined)
  // If `thisArg` is a primitive (e.g., number, string, boolean), it gets wrapped into an object.
  const wrapperObj = Object(thisArg);

  // Define a non-enumerable property on `wrapperObj` using `sym`
  // This prevents accidental exposure of the function when iterating over properties.
  Object.defineProperty(wrapperObj, sym, {
    enumerable: false, // The property won't appear in `for...in` loops
    value: this,       // Store the function being called
  });

  // Call the function using `wrapperObj[sym]`, spreading `argArray` as function arguments
  return wrapperObj[sym](...argArray);
};
```

----

## Solution 2

```js
Function.prototype.myCall = function (thisArg, ...argArray) {
  const sym = Symbol();
  const wrapperObj = Object(thisArg);

  wrapperObj[sym] = this;

  return wrapperObj[sym](...argArray);
};
```

---

## Solution 3


```js
Function.prototype.myCall = function (thisArg, ...argArray) {
  const sym = Symbol();
  const wrapperObj = Object(thisArg);

 Object.defineProperty(wrapperObj, sym, {

  enumerable: false,
  value: this
 })

  return wrapperObj[sym](...argArray);
};
```
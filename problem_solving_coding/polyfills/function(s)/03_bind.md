## Function.prototype.bind()

**Definition**: creates a new function (when called), calls this function with its `this keyword set to the provided value`, and a given sequence of arguments preceding any provided when the new function is called.

```js
//syntax:
bind(thisArg, arg1, arg2, /* …, */ argN);
```

<strong>Approach Taken:</strong>

1. Function takes context and rest of the args as parameters
2. and returns function with more arguments (ex: there are innerArguments of the bound function)
3. and in that we are returning this.apply(param1, args.concat(innerArgs))

```js
if (!Function.prototype.customBind) {
  Function.prototype.customBind = function (context, ...args) {
    if (typeof this !== 'function') {
      throw new TypeError('Must be called on a function');
    }

    const self = this;
    return function (...innerArgs) {
      // context is the person object that we are passing as the param1, innerArgs are the ones if you pass in the boundGreet, args are the ones you pass in greet.customBind(person, ....thisArgs)
      return self.apply(context, args.concat(innerArgs));
    };
  };
}

// Example usage:
function greet(name, age, occupation) {
  console.log(
    `Hello, my name is ${name} and I am ${age} years old. I'm a ${occupation} and I live in ${this.city}.`
  );
}
const person = { city: 'New York' };
const boundGreet = greet.customBind(person, 'Alice', 30, 'Frontend Engineer');
boundGreet(); //Hello, my name is Alice and I am 30 years old. I'm a Frontend Engineer and I live in New York.
```

----
## Solution 2 


```js
Function.prototype.myBind = function (thisArg, ...argArray) {
  // Create a unique Symbol to temporarily store the function
  const sym = Symbol();

  // Convert `thisArg` to an object (handles null and primitive values)
  const wrapperObj = Object(thisArg);

  // Assign the function (`this`) to the wrapper object
  wrapperObj[sym] = this;

  // Return a bound function that calls the stored function with provided arguments
  return function (...args) {
    // Call the function with merged arguments
    const result = wrapperObj[sym](...argArray, ...args);

    // Cleanup: Remove the temporary function reference
    delete wrapperObj[sym];

    return result;
  };
};
```

----

## Solution 3 (defineProperty)

```js
Function.prototype.myBind = function (thisArg, ...argArray) {
  const sym = Symbol();

  const wrapperObj = Object(thisArg);

  Object.defineProperty(wrapperObj, sym, {
    enumerable: false,
    value: this,
  });

  return function (...args) {
    return wrapperObj[sym](...argArray, ...args);
  };
};
```

----

## Curry function

**Approach Taken**

1. start with the coding example (I mean function sum(a,b,c) which return a+b+c)
2. then pass this function sum inside another function called curry (ex: curry(sum))
3. ex: const curriedSum = curry(sum), to invoke curriedSum(1)(2)(3)
4. now start writing the logic for curry function which accepts one parameter
5. return another function with rest of args (ex: ...args)
6. if args.length >=fn.length then fn.apply(this, args)
7. else return a function with remaining args
8. return curried.apply(this, args.concat(remainingArgs))

```js
function curry(fn) {
  //fn is the sum function which has 3 arguments, so fn.length = 3
  return function curried(...args) {
    //args are the curriedSum provided arguments, in the first example it is 1, so it fails if condition
    if (args.length >= fn.length) {
      // invokes the function sum with those arguments using the apply method and returns the result
      // below will be called at final, ex: else condition does the job to concatenate the provided args with all the left over arguments until it matches with the sum function in our case
      return fn.apply(this, args);
    } else {
      // if curriedSum doesn't have enough arguments, it returns a new function that expects the remaining arguments.
      // This new function when invoked calls `curried` with both the original and new arguments concatenated together
      return function (...remainingArgs) {
        //for the first example remainingArgs is 2, and args is 1, after concatenation args will look like [1,2] and then remaining args will be 3, and post concat args will be [1,2,3]
        return curried.apply(this, args.concat(remainingArgs));
      };
    }
  };
}

// Example usage:
function sum(a, b, c) {
  // return fn.apply(this, args) will execute this below line
  return a + b + c;
}

//inside curry function, we are passing sum function, sum function.length results 3
const curriedSum = curry(sum);

//for the first example args.length is 1
console.log(curriedSum(1)(2)(3)); // Output: 6
//for the first example args.length is 2
console.log(curriedSum(1, 2)(3)); // Output: 6
//for the first example args.length is 3
console.log(curriedSum(1, 2, 3)); // Output: 6
```

-----

## Solution 2:

```js
export default function curry(func) {
  return function curried(...args) {
    // Check if we have received enough arguments to call the original function
    if (args.length >= func.length) {
      // Case 1: We have received all required arguments
      // Example: func is sum(a, b, c) and we call curried(1, 2, 3)
      // args.length = 3 (matches func.length), so we directly execute sum(1, 2, 3)
      return func.apply(this, args);
    }

    // Case 2: We have received partial arguments, so we return a function to collect more
    return (arg) =>
      arg === undefined
        ? // Case 2A: If `undefined` is explicitly passed, re-invoke curried with collected args
          // Example: curried(1, 2, undefined) should be equivalent to curried(1, 2)
          curried.apply(this, args)
        : // Case 2B: Add the new argument to args and return curried again
          // Example: curried(1)(2) returns a new function expecting the last argument
          curried.apply(this, [...args, arg]);
  };
}
```

----

## Solution 3: (Using call)

```js
export default function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.call(this, ...args);
    }

    return (arg) =>
      arg === undefined
        ? curried.call(this, ...args)
        : curried.call(this, ...args, arg);
  };
}
```

----

## Solution 4: (Using bind)

```js
export default function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    }

    return curried.bind(this, ...args);
  };
}
```
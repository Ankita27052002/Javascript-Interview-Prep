## Type Utilities



```js
export function isBoolean(value) {
  return value === true || value === false;
}

export function isNumber(value) {
  return typeof value === "number";
}

export function isNull(value) {
  return value === null;
}

export function isString(value) {
  return typeof value === "string";
}

export function isSymbol(value) {
  return typeof value === "symbol";
}

export function isUndefined(value) {
  return value === undefined;
}

export function isArray(value) {
  return Array.isArray(value);
}

export function isFunction(value) {
  return typeof value === "function";
}

export function isObject(value) {
  // Edge case: typeof null is also an object, undefined == null is also handled here
  if (value == null) {
    return false;
  }

// Edge Case: Functions are also objects
  return typeof value === "object" || typeof value === "function";
}

export function isPlainObject(value) {
  // Edge case: typeof null is also an object, undefined == null is also handled here
  if (value == null) {
    return false;
  }

  const prototype = Object.getPrototypeOf(value);
  return prototype === null || prototype === Object.prototype;
}
```
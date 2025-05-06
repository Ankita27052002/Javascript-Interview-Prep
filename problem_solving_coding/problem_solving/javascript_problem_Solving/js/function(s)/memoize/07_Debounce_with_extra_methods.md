## Debounce with cancel and flush methods

> - cancel() method to cancel pending invocations.
> - flush() method to immediately invoke any delayed invocations.

```js
// Solution

function debounce(func, wait = 0) {
  let timeoutId = null; // Holds the active timeout ID
  let context = undefined; // Context (`this`) to apply when invoking
  let argsToInvoke = undefined; // Arguments to pass when invoking

  // Clears the current timeout, if any.
  function clearTimer() {
    clearTimeout(timeoutId);
    timeoutId = null;
  }

  // Invokes the original function with stored context and arguments.
  // Only runs if there's an active timeout.
  function invoke() {
    if (timeoutId == null) return; // Nothing to do if no scheduled call
    clearTimer();
    func.apply(context, argsToInvoke);
  }

  // The debounced function that replaces the original `func`.
  // It resets the delay timer each time it's called.
  function fn(...args) {
    clearTimer(); // Reset previous timeout
    argsToInvoke = args; // Store new arguments
    context = this; // Store current context

    // Schedule the function call after `wait` ms
    timeoutId = setTimeout(() => invoke(), wait);
  }

  // Allow manual cancellation of the pending invocation
  fn.cancel = clearTimer;

  // Immediately invoke the pending function (if any)
  fn.flush = invoke;

  return fn;
}
```

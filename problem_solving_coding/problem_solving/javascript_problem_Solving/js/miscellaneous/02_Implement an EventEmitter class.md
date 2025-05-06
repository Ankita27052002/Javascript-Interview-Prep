## Implement an EventEmitter class

<strong>Approach Taken:</strong>

1. this.events is an object inside constructor function
2. on, off receives eventName and callback as arguments and we check eventName is present or not in the this.events object ex: this.events[eventName]
3. for `on` method, if there is no eventName then create an array ex: this.events[eventName] = [] and push the callback (2nd arg) inside array
4. off is to filter inside, emit is to loop inside

```js
class EventEmitter {
  constructor() {
    // Object to manage events and callbacks
    this.events = {};
  }

  // Register an event with a callback
  on(eventName, callback) {
    if (!this.events[eventName]) {
      this.events[eventName] = [];
    }
    this.events[eventName].push(callback);
  }

  // Trigger an event
  emit(eventName, ...args) {
    if (this.events[eventName]) {
      this.events[eventName].forEach((callback) => {
        callback(...args);
      });
    }
  }

  // Remove a specific callback for an event
  off(eventName, callback) {
    if (this.events[eventName]) {
      this.events[eventName] = this.events[eventName].filter(
        (cb) => cb !== callback
      );
    }
  }

  // Remove all callbacks for an event
  removeAllListeners(eventName) {
    if (this.events[eventName]) {
      delete this.events[eventName];
    }
  }
}

// Example usage:
const emitter = new EventEmitter();

// Define a couple of callback functions
function onDataReceived(data) {
  console.log('onDataReceived:', data);
}

function onAnotherDataReceived(data) {
  console.log('onAnotherDataReceived:', data);
}

// Register the callbacks for the 'data' event
emitter.on('data', onDataReceived);
emitter.on('data', onAnotherDataReceived);

// Emit the 'data' event
console.log('First emit:');
emitter.emit('data', { id: 1, message: 'Hello World' });
// Both onDataReceived and onAnotherDataReceived will be called

// Remove onDataReceived callback for the 'data' event
emitter.off('data', onDataReceived);

// Emit the 'data' event again
console.log('Second emit after removing onDataReceived:');
emitter.emit('data', { id: 2, message: 'Goodbye World' });
// Only onAnotherDataReceived will be called this time

/*
Output: If emitter.off line is `not commented`
First emit:
onDataReceived: { id: 1, message: 'Hello World' }
onAnotherDataReceived: { id: 1, message: 'Hello World' }  
Second emit after removing onDataReceived:
onAnotherDataReceived: { id: 2, message: 'Goodbye World' }
*/

/*
Output: If emitter.off line is `commented`
First emit:
onDataReceived: { id: 1, message: 'Hello World' }
onAnotherDataReceived: { id: 1, message: 'Hello World' }  
Second emit after removing onDataReceived:
onDataReceived: { id: 2, message: 'Goodbye World' }       
onAnotherDataReceived: { id: 2, message: 'Goodbye World' }
*/
```

-----

## Solution 2: (Edge Cases)

```js
export default class EventEmitter {
  constructor() {
    this.events = Object.create(null);
  }

  on(eventName, listener) {
     // Using `Object.hasOwn()` ensures we are checking only the object's own properties,
    // preventing issues if the object inherits properties from its prototype.
    if (!Object.hasOwn(this.events, eventName)) {
      this.events[eventName] = [];
    }
    this.events[eventName].push(listener);
    return this; // Supports chaining
  }

  off(eventName, listener) {
    // Step 1: Check if the event exists in the `events` object.
    // Using `Object.hasOwn()` ensures we are checking only the object's own properties,
    // preventing issues if the object inherits properties from its prototype.
    if (!Object.hasOwn(this.events, eventName)) {
      return this; // If the event does not exist, simply return `this` for chaining.
    }

    const listeners = this.events[eventName];

    // Step 2: Find the index of the provided listener in the listeners array.
    const index = listeners.findIndex((item) => item === listener);

    // Step 3: If the listener is not found, do nothing and return `this`.
    // This prevents errors when trying to remove a listener that was never added.
    if (index < 0) {
      return this;
    }

    // Step 4: Remove the listener at the found index using `splice()`.
    // This ensures only the first occurrence of the listener is removed.
    listeners.splice(index, 1);

    // Step 5: Return `this` to enable method chaining.
    return this;
  }

  emit(eventName, ...args) {
    if (
      !Array.isArray(this.events[eventName]) ||
      this.events[eventName].length === 0
    ) {
      return false; // Returns `false` if no listeners exist
    }
    this.events[eventName].forEach((listener) => listener(...args));
    return true; // Returns `true` if event was handled
  }
}
```

---

## Solution 3: (Edge Cases)

```js
export default class EventEmitter {
  constructor() {
    // Using `Object.create(null)` prevents unintended property lookups from the prototype chain.
    this.events = Object.create(null);
    
    // Unique key counter to track listener IDs.
    this.key = 0;
  }

   
 
  on(eventName, listener) {
    // Ensure the event exists; if not, initialize an empty object for it.
    if (!Object.hasOwn(this.events, eventName)) {

      //  Instead of storing listeners in an array, each listener is stored as an object property with a unique numeric key.
      this.events[eventName] = {}; 
    }

    // Assign a unique ID to the listener and store it.
    const listenerId = this.key;

    this.events[eventName][listenerId] = listener;
    
    // Increment the unique key for the next listener.
    this.key++;

    // Return an object with an `off` function that allows the listener to be removed.
    return {
      off: () => {
        delete this.events[eventName][listenerId];
      },
    };
  }

  /**
   * The listeners are stored in an object instead of an array, so we must extract them before iteration.
   */
  emit(eventName, ...args) {
    // Check if the event exists and has at least one listener.
    if (
      !Object.hasOwn(this.events, eventName) ||
      Object.keys(this.events[eventName]).length === 0
    ) {
      return false; // Return `false` if no listeners exist
    }

    // Clone the listeners object to prevent issues if listeners are modified during execution.
    const listeners = { ...this.events[eventName] };

    // Convert object values (functions) into an array and execute them.
    Object.values(listeners).forEach((listener) => {
      listener(...args);
    });

    return true; // Return `true` to indicate that event listeners were triggered.
  }
}

```
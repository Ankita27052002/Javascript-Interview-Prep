# Intersection

>Q) Implement a JavaScript function intersection(arrays) that takes multiple arrays as input and returns a new array containing the unique values that are present in all given arrays SameValueZero for equality comparisons.

## Solution
```js
export default function intersection(...arrays) {
  
  if (arrays.length === 0) {
    return []; // If no arrays are provided, return an empty array
  }

  // Create a Set from the first array (to store potential intersection values)
  const set = new Set(arrays[0]);

  // Loop through the rest of the arrays, filtering out values not present in all arrays
  for (let i = 1; i < arrays.length; i++) {
    set.forEach((value) => {
      // If the current array does not include the value, remove it from the set
      if (!arrays[i].includes(value)) {
        set.delete(value);
      }
    });
  }

  // Convert the set back to an array and return it
  return Array.from(set);
}

// === Real-time Examples ===
console.log(intersection([1, 2, 3], [2, 3, 4], [3, 4, 5])); 
// Output: [3] (only 3 is common in all three arrays)

console.log(intersection(['a', 'b', 'c'], ['b', 'c', 'd'], ['c', 'd', 'e'])); 
// Output: ['c'] (only 'c' is common in all three arrays)

console.log(intersection([10, 20, 30], [30, 40, 50])); 
// Output: [30] (only 30 is common)

console.log(intersection([1, 2, 3], [4, 5, 6])); 
// Output: [] (no common elements)

console.log(intersection()); 
// Output: [] (no input arrays, so return an empty array)
```

----

## <ins>Behind the scenes</ins>


```js
const set = new Set(arrays[0]);

set.forEach((value) => {
  if (!arrays[i].includes(value)) {
    set.delete(value);
  }
});
```
#### **What does it do?**
- It **iterates through all values in the `set`** (which initially contains elements from the first array).
- It **checks if the current array (`arrays[i]`) contains the value**.
- If the **value is NOT found in the current array**, it is **removed (`set.delete(value)`)**.

---

## **Real-Time Example 1**
### **Finding Common Elements Between Three Arrays**
```js
console.log(intersection([1, 2, 3], [2, 3, 4], [3, 4, 5]));
// Output: [3]
```

### **Step-by-Step Execution**
1️⃣ **Start with first array** `[1, 2, 3]` → Create `Set`  
   ```js
   set = {1, 2, 3};

   // I mean ---> const set = new Set(arrays[0]);
   ```

2️⃣ **Check against second array `[2, 3, 4]`**
   - `1` is **not** in `[2, 3, 4]` → **remove 1** from `set`
   - `2` **is** in `[2, 3, 4]` → **keep 2**
   - `3` **is** in `[2, 3, 4]` → **keep 3**
   ```js
   set = {2, 3};
   ```

3️⃣ **Check against third array `[3, 4, 5]`**
   - `2` is **not** in `[3, 4, 5]` → **remove 2** from `set`
   - `3` **is** in `[3, 4, 5]` → **keep 3**
   ```js
   set = {3};
   ```

✅ **Final Output:** `[3]` (Only `3` is in all arrays)

---

## **Real-Time Example 2**
### **Finding Common Letters Across Words**
```js
console.log(intersection(['a', 'b', 'c', 'd'], ['b', 'c', 'e'], ['c', 'd', 'e']));
// Output: ['c']
```

### **Step-by-Step Execution**
1️⃣ **Start with first array** `['a', 'b', 'c', 'd']`
   ```js
   set = {'a', 'b', 'c', 'd'};
   ```

2️⃣ **Check against second array `['b', 'c', 'e']`**
   - `'a'` **not** in `['b', 'c', 'e']` → **remove 'a'**
   - `'b'` **is** in `['b', 'c', 'e']` → **keep 'b'**
   - `'c'` **is** in `['b', 'c', 'e']` → **keep 'c'**
   - `'d'` **not** in `['b', 'c', 'e']` → **remove 'd'**
   ```js
   set = {'b', 'c'};
   ```

3️⃣ **Check against third array `['c', 'd', 'e']`**
   - `'b'` **not** in `['c', 'd', 'e']` → **remove 'b'**
   - `'c'` **is** in `['c', 'd', 'e']` → **keep 'c'**
   ```js
   set = {'c'};
   ```

✅ **Final Output:** `['c']` (Only `'c'` is in all arrays)

---

## **Key Takeaways**
- The `set.forEach()` loop iterates through the `set`, checking each value against the **current array**.
- If the value **does not exist** in the current array, it is **removed** (`set.delete(value)`).
- This process repeats for all arrays, ensuring only **common elements remain**.
- The final result is **converted back to an array** and returned.



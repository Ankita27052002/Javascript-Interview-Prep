## Data Merging

```js
// Input Array

const sessions = [
  { user: 8, duration: 50, equipment: ["bench"] },
  { user: 7, duration: 150, equipment: ["dumbbell"] },
  { user: 1, duration: 10, equipment: ["barbell"] },
  { user: 7, duration: 100, equipment: ["bike", "kettlebell"] },
  { user: 7, duration: 200, equipment: ["bike"] },
  { user: 2, duration: 200, equipment: ["treadmill"] },
  { user: 2, duration: 200, equipment: ["bike"] },
];
```


-----

```js
// Expected Output

// [
//   { user: 8, duration: 50, equipment: ['bench'] },
//   { user: 7, duration: 450, equipment: ['bike', 'dumbbell', 'kettlebell'] },
//   { user: 1, duration: 10, equipment: ['barbell'] },
//   { user: 2, duration: 400, equipment: ['bike', 'treadmill'] },
// ];
```
-----


```js
// SOLUTION

function mergeData(inputArr) {
  const results = [];

  // Case 1: Maintain a Map function
  const sessionsForUser = new Map();

  // Perform forEach on inputArr
  inputArr.forEach((arrItem) => {
    // Case 2: Check if user is already present inside Map Function  (🖥️ Attached Screenshot)
    if (sessionsForUser.has(arrItem.user)) {
      
      // Case 6: We can get the userSession with the help of Map Function (.get)
      const userSession = sessionsForUser.get(arrItem.user);

      // Case 7: As user is already present, we are updating the duration property
      userSession.duration += arrItem.duration;

      // Case 8: As user is already present, we have to merge all the equipments inside equipments array
      arrItem.equipment.forEach((equipment) => {
        // Case 9: As equipment is stored in the form of Set, we are adding that using (.add) method
        userSession.equipment.add(equipment);
      });
    } else {
      // Case 3: If user is not present, then mutate only equipment property to remove duplicates
      // Ex: {duration: 50, equipment: Set {'bench'}, user: 8}
      const clonedSession = {
        ...arrItem,
        equipment: new Set(arrItem.equipment), // Convert the equipment array into a Set to remove duplicates // Example: ['bench', 'bench'] -> Set {'bench'}
      };

      // Case 4: Inside the Map Function, set the user digit as "Key", and all the details as "Value"
      sessionsForUser.set(arrItem.user, clonedSession); // 🖥️ Attached Screenshot

      // Case 5: Push the clonedSession inside resultsArr
      results.push(clonedSession); // 🖥️ Attached Screenshot
    }
  });

  // Convert equipment back into array (after storing as Set initially).
  return results.map((session) => ({
    ...session,
    equipment: Array.from(session.equipment).sort(), // Maintain Sort order of equipments ex: b, d, k (in alphabetical order)
  }));
}

console.log(mergeData(sessions));
```

----

### clonedSession object
<img src="./imagesUsed/mergeSessions_01.png">

----
### resultsArr before map operation 

<img src="./imagesUsed/resultsBeforeMap_01.png">

----

### sessionForUser Key, Value Pair

<img src="./imagesUsed/sessionForUser_01.png">
<img src="./imagesUsed/sessionForUser_02.png">

----


```js
// OUTPUT

[
  {"user": 8, "duration": 50, "equipment": ["bench"]},
  {"user": 7, "duration": 450, "equipment": ["bike", "dumbbell", "kettlebell"]},
  {"user": 1, "duration": 10, "equipment": ["barbell"]},
  {"user": 2, "duration": 400, "equipment": ["bike", "treadmill"]}
]
```
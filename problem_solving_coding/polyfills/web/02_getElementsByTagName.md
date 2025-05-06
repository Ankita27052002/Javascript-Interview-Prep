# getElementsByTagName

> - a method helps to return an HTMLCollection of descendant elements within the Document/Element given a tag name


```js
//core logics
1. root.children is stored as child (inside loop)
2. child.tagName.toLowerCase() === tag
3. Maintain an array and return that array
```


```js
export default function getElementsByTagName(root, tagName) {
  const result = []; // Array to store matching elements
  const tag = tagName.toLowerCase(); // Convert tagName to lowercase for case-insensitive matching

  function traverse(node) {
    // Iterate over only element children (skipping text, comment, and other node types)
    for (let child of node.children) {
      // Check if the child's tag name matches the requested tag
      if (child.tagName.toLowerCase() === tag) {
        result.push(child); // Add matching element to result array
      }
      traverse(child); // Recursively traverse child elements
    }
  }

  traverse(root); // Start traversal from the provided root element
  return result; // Return all matching elements as an array
}
```

```html
<!-- Scenario 1: Basic HTML Structure -->
<div>
  <p>Hello</p>
  <span>World</span>
  <p>Another</p>
</div>
```
```js
// Scenario 1: Basic HTML Structure
getElementsByTagName(document.body, 'p') => Returns an array with both <p> elements.
```

----

```html
<!-- Scenario 2: Nested Elements -->
<div>
  <section>
    <p>Nested</p>
  </section>
</div>
```

```js
// Scenario 2: Nested Elements
getElementsByTagName(document.body, 'p') => Returns an array with the nested <p>.
```

----

```html
<!-- Scenario 3: Case Insensitivity -->
<div>
  <p>Case Test</p>
</div>
```

```js
// Scenario 3: Case Insensitivity
getElementsByTagName(document.body, 'p') => Works despite 'P' being uppercase in the HTML.
```

----

```html
<!-- Scenario 4: Elements Without Matching Tags -->
<div>
  <span>No Match</span>
</div>
```

```js
// Scenario 4: Elements Without Matching Tags
getElementsByTagName(document.body, 'p') => Returns an empty array since no <p> exists.
```

----

```html
<!-- Scenario 5: Ignoring Non-Element Nodes -->
<div><!-- Comment --><p>Text</p></div>
```

```js
// Scenario 5: Ignoring Non-Element Nodes
getElementsByTagName(document.body, 'p') => Returns only <p>, ignoring the comment.
```
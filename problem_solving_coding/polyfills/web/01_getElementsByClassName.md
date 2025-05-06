# getElementsByClassName

```js
- a method helps to return an HTMLCollection of descendant elements within the Document/Element
- which has the specified class names(s)
```

```js
/**
 * Checks if all elements in Set `a` exist in DOMTokenList `b`
 * (i.e., whether `a` is a subset of `b`)
 *
 * Example:
 *   isSubset(new Set(['foo', 'bar']), div.classList) 
 *   -> Returns true if div has both 'foo' and 'bar' classes
 */
function isSubset(a, b) {
  return Array.from(a).every((value) => b.contains(value)); // ✅ Fix: b is a DOMTokenList, so we use `.contains(value)`
}

/**
 * Recursively finds all elements with the given class names within the provided root element.
 *
 * Example Usage:
 *   const container = document.getElementById('root');
 *   const result = getElementsByClassName(container, 'foo bar');
 *   console.log(result); // Returns all elements that have both 'foo' and 'bar' classes
 */
export default function getElementsByClassName(element, classNames) {
  const elements = [];
  const classNamesSet = new Set(classNames.trim().split(/\s+/)); // Convert space-separated class names into a Set

  function traverse(el) {
    if (!el) {
      return;
    }

    // ✅ If element's classList contains all required class names, add it to the result
    if (isSubset(classNamesSet, el.classList)) {
      elements.push(el);
    }

    // 🔄 Recursively traverse child elements
    for (const child of el.children) {
      traverse(child);
    }
  }

  // Start traversal from the children of the root element
  for (const child of element.children) {
    traverse(child);
  }

  return elements;
}
```


----

```js
// Usage
<div class="foo bar baz">
    <span class="bar baz">Span</span>
    <p class="foo baz">Paragraph</p>
    <div class="foo bar"></div>
</div>

// Invoke
getElementsByClassName(doc.body, 'foo bar');

// [div.foo.bar.baz, div.foo.bar] <-- This is an array of elements.
```
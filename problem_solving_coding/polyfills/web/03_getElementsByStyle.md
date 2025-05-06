# getElementsByStyle

> - finds DOM elements that are rendered by the browser using the specified style


```js
//core logics
1. element.children is stored as child (inside loop)
2. computedStyles.getPropertyValue(property) === value
3. Maintain an array and return that array
```


```js
export default function getElementsByStyle(element, property, value) {
  const elements = [];

  function traverse(el) {
    if (el == null) {
      return;
    }

    // To get a CSSStyleDeclaration object containing the resolved styles of the specified element.
    const computedStyles = getComputedStyle(el);

    // Used to retrieve the value of a specific CSS property such as color, font-size, margin, etc
    if (computedStyles.getPropertyValue(property) === value) {
      elements.push(el);
    }

    for (const child of el.children) {
      traverse(child);
    }
  }

  for (const child of element.children) {
    traverse(child);
  }

  return elements;
}
```


----

### Usage:

```html
<div>
    <span style="font-size: 12px">Span</span>
    <p style="font-size: 12px">Paragraph</p>
    <blockquote style="font-size: 14px">Blockquote</blockquote>
  </div>
```
```js
getElementsByStyle(doc.body, 'font-size', '12px');
// [span, p] <-- This is an array of elements.
```

----

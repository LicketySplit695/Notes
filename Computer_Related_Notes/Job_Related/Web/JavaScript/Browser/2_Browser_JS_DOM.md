# The DOM

* The Document Object Model (DOM) is the rendered HTML page.

* DOM is made of **nodes**. 

  * Each node (a tag is a element node) is a rendered JavaScript object.



## Types of Node

* We can have 12 types of nodes. But the following are relevant ones.

  1. `document` node -- the 'entry' point to the 'DOM'

  2. element nodes -- the HTML-tags, the building blocks of the DOM tree

  3. text nodes -- all text (the contents of elements, white-spaces etc.)

  4. comment node -- comments

* `object.nodeType` gives a number based on the type of node. A few are as follows:

  <table>
    <thead>
      <tr>
        <th>Node-Type</th>
        <th>Number</th>
        <th>Comments</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Element</td>
        <td>1</td>
        <td></td>
      </tr>
      <tr>
        <td>Attribute</td>
        <td>2</td>
        <td>Deprecated</td>
      </tr>
      <tr>
        <td>Text</td>
        <td>3</td>
        <td></td>
      </tr>
      <tr>
        <td>Comment</td>
        <td>8</td>
        <td></td>
      </tr>
      <tr>
        <td>Document</td>
        <td>9</td>
        <td></td>
      </tr>
      <tr>
        <td>Document-Type</td>
        <td>10</td>
        <td></td>
      </tr>
    </tbody>
  </table>

  * The node created for `<!DOCTYPE html>` is the 'Document-type' node with number 10. A list of all node types can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType).



## Navigating in the DOM

* `document` is the entry point to the DOM.

* `<html>` is represented by the `document.documentElement` property.

* `<body>` is available as by `document.body`.

* `<head>` = `document.head`



---

#### Note

A script cannot access an element before it exists. In such situation it returns `null`.

```html
<html>
<head>
  <script>
    alert( "From HEAD: " + document.body ); // null, there's no <body> yet
  </script>
</head>

<body>
  <script>
    alert( "From BODY: " + document.body ); // HTMLBodyElement, now it exists
  </script>
</body>
</html>
```

---



### ChildNode

* Child Nodes are direct children of the parent node.

* Descendant nodes are all which are nested within a given node.

* `childNodes` lists all nodes within a node (including text nodes). The return type is `NodeList`. It's not an array but an iterable object.

  ```javascript
  document.body.childNodes
  // -> NodeList(5) etc.
  ```

* `firstChild` and `lastChild` gives access to first and last child node respectively. `hasChildNodes()` checks if a node has children.

* All the above methods are readonly.



### Sibling and Parent

* As per below example 

  ```html
  <html>
    <head></head>
    <body></body>
  </html>
  ```

  * `<body>` is the 'next' or 'right' sibling of `<head>`

  ```javascript
  console.log(document.body.parentNode === document.documentElement);   // true
  console.log(document.head.nextSibling);   // HTMLBodyElement
  console.log(document.body.previousSibling);   // HTMLHeadElement
  ```



### Navigating through elements

* The following are the properties which help us navigate through elements and <u>neglecting other types of nodes</u>:

  * `children` -- returns an (`HTMLCollection`) -- A list of child element nodes

  * `firstElementChild` and `lastElementChild`

  * `previousElementSibling` and `NextElementSibling`

  * `parentElement`




## Searching for an element



### `document.getElementById`

* `document.getElementById` returns any element present in the html file based on its 'id'.

  ```html
  <div id="x">
    <div id="x-content">
      Test Paragraph 01
    </div>
  </div>
  
  <script>
    const x = document.getElementById('x'); //  (1)
    x.style.color = 'red';  // Text color is now red.
  </script>
  ```

* Line (1) in the above code may be omitted optionally, as browsers have a global variable called `id`.

  ```javascript
  x.style.color = 'red';  // This line should be good enough
  ```

  * Unless we have a local variable having the same name.

* The `id` must be unique. Otherwise the behavior will be undefined.



### `querySelectorAll`

* The format is the following

  ```javascript
  elem.querySelectorAll(css)
  ```

  * `elem` could be `document` or any element selected using other methods.

  * It returns a `NodeList` type with all the elements that match the condition.

    ```html
    <ul id="listId">
      <li>The</li>
      <li>Test</li>
    </ul>
    <ul>
      <li>has</li>
      <li>passed</li>
    </ul>

    <script>
      const list1 = document.getElementById('listId');
      const elements = list1.querySelectorAll('li:last-child');
      
      for(const elem of elements) {
        console.log(elem.innerHTML);  // Test passed
      }
    </script>
    ```

  * Any CSS selector can be used. Pseudo-classes like `:hover` and `:active` are also supported.



### `querySelector`

* The syntax remains the same i.e. `elem.querySelector(css)`. It returns the first element that matches the given CSS selector.



### `matches`

* The syntax is as:

  ```javascript
  elem.matches(css)
  ```

  * It checks if an element matches the given css. Returns `true` or `false`.

  ```javascript
  // For the above list HTML
  const elements = document.querySelectorAll('#listId');
  
  for(const elem of elements) {
    if(elem.matches('#listId'))
      console.log(elem.innerHTML);
  }

  /*
  <li>The</li>
  <li>Test</li>
  */
  ```



### `closest`

* The syntax is `elem.closest(css)` looks for the **nearest** ancestor (parent, the parent of parent and so on) that matches the CSS selector.

  ```html
  <h1>Heading h1</h1>

  <div class="book">
    <ul class="contents">
      <li class="chapter">Chapter 1</li>
      <li class="chapter">Chapter 2</li>
    </ul>
  </div>

  <script>
    const chapter = document.querySelector('.chapter');
    console.log(chapter.closest('.contents'));      // UL
    console.log(chapter.closest('.book'));          // DIV
    
    console.log(chapter.closest('h1'));     // null -- As h1 is not an ancestor of the element.
  </script>
  ```

  * Since it returns the entire parent element, it also contains the child element.



---

### `getElementsBy*`

* We can have the following methods of this type

  * `elem.getElementsByTagName(tag)`

    * returns all element that have the given tag (eg. `div`). We can use `*` for any tags.

  * `elem.getElementsByClassName(className)`

    * returns all elements having the same CSS class.

  * `document.getElementsByName(name)`

    * returns elements with the given `name` attribute, document-wide.

* The methods return a *live* collection.

  ```html
  <div>Div 1</div>

  <script>
    let divs = document.getElementsByTagName('div');
    console.log(divs.length);   // 1
  </script>

  <div>Div 2</div>

  <script>
    console.log(divs.length);   // 2
  </script>
  ```

* In contrast methods like `querySelectorAll` return a static collection. 

  ```html
  <div>Div 1</div>

  <script>
    let divs = document.querySelectorAll('div');
    console.log(divs.length);   // 1
  </script>

  <div>Div 2</div>

  <script>
    console.log(divs.length);   // 1
  </script>
  ```

---

# More DOM Topics



## DOM Node Classes

* DOM nodes are hierarchical in nature. They follow prototype based inheritance.

  ```mermaid
  classDiagram
  object <|-- EventTarget
  EventTarget <|-- Node
  Node <|-- Text
  Node <|-- Element
  Node <|-- Comment
  Element <|-- HTMLElement
  Element <|-- SVGElement
  HTMLElement <|-- HTMLInputElement
  HTMLElement <|-- HTMLBodyElement
  HTMLElement <|-- HTMLAnchorElement
  ```

  * **EventTarget** -- root abstract class. Supports 'events'.

  * **Node** -- abstract class.

  * **Element** -- base class for DOM Elements. Provides functions like the `querySelector` etc.

  * **HTMLElement** -- base class for all HTML elements.

* An element like the `<input>` acquires the properties of the `HTMLInputElement`, `HTMLElement`, up-to `object`.

* To see the DOM node class name we can use `constructor.name`:

  ```javascript
  console.log( document.body.constructor.name );    // HTMLBodyElement
  ```

  * We can also use `instanceof` to check the inheritance.



---

#### Difference between `console.log` and `console.dir`

* For normal JS objects they are the same. For DOM elements 

  * `console.log(elem)` shows the element DOM tree.

  * `console.dir(elem)` shows the element as a DOM object, good to explore its properties.

---



### `nodeType`, `tagName` and `nodeName`

* `elem.nodeType` returns a number as discussed above. We may use `instanceof` (look at inheritance for reference) in its place for modern browsers.

* Given a DOM node, we can read its tag name from `nodeName` or `tagName` properties:

  ```javascript
  alert( document.body.nodeName ); // BODY
  alert( document.body.tagName ); // BODY
  ```

  * The difference is:

    * The `tagName` property exists only for `Element` nodes.

    * The `nodeName` is defined for any node. Thus it's same as `tagName` for elements.




### `innerHTML` and `outerHTML`

* The `innerHTML` property allows to get the HTML inside the element as a string. We can also modify it.

  ```html
  <body>
    <p>A paragraph</p>
    <div>A div</div>
  
    <script>
      console.log( document.body.innerHTML );
      document.body.innerHTML = 'The new BODY!';
    </script>
  
  </body>

  <!-- BODY - innerHTML
  <p>A paragraph</p>
  <div>A div</div>

  <script>
    console.log(document.body.innerHTML);
    document.body.innerHTML = 'The new BODY!';
  </script>
  -->
  ```

  * Invalid HTML will be fixed by browsers.

  * Overwriting the `innerHTML` or appending to it **recreates** the element. All resources are reloaded.

* Scripts can be inserted into body but can't be executed.

* The `outerHTML` property contains the full HTML of the element. That’s like `innerHTML` plus the element itself.

  ```html
  <div id="elem">Hello <b>World</b></div>
  
  <script>
    alert(elem.outerHTML); // <div id="elem">Hello <b>World</b></div>
  </script>
  ```

  * Rewriting the `outerHTML` replaces that element in the DOM. The existing element remains, but is not a part of DOM anymore.



### `nodeValue`/`data`

* For text and comment or other nodes the above properties return the content. They are the same.

  ```html
  <body>
    Hello
    <!-- Comment -->
    <script>
      let text = document.body.firstChild;
      alert(text.data); // Hello
  
      let comment = text.nextSibling;
      alert(comment.data); // Comment
    </script>
  </body>
  ```

  * We may thus use comments as metadata for the script.



### `textContent`

* Returns only the text inside the element.

  ```html
  <div id="news">
    <h1>Headline!</h1>
    <p>A paragraph</p>
  </div>
  
  <script>
    console.log(news.textContent);   // Headline! A paragraph
  </script>
  ```



## HTML attributes and node properties

* HTML tags can have attributes -- which can be user defined or a built-in. **Standard** attributes are rendered as node (or element node) properties.

  * Attribute names are case-**IN**-sensitive.

  * Their values are always strings.

  * Property names are case sensitive.

    ```html
    <body id="test" something="rubbish">
      <script>
        console.log(document.body.id);        // test
        console.log(document.body.something); // undefined
      </script>
    </body>
    ```

  * DOM property values may not always be strings. (For urls attribute is only what is mentioned in the HTML, property however contains the full route.)

  * Standard attribute for one element may not be standard for other.

* To work with any attributes (standard or non-standard), we have the following methods:

  * `elem.attributes` -- returns a collection of attributes with an iterator.

  * `elem.hasAttribute(name)` -- checks for existence.

  * `elem.getAttribute(name)` -- gets the value.

  * `elem.setAttribute(name, value)` -- sets the value.

  * `elem.removeAttribute(name)` -- removes the value


* When a standard attribute changes, the corresponding DOM property changes, and vice-versa. (Exceptions to this rule include `input.value` -- syncs from attribute to property, but not back.)

* We can use custom attributes like the following example (starting with 'data'):

  ```html
  <body data-order-state="pending">
    ...
    <script>
      console.log(document.body.dataset.orderState); // pending
    </script>
  </body>
  ```

# Modifying the DOM



## Creating an Element

We can use the following methods to create nodes:



### `document.createElement(tag)`

* Creates a new element with the given tag:

  ```javascript
  let div = document.createElement('div');
  ```



### `document.createTextNode(text)`

* Creates a text-node with the given text:

  ```javascript
  let textNode = document.createTextNode('Sample Text');
  ```



### `elem.cloneNode(deep)`

* It clones an already created node. `deep` is a boolean:

  * `true` -- clones with all attributes and child-elements (deep clone)

  * `false` -- clones without the child-elements (shallow clone)

  ```javascript
  let div1 = div.cloneNode(false);
  div1.querySelector('strong').innerHTML = 'div1';
  ```



---

* The following method is a common example of how to finish completing an element dynamically:

  ```javascript
  let div = document.createElement('div');
  div.className = 'some-css-class';
  div.innerHTML = 'A <strong>div</strong> has been created.'
  ```

---



## Adding Nodes

* Given a Node (`node`), we can insert our dynamically created node (or string) at various positions. For that we have the following functions:

  * `node.prepend(nodes or strings)` -- At the *beginning*

  * `node.before(nodes or strings)` -- *Before* the node

  * `node.after(nodes or strings)` -- *After* the node

  * `node.append(nodes or strings)` -- At the *end* of the node

  ```html
  <ol id="ol">
    <li>0</li>
    <li>1</li>
    <li>2</li>
  </ol>
  
  <script>
    ol.before('before'); // insert string "before" before <ol>
    ol.after('after'); // insert string "after" after <ol>
  
    let liFirst = document.createElement('li');
    liFirst.innerHTML = 'prepend';
    ol.prepend(liFirst); // insert liFirst at the beginning of <ol>
  
    let liLast = document.createElement('li');
    liLast.innerHTML = 'append';
    ol.append(liLast); // insert liLast at the end of <ol>
  </script>

  <!--
  before
    
    1. prepend
    2. 0
    3. 1
    4. 2
    5. append
  
  after
  -->
  ```

  * We can also add multiple nodes and text in a single call.

    ```javascript
    div.before('<p>Hello</p>', document.createElement('hr'));
    ```

    * The above text will be inserted as "text" and not as HTML.



### `elem.insertAdjacentHTML(Text/Element)`

* We can use this method to add elements in the form of strings like `innerHTML`. The syntax is as:

  ```javascript
  elem.insertAdjacentHTML(where, html)
  ```

  * 'where' is inserted as HTML. It could one of the following:

    * `beforebegin` -- inserts `html` *immediately before* `elem`

    * `afterbegin` -- inserts *into* the `elem` at the beginning

    * `beforeend` -- inserts *into* the `elem` at the end

    * `afterend` -- inserts `html` *immediately after* `elem`

  ```html
  <div id="div"></div>

  <script>
    div.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>')
    div.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>')
  </script>

  <!-- Resultant DOM
  <p>beforebegin</p>
  <div id="div">
    <p>afterbegin</p>
  </div>
  -->
  ```

* Similar to above method we have:

  * `elem.insertAdjacentText(where, text)` -- the same syntax, but a string of `text` is inserted “as text” instead of HTML,

  * `elem.insertAdjacentElement(where, elem)` – the same syntax, but inserts an element.



## Removing Nodes

* We can plain remove a node

  ```javascript
  node.remove()
  ```

* We may want to replace a node with a node or string

  ```javascript
  node.replaceWith(..nodes or strings)
  ```

  * Adding strings like `'<p>Test</p>'` will result in a text node rather than an element node.



---

### DocumentFragment

* It is an 'class' whose object can hold a list of nodes. The object is such that when we read it we only get the content objects (and not the wrapping `DocumentFragment` object).

  ```html
  <ul id="ul"></ul>

  <script>
    const doc = new DocumentFragment();

    for(let i = 0; i < 3; ++i) {
      const li = document.createElement('li');
      li.append(i)
      doc.append(li);
    }

    ul.append(doc);   // Reading the DocumentFragment object
  </script>
  
  <!-- Resulting DOM
  <ul>
    <li>0</li>
    <li>1</li>
    <li>2</li>
  </ul>
  -->
  ```

  * Note that the following can be achieved by using:

    ```javascript
    const arr = [];

    for(let i = 0; i < 3; ++i) {
      const li = document.createElement('li');
      li.append(i);
      arr.push(li);
    }

    ul.append(...arr);
    ```

---



## Old School Methods

* The following as certain old school ways of adding/deleting nodes. All the above methods return the `node`.

  * `parentElem.appendChild(node)` -- Appends `node` as the last child of `parentElem`.

  * `parentElem.insertBefore(node. nextSibling)` -- Inserts `node` before `nextSibling` into `parentElem`.

  * `parentElem.replaceChild(node, oldChild)` -- Replace `oldChild` with `node` among children of `parentElem`.

  * `parentElem.removeChild(node)` -- Removes a `node` from `parentElem`.



### `document.write`

* This method can be used to replace the entire HTML document.

  ```javascript
  document.write(html)
  ```

  ```javascript
  // Clears the entire HTML

  setTimeout(() => {
    document.write('<p>Hello</p>');
  }, 2000);
  ```

* Technically we can write `document.write` while the page is loading. In such cases the method **appends** the `html` to the document. When used otherwise it erases the entire HTML.

# Styles and Classes



We can add styles in our page by the following:

  1. Using CSS classes

  2. Use the `style` attribute

JS can work on both ways.



## `className` and `classList`

* `elem.className` gives a space separated (as we add class) list of the CSS classes (as a string) that are used with the given element.

* `elem.classList` gives an **iterable** with a list of classes that are included with an element, with methods to add/remove individual classes.

  * `elem.classList.add/remove("class")` -- Adds or removes class

  * `elem.classList.toggle("class")` -- adds class if it doesn't exist, otherwise removes it

  * `elem.classList.contains("class")` -- checks if a class is added, returns `true`/`false`

  ```html
  <body class="main page">
    <script>
      console.log(document.body.className);   // main page
    </script>
  </body>
  ```

  ```html
  <body class="main page">
    <script>
      document.body.classList.add('article');   // add-ing a pre-defined class
      console.log(document.body.className);     // main page article
    </script></body>
  ```



## `elem.style`

* Using this approach is like manipulating inside the `style` attribute of an element.
  For ex. 
 
  ```javascript
  elem.style.width = 200px;
  ```

* For multi-word properties camelCase is used:

  ```
  background-color    => elem.style.backgroundColor
  z-index             => elem.style.zIndex
  -moz-border-radius  => elem.style.MozBorderRadius 
  ```

* If we want to change the entire value of the style attribute we may use `style.cssText`

  ```html
  <div id="div">Button</div>

  <script>
    div.style.cssText=`color: red !important; <!-- flags like !important are allowed -->
      background-color: yellow;
      width: 100px;
      text-align: center;
    `;
  </script>
  ```

  * This removes all existing rules.

  * The following also performs a similar action

    ```javascript
    div.setAttribute('style', 'color: red...');
    ```

* For all above methods/properties if we don't use units it wouldn't work.



## Computed styles

* To get the value of the style that is applied on an element we use 

  * Using `elem.style.color` to get the text-color will not work. We need to use `const txtCol = getComputedStyle(elem).color`.

  ```javascript
  getComputedStyle(element, [pseudo]);
  ```

  * `element` -- Element to read the value from

  * `pseudo` -- A pseudo-element like `::before`. Empty string or no argument means no pseudo element.

  * It returns a **readonly** object with all styles and values as key-value pair.

* We must use the full property name for `getComputedStyle`. Stuffs like `getComputedStyle(elem).padding` will lead to bogus values.



---

### Computer vs resolved values

  1. Computed Styles -- values after CSS rules and inheritance is applied. We may have relative values like `height: 1em;` etc.

  2. Resolved Styles -- All values are converted to absolute value by the browser. Such post-processed values are called resolved values.

  `getComputedStyle` returns resolved value.

---



---

### Note:

* Styles applied to the `:visited` are hidden, so that malicious websites can't break privacy.

---

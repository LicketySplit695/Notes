# Basics of JavaScript



### Notes on JavaScript

* JavaScript is a <b>Dynamic, weakly typed</b> programming language which is <b>compiled at runtime</b>.

* To run JavaScript we need a *host environment* (an example for an environment is a web browser).

* A JavaScript engine (v8 for Chrome)
  * Parses the JS Code
  * Compiles to Machine Code
  * Executes the Machine Code.
  * All this is executed in a **single thread**.

* External Scripts can be added as:

    ```html
      <script src="/js/script1.js"></script>
      <script src="/js/script2.js"></script>
    ```

* If `src` is set, the script content is ignored. We must choose either an external `<script src="…">` or a regular `<script>` with code.

* HTML markups such as `<script type=...>` or `<script language=...>` are not required.

* `'use strict'` if at all used should be present at the **top** of **files** or **functions**.

* Browser consoles often don't run on 'strict' mode. We can add 'use strict' on modern browser consoles. For old browser (IE etc.) IIFE can be used.

    ```javascript
    (function() {
      'use strict';
      // ...your code here...
    })()
    ```



### Expressions and Statements

1. A <u>fragment</u> of code that **produces value** is an *expression*.

2. A *statement* is a <u>sentence</u> of expressions.

   An expression followed by a `;` is a statement. For ex:

   ```javascript
   1;
   false;
   ```

3. A *program* is a list of statements.

4. Comments can be added in JavaScript using `//` or `/**/`. Nesting of comments is not allowed.



### Basic browser interactions: `alert`, `prompt`, `confirm`

* **`alert`** 

  Shows a message with a *modal* [^1] unless 'OK' is pressed.

  ```javascript
  alert('Test');
  ```

* **`prompt`**

  Its a function that returns a value. It shows a *modal* with a **title** and an <u>optional</u> default value.

  ```
  result = prompt(title, [default]);
  ```

  There are 2 options: *OK* and *Cancel*.

* **`confirm`**

  Its a function that returns a **boolean**. A modal window is opened with a *question*, *Cancel* and *OK*.

  ```javascript
  let binaryAnswer = confirm('The Question');
  ```



[^1]: A *modal* is a window that stops the user from interacting with the web-page unless that has been dealt with.

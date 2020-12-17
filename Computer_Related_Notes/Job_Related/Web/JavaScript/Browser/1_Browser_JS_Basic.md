# Basics of the Browser Environment



* JavaScript needs a platform to run. It could be Browser, NodeJS etc. The JavaScript specification calls that a host environment.

* A host environment provides own objects and functions additional to the language core. 

  * Web browsers give a means to control web pages. 

  * Node.js provides server-side features

* Browser provides 

  1. Window

  2. BOM (Browser object Model) -- Includes things like `navigator`, `screen`, `location`, `frames`, `history`, `XMLHttpRequest` etc.

  3. DOM (Document Object Model) -- Includes `document`

  4. Plain Javascript



### Window

* Window is the "root" object. It has two roles:

  1. It is a global object for JavaScript code.

  2. It represents the “browser window” and provides methods to control it.

  ```javascript
  function sayHello() {
    alert('Hello');
  }

  // global functions are methods of the global object:
  window.sayHello();
  ```

  * And here we use it as a browser window, to see the window height:

  ```javascript
  alert(window.innerHeight);
  ```



### Document Object Model

* Document Object Model (DOM) represents all page content as objects that can be modified.

* The document object is the main “entry point” to the page. We can change or create anything on the page using it.

  ```javascript
  // change the background color to red
  document.body.style.background = 'red';
  
  // change it back after 1 second
  setTimeout(() => document.body.style.background = '', 1000);
  ```

  * Similar to DOM we also have CSSOM (CSS Object Model) but its not often used.



### Browser Object Model

* The Browser Object Model (BOM) represents additional objects provided by the browser (host environment) for working with everything except the document. For instance:

  * The navigator object provides background information about the browser and the operating system.

  * The location object allows us to read the current URL and can redirect the browser to a new URL.

  ```javascript
  alert(location.href); // shows current URL
  if (confirm('Go to Wikipedia?')) {
    location.href = 'https://wikipedia.org'; // redirect the browser to another URL
  }
  ```

  * Functions `alert/confirm/prompt` are also a part of BOM: they are directly not related to the document, but represent pure browser methods of communicating with the user.


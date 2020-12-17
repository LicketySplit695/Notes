# Flow of Control



* When a program contains more than one statement, the statements are executed from top to bottom.

* **Conditionals:** 

  * We have `if-else`, ternary and `switch` statements.

* **Loops:**

  * We have `while`, `for` and `do-while` loops.
  * `break` can be used to break out of a loop.
  * `continue` skips the execution of the loop.



### `for-of` loop

* The `for...of` statement creates a loop iterating over **iterable** objects.

  ```javascript
  const arr = [ 1, 2, 3 ];

  for(let item of arr) {
    console.log(item);
  }

  const str = 'test'

  for(let ch of str) {
    console.log(ch);
  }

  // Output
  // 't'
  // 'e'
  // 's'
  // 't'
  ```



### `for-in` loop

* The `for...in` statement iterates over all enumerable properties (in an arbitrary order) of an object that are keyed by strings (ignoring ones keyed by Symbols), including inherited enumerable properties.

  ```javascript
  const object = { a: 1, b: 2, c: 3 };
  
  for (const property in object) {
    console.log(`${property}: ${object[property]}`);
  }
  
  // expected output:
  // "a: 1"
  // "b: 2"
  // "c: 3"
  ```



### labels

* We can add labels in loops and other simple block of statements.

  ```javascript
  labelName: for (...) {
    ...
    if (some condition) break labelName;
    ...

    if (another condition) continue labelName;
  }
  ```

* `break` breaks from the loop. `continue` forces to abort the current iteration.

* `continue` can work with loops only. We can also have `break` as follows:

  ```javascript
  foo: {
    ...
    ...
    break foo;
    ...
  }
  ```

* We can't use label to jump anywhere in the script.



### Notes on switch-case

* Example of a switch case:

  ```javascript
  switch(x) {
    case 'value1':  // if (x === 'value1')
      ...
      [break]
  
    case 'value2':  // if (x === 'value2')
      ...
      [break]
  
    default:
      ...
      [break]
  }
  ```

* Any expression can be a switch/case argument

  ```javascript
  let a = "1";
  let b = 0;
  
  switch (+a) {
    case b + 1:
      alert("this runs, because +a is 1, exactly equals b+1");
      break;
  
    default:
      alert("this doesn't run");
  }
  ```

* The equality check is always strict. The values must be of the same type to match.

  ```javascript
  let arg = prompt("Enter a value?");
  switch (arg) {
    case '0':
    case '1':
      alert( 'One or zero' );
      break;
  
    case 2:
      alert( 'Never executes!' );   // prompt returns a string, which can't be strictly 2
      break;
    default:
    alert( 'An unknown value' );
  }
  ```

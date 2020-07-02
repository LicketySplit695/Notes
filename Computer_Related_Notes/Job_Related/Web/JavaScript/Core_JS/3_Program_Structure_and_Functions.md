# Program Structure

## Expressions and Statements

1. A <u>fragment</u> of code that **produces value** is an *expression*.

2. A *statement* is a <u>sentence</u> of expressions.

   An expression followed by a `;` is a statement. For ex:

   ```javascript
   1;
   false;
   ```

3. A *program* is a list of statements.

---

#### Side Effects

A *function* or a *piece of code* is said to have a <u>side effect</u> when it modifies some state values ==outside== its local environment.

In other words, it has an observable effect other than returning a value. Actions such as printing a value etc. are purely called for their side effects.

---



## Bindings

Bindings are same as *variables*.

Bindings in JS behave like *tentacles*, as they just *grasp* the values and **not** *contain* values. Binding hold the ==**reference**== of the memory location where the object was created. Note:

```javascript
let x = "Angshuman";
let y = x; 			// -> y grasps the reference of "Angshuman"	
y = 5;				// -> y now graps the reference of 5
console.log(x); 	// -> Prints Angshuman not 5
```

* A program can access only the values that it still has a reference to, i.e. if a value is no more referenced we cannot use it anymore.

* Value of an empty binding is `undefined`.
* A single let statement may define multiple bindings. The definitions must be separated by commas.



### `var`, `let` and `const`

`var ` and `let` are used for variable declaration in JavaScript but there are differences:



#### Scoping Rules

* `var` keyword are scoped to the immediate function body (hence the function scope)

* `let` variables are scoped to the immediate enclosing block denoted by { } (hence the block scope).

  A classic example is:

  ```javascript
  var funcs = [];
  for (var i = 0; i < 3; i++) {
    funcs[i] = function() {
      console.log("My value: " + i);
    };
  }
  for (var j = 0; j < 3; j++) {
    funcs[j]();
  }
  ```

  The Output is `My value: 3` three times. This is so, because when the function is executed at line 8 the value of `i` is already 3.

  

#### Hoisting

* Variables declared with `var` keyword are [hoisted](https://dev.to/godcrampy/the-secret-of-hoisting-in-javascript-egi) (initialised with `undefined` before the code is run) which means they are accessible in their enclosing scope even before they are declared.

* `let` variables are not initialized until their definition is evaluated. Accessing them before the initialization results in a `ReferenceError`. Variable said to be in "temporal dead zone" from the start of the block until the initialization is processed.

  ```javascript
  function run() {
    console.log(foo); // undefined
    var foo = "Foo";
    console.log(foo); // Foo
    console.log(bar); // ReferenceError
    let bar = "Bar";
    console.log(bar); // Bar
  }
  
  run();
  ```



#### Creating Global Object Property

At the top level, `let`, unlike `var`, does not create a property on the global object

```javascript
var foo = "Foo";  // globally scoped
let bar = "Bar"; // globally scoped

console.log(window.foo); // Foo
console.log(window.bar); // undefined
```



#### Re-declaration

In strict mode, `var` will let you re-declare the same variable in the same scope while `let` raises a `SyntaxError`.

```javascript
'use strict';
var foo = "foo1";
var foo = "foo2"; // No problem, 'foo' is replaced.

let bar = "bar1";
let bar = "bar2"; // SyntaxError: Identifier 'bar' has already been declared
```



`const` creates a constant binding as other languages. It like `let` creates a blocked scope binding.

```javascript
const x = 1;
x = 2; // TypeError: Assignment to constant variable.
```



### JavaScript Environment

* The collection of bindings and their values that exist at a given time is called the environment.

* When a program starts up, this environment contains bindings that are part of the language standard, and most of the time, it also has bindings that provide ways to interact with the surrounding system. 

  For example, in a browser, there are functions to interact with the currently loaded website and to read mouse and keyboard input.



## Flow of Control

* When a program contains more than one statement, the statements are executed from top to bottom.

* Conditionals: 

  * We have `if-else`, ternary and `switch` statements.

    A typical `switch` may look like

    ```javascript
    switch (prompt("What is the weather like?")) {
        case "rainy":
            console.log("Remember to bring an umbrella.");
            break;
        case "sunny":
            console.log("Dress lightly.");
        case "cloudy":
            console.log("Go outside.");
            break;
        default:
        	console.log("Unknown weather type!");
        	break;
    }
    ```

    

* Loops

  * We have `while`, `for` and `do-while` loops.
  * `break` can be used to break out of a loop.
  * `continue` skips the execution of the loop.



# Functions

## Defining a General Function

* A function definition is a regular binding where the value of the binding is a function.
* The function body of a function created this way must always be wrapped in
  braces, even when it consists of only a single statement.
* A return keyword without an expression after it will cause the function to return `undefined`. Functions
  that don’t have a return statement at all return `undefined`.

```javascript
const square = function(x) {
return x * x;
};
console.log(square(12));
// -> 144
```



### Scope

* For bindings defined outside of any function or block, the scope is the whole program — called *global scope*.
* Bindings created for function parameters or declared inside a function can be referenced only in that function, so they are known as local bindings.
* In pre - 2015 JavaScript, only functions created new scopes, so old-style bindings, created with the `var` keyword, are visible throughout the whole function that they appear in — or throughout the global scope, if they are not in a function.
* When multiple bindings have the same name — in that case, the innermost name overrides everyone.

```javascript
const halve = function(n) {
return n / 2;
};
let n = 10;
console.log(halve(100));
// -> 50
console.log(n);
// -> 10
```



#### Nested Scope

* Blocks and functions can be created inside other blocks and functions, producing *nested scope*.
* The set of bindings visible inside a block is determined by the place of that block in the program text. Each local scope can also see all the local scopes that contain it, and all scopes can see the global scope.



### Function as Values

* A binding that holds a function is still just a regular binding and can, if not constant, be assigned a new value.



## Declaration Notation

A function *declaration* is as follows

```javascript
console.log("The future says:", future());

function future() {
	return "You'll never have flying cars";
}
```

Points to note:

1. It doesn’t require a semicolon after the function.
2. Function declarations are not part of the regular top-to-bottom flow of control. They are conceptually moved to the top of their scope and can be used by all the code in that scope.



## Arrow Functions

An *arrow* function is as follows:

```javascript
const horn = () => {
	console.log("Toot");
};
```

Points:

1. When there is only one parameter name, we can omit the parentheses around the parameter list. 

2. If the body is a single expression, rather than a block in braces, that expression will be returned from the function. Both of the following are same

   ```javascript
   const square1 = (x) => { return x * x; };
   const square2 = x => x * x;
   ```

3. When an arrow function has no parameters at all, its parameter list is just an empty set of parentheses.

4. The difference with other function is shown in the example

   ```javascript
   function normalize() {
   	console.log(this.coords.map(n => n / this.length));
   }
   normalize.call({coords: [0, 2, 3], length: 5});
   // -> [0, 0.4, 0.6]
   ```

   To implement the above as a normal function we need the following changes

   ```javascript
   function normalize() {
       let that = this;
   	console.log(this.coords.map(function(n){ return n / that.length; });
   }
   normalize.call({coords: [0, 2, 3], length: 5});
   // -> [0, 0.4, 0.6]
   ```

   Thus we cannot refer to the `this` of the wrapping scope in a regular function defined with the function keyword. Arrow functions are different — they do not bind their own `this` but can see the `this` binding of the scope around them.



## Optional Arguments

* If we pass too many arguments, the extra ones are ignored. 

* If we pass too few arguments, the missing parameters get assigned the value undefined.

  It can be useful in the following example

  ```javascript
  function minus(a, b) {
      if (b === undefined) return -a;
      else return a - b;
  }
  console.log(minus(10));
  // -> -10
  console.log(minus(10, 5));
  // -> 5
  ```

* If we write an = operator after a parameter, followed by an expression, the value of that expression will replace the argument when it is not given as in C.



### Rest Parameters

* We can make a function accept any number of arguments using *rest parameters*.

```javascript
function max(...numbers) {
    let result = -Infinity;
    for (let number of numbers) {
    	if (number > result) result = number;
    }
    return result;
}

console.log(max(4, 1, 9, -2));
// -> 9
```

* This `...numbers` “spreads” out the array, passing its elements as separate arguments.

* If there are other parameters before it, their values aren’t part of that array.

* It can also be used as follows:

  ```javascript
  let numbers = [5, 1, 7];
  console.log(max(...numbers));
  // -> 7
  
  console.log(max(9, ...numbers, 2));
  // -> 9
  
  let words = ["never", "fully"];
  console.log(["will", ...words, "understand"]);
  // -> ["will", "never", "fully", "understand"]
  ```



## Closure

* A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). 
* In other words, a closure gives us access to an outer function’s scope from an inner function. 
* In JavaScript, closures are created every time a function is created, at function creation time and different calls can’t trample on one another's local bindings.

```javascript
function wrapValue(n) {	
    return () => n; 	// A Closure -> Function references n 	
}
let wrap1 = wrapValue(1);
let wrap2 = wrapValue(2);
console.log(wrap1());
// -> 1
console.log(wrap2());
// -> 2
```

* A function that references bindings from local scopes around it is called a closure.

* The property can be used to create function generators as

  ```javascript
  function multiplier(factor) {
  	return number => number * factor;
  }
  /*
  multiplier is called and creates an environment in which its factor parameter is bound to 2
  */
  let twice = multiplier(2);
  /*
  The function value it returns, which is stored in twice, remembers this environment
  */
  console.log(twice(5));
  // -> 10
  ```

* The following 2 points are to be noted:

  * Function values contain both the code in their **body and the environment** in which they are created.
  * When called, the function body sees **the environment in which it was created**, not the environment in which it is called.



## Functions and Side Effects

Functions are of two three types that are called for their:

1. side effects
2. return value.
3. Both the above

A *pure function* is a specific kind of value-producing function that 

* has no side effects 
* doesn’t rely on side effects from other code—for example, it doesn’t read global bindings whose value might change.

When a pure function is called with the same arguments, it always produces the same value.


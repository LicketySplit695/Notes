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

* Variables declared with `var` keyword are [hoisted](https://dev.to/godcrampy/the-secret-of-hoisting-in-javascript-egi) (initialized with `undefined` before the code is run) which means they are accessible in their enclosing scope even before they are declared.

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

* If we write an = operator after a parameter, followed by an expression, the value of that expression will replace the argument when it is not given (default parameter as in C).



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



## IIFE

* An **IIFE** (Immediately Invoked Function Expression) is a [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript) [function](https://developer.mozilla.org/en-US/docs/Glossary/function) that runs as soon as it is defined.

  ```javascript
  (function () {
      statements
  })();
  ```

* It is a design pattern which is also known as a [Self-Executing Anonymous Function](https://developer.mozilla.org/en-US/docs/Glossary/Self-Executing_Anonymous_Function) and contains two major parts:

  1. The first is the anonymous function with lexical scope enclosed within the [`Grouping Operator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Grouping) `()`. This prevents accessing variables within the IIFE idiom as well as polluting the global scope.
  2. The second part creates the immediately invoked function expression `()` through which the JavaScript engine will directly interpret the function.

* Assigning the IIFE to a variable stores the function's return value, not the function definition itself.

  ```javascript
  let result = (function () {
      let name = "Barry"; 
      return name; 
  })(); 
  // Immediately creates the output: 
  console.log(result); // "Barry"
  ```

* The function becomes a function expression which is immediately executed. The variable within the expression can not be accessed from outside it.

  ```javascript
  let result = (function () {
      var aName = "Barry";
  })();
  // Variable aName is not accessible from the outside scope
  console.log(result.aName); // TypeError: Cannot read property 'aName' of undefined
  ```



## Higher Order Function

* Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.
* Higher-order functions allow us to abstract over actions, not just values.

 Certain higher order functions are defined for arrays (mostly useful for array of objects). Some are noted with similar alternate representation and use.



### Looping through Array

`forEach()` loops though and performs an action for every element in the array.
Alternative Implementation:

```javascript
let forEach = function(array, action){
	for(let item of array)
	{
		action(item);
	}
};
forEach(["A", "B"], console.log); 
// -> A
// -> B
```

Actual Use:

```javascript
["A", "B"].forEach(l => console.log(l));
// -> A
// -> B
```



### Filtering Arrays

`filter()` creates a new array with elements that fall under a given criteria from an existing array. Alternative implementation is as:

```javascript
function filter(array, test) {
	let passed = [];
	for (let element of array) {
		if (test(element)) {
			passed.push(element);
		}
	}
	return passed;
}
console.log(filter(SCRIPTS, script => script.living));
// -> [{name: "Adlam", ...}, ...]
```

The array method can be called as:

```javascript
console.log(SCRIPTS.filter(function(item) { return item.direction == "ttb"; }));
// -> [{name: "Mongolian", ...}, ...]
```

The *item* argument is a reference to the current element in the array as filter() checks it against the *condition*. This is useful for accessing properties, in the case of objects.

If the current *item* passes the condition, it gets sent to the new array.



### Transforming with map

* The map method transforms an array by applying a function to all of its elements and building a new array from the returned values.

* The new array will have the same length as the input array, but its content will have been mapped to a new form by the function.

  ```javascript
  function map(array, transform) {
      let mapped = [];
      for (let element of array) {
      	mapped.push(transform(element));
      }
      return mapped;
  }
  let rtlScripts = SCRIPTS.filter(s => s.direction == "rtl");
  console.log(map(rtlScripts, s => s.name));
  // -> ["Adlam", "Arabic", "Imperial Aramaic", ...]
  ```

  Actual use:

  ```javascript
  console.log(rtlScripts.map(s => s.name));
  // -> ["Adlam", "Arabic", "Imperial Aramaic", ...]
  ```



### Summarizing with reduce

`reduce()` builds a value by repeatedly taking a single element from the array and combining it with the current value. Alternative representation:

```javascript
function reduce(array, combine, start) {
    let current = start;
    for (let element of array) {
    	current = combine(current, element);
    }
    return current;
}
console.log(reduce([1, 2, 3, 4], (a, b) => a + b, 0));
// -> 10
```

In actual use if the array contains at least one element, we are allowed to leave off the start argument

```javascript
console.log([1, 2, 3, 4].reduce((a, b) => a + b));
// -> 10
```

---

#### Further note on `reduce()`

The signature for the `reduce` array method in JavaScript is:

```javascript
arr.reduce(callback, initialValue);
```



##### The Accumulator example once again

```javascript
/* this is our initial value i.e. the starting point*/
const initialValue = 0;

/* numbers array */
const numbers = [5, 10, 15];

/* reducer method that takes in the accumulator and next item */
const reducer = (accumulator, item) => {
  return accumulator + item;
};

/* we give the reduce method our reducer function
  and our initial value */
const total = numbers.reduce(reducer, initialValue)
```

* our method is called `3` times because there are `3` values in our array. 
* Our accumulator begins at `0` which is our `initialValue` we passed to `reduce`. 
* On each call to the function the `item` is added to the `accumulator`. 
* The final call to the method has the `accumulator` value of `15` and `item` is `15`, `15 + 15` gives us `30` which is our final value. Remember the `reducer` method returns the `accumulator` plus the `item`.



#####  Flattening an Array Using Reduce

```javascript
const numArray = [1, 2, [3, 10, [11, 12]], [1, 2, [3, 4]], 5, 6];

function flattenArray(data) {
  // our initial value this time is a blank array
  const initialValue = [];

  // call reduce on our data
  return data.reduce((total, value) => {
    // if the value is an array then recursively call reduce
    // if the value is not an array then just concat our value
    return total.concat(Array.isArray(value) ? flattenArray(value) : value);
  }, initialValue);
}
```



##### Changing an Object Structure

```javascript
const pokemon = [
  { name: "charmander", type: "fire" },
  { name: "squirtle", type: "water" },
  { name: "bulbasaur", type: "grass" }
];

// We want
// const pokemonModified = {
// 	charmander: { type: "fire" },
// 	squirtle: { type: "water" },
// 	bulbasaur: { type: "grass" }
// };

const getMapFromArray = data =>
  data.reduce((acc, item) => {
    // add object key to our object i.e. charmander: { type: 'water' }
    acc[item.name] = { type: item.type };
    return acc;
  }, {});
getMapFromArray(pokemon)
```

---



### `some()` method

It takes a test function and tells you whether that function returns true for any of the elements in the array.
Alternative implementation

```javascript
let some = function(array, checkFunction){
    for(let item of array){
        if(checkFunction(item))
            return true;
    }
    return false;
}

console.log(some([3, 10, 18, 20], age => age >= 18));
// -> true
```

Actual implementation:

```javascript
var ages = [3, 10, 18, 20];

function checkAdult(age) {
  return age >= 18;
}

function myFunction() {
  document.getElementById("demo").innerHTML = ages.some(checkAdult);
}
```



### `findIndex()` method

It finds the first value for which the given function returns true. Like `indexOf()` , it returns -1 when no such element is found. Alternative implementation:

```javascript
let findIndex = function(array, checkFunction){
    for(let i = 0; i < array.length; ++i){
        if(checkFunction(array[i]))
            return i;
    }
    return -1;
}

console.log(findIndex([3, 10, 18, 20], age => age >= 18));
// -> 2
```

Actual implementation:

```javascript
console.log([3, 10, 18, 20].findIndex(age => age >= 18));
```


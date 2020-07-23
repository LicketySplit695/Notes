 

# Objects and Arrays

## Arrays

* An *array* is a *datatype* for storing a sequence of values. It is written as a list of values between square brackets separated by commas.
* The first index of an array in JavaScript is 0.

```javascript
let listOfNumbers = [2, 3, 5, 7, 11];
console.log(listOfNumbers[2]);
// -> 5
```



### Array Methods

* `push` and `pop` methods for arrays (adding and removing at the end):

  ```javascript
  let sequence = [1, 2, 3];
  sequence.push(4);
  console.log(sequence.push(5));
  // -> 5 (No. of elements after the push operation)
  console.log(sequence);
  // -> [1, 2, 3, 4, 5]
  console.log(sequence.pop());
  // -> 5 (The element removed)
  console.log(sequence);
  // -> [1, 2, 3, 4]
  ```


* `shift` and `unshift` methods for arrays (adding and removing from the front):

    ```javascript
console.log(sequence.shift());
    // -> 1 (The element shifted out)
    console.log(sequence);
    // -> [2, 3, 4]
    
    console.log(sequence.unshift("Test"));
    // -> 4 (The no. of elemnts after the operation)
    console.log(sequence);
    // -> [ 'Test', 2, 3, 4 ]
    ```

* `indexOf()` and `lastIndexOf()` methods:

  ```javascript
  console.log([1, 2, 3, 2, 1].indexOf(2)); // returns -1 if not found
  // -> 1
  console.log([1, 2, 3, 2, 1].lastIndexOf(2)); // Searchs from the end instead of start
  // -> 3
  console.log([1, 2, 3, 2, 1].lastIndexOf(2, 2));
  // -> 1
  ```

  Both `indexOf()` and `lastIndexOf()` has an optional second argument, which tells at which index to start searching.

* `slice()` and `concat()` methods

  ```javascript
  function remove(array, index) {
      return array.slice(0, index)	//Take from index 0 (included) to index index (not included)
          .concat(array.slice(index + 1)); // Take rest from index (index + 1) (included)
  }
  console.log(remove(["a", "b", "c", "d", "e"], 2));
  // -> ["a", "b", "d", "e"]
  ```



### Strings and their properties

* Strings are a separate datatype, they are not objects (as Arrays) and thus are **immutable**. Thus we can't add new Properties to a string

  ```javascript
  let kim = "Kim";
  kim.age = 88;
  console.log(kim.age);
  // -> undefined
  ```

* Some string properties are listed

  ```javascript
  console.log("coconuts".slice(4, 7));
  // -> nut
  
  console.log("coconut".indexOf("u"));
  // -> 5
  console.log("Coconut").indexOf("nu");
  // -> 4
  
  console.log("  okay \n ".trim());
  // -> okay (removes whitespace (spaces, newlines, tabs, and similar characters))
  
  console.log(String(6).padStart(3, "0")); // desired length of output and padding character
  // -> 006 
  console.log("LA".repeat(3));
  // -> LALALA
  
  let string = "abc";
  console.log(string.length);
  // -> 3
  console.log(string[1]);
  // -> b
  
  let sentence = "Secretarybirds specialize in stomping";
  let words = sentence.split(" ");
  console.log(words);
  // -> ["Secretarybirds", "specialize", "in", "stomping"]
  console.log(words.join(". "));
  // -> Secretarybirds. specialize. in. stomping
  ```



### Array Loops

* Like `foreach` in C# we have

  ```javascript
  for(let item of [1, 2, 3]){ console.log(item); }
  ```

*  This works not only for arrays but also for strings and some other data structures.



## Properties

* Property defines the state/characteristic of a JavaScript object.

* Property name is case sensitive.

* Almost all JavaScript values have properties. The exceptions are `null` and `undefined`.

  ```javascript
  null.length;
  // -> TypeError: null has no properties
  ```

* Ways to access a property:

  * **With a dot `.`**

    The word after the `.` is the literal name of the property. Ex. in `value.x`, `x` is the name of the property.

  * **With Square Brackets `[]`**

    The word/expression within the square brackets is **evaluated** to get the property name. Ex. in `value[x]`, the expression x is evaluated and the result is converted to a string, and used as the property name.

    * To access properties like `arrayObj.length` with bracket notation we use `arrayObj["length"]`.

    * To create or access property with a space in its name use square brackets notation as:

      ```javascript
      let testObj = { left: 1, right: 2 };
      testObj["John Doe"] = "Test";
      console.log(testObj);
      // -> { left: 1, right: 2, 'John Doe': 'Test' }
      ```

    * Dot notation can not be used with property names which are only numbers (like array indexes).

* The elements in an array are stored as the array’s properties, using numbers as property names.



## Methods

* Properties that contain functions are generally called methods

```javascript
let doh = "Doh";
console.log(typeof doh.toUpperCase);
// -> function
console.log(doh.toUpperCase());
// -> DOH
```



## Objects

* Values of the type object are a **set** of properties.

* Objects can be created as:

  ```javascript
  let testObj = {
      left: 1,
      right: 2,
      "John Doe": "Testing"
  }
  ```

  * Properties whose names aren’t valid binding names or valid numbers have to be quoted ("John Doe" in the example).
  * Reading a property that doesn’t exist will gives `undefined`.

* Arrays are just a kind of object specialized for storing sequences of things. If we evaluate `typeof []`, it produces `object`.

* It's possible to create a new property or reassign a new value to an existing property by:

  ```javascript
  testObj.newProperty = "RandomValue";
  console.log(testObj);
  // -> { left: 1, right: 2, 'John Doe': 'Testing', newProperty: 'RandomValue' }
  ```

* Properties grasp on to values as variables (bindings) do in JavaScript.

* `delete` can be used to remove a named property in an object

  ```javascript
  delete testObj["John Doe"];
  console.log(testObj["John Doe"]);
  // -> undefined
  ```

* To check if a string is a valid property name (even if the property has `undefined` value) of an object we use `in` operator:

  ```javascript
  console.log(["John Doe"] in testObj);
  // -> false
  ```

* `Object.keys()` returns an array of property name.

  ```javascript
  console.log(Object.keys({x: 0, y: 0, z: 2}));
  // -> ["x", "y", "z"]
  ```

*  `Object.assign()` copies all properties from one object into another.

  ```javascript
  let objectA = {a: 1, b: 2};
  Object.assign(objectA, {b: 3, c: 4});
  console.log(objectA);
  // -> {a: 1, b: 3, c: 4}
  ```



### Mutability

* Numbers, strings, and Booleans, are all **immutable** — it is impossible to change values of those types.

* Objects (and arrays) are mutable - we can change their properties, causing a single object value to have different content at different times.

* Note that:

  ```javascript
  let object1 = {value: 10};
  let object2 = object1;
  let object3 = {value: 10};
  
  console.log(object1 == object2);
  // -> true
  console.log(object1 == object3);
  // -> false
  
  object1.value = 15;
  console.log(object2.value);
  // -> 15
  console.log(object3.value);
  // -> 10
  ```

  * Since the `object1` and `object2` bindings grasp the same object, they are said to have the same identity.

* Although a `const` binding to an object can itself not be changed and <u>will continue to point at the same object</u>, the contents of that object might change.

  ```javascript
  const score = {visitors: 0, home: 0};
  // This is okay
  score.visitors = 1;
  // This isn't allowed
  score = {visitors: 1, home: 1};
  ```

  * When we compare objects with JavaScript’s `==` operator, it compares by **identity**: it will produce `true` only if both objects are precisely the same value. Comparing different objects will return `false`, even if they have identical properties. There is no “deep” comparison operation built into JavaScript, which compares objects by contents.



### Creating Namespace

A Namespace can be created as:

```javascript
let yourNamespace = {
    
    foo: function() {
    },

    bar: function() {
    }
};

yourNamespace.foo();
```

`MATH` object provides such a namespace defining a host of functions.



### Destructuring

The **destructuring assignment** syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables. 

```javascript
let a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20
```

A detailed description can be found [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).



## JSON

* JSON stands for JavaScript Object Notation is a popular serialization (converting data into a flat format) format.
* JSON looks similar to JavaScript’s way of writing arrays and objects, with a few restrictions:
  * All property names have to be surrounded by double quotes (single quotes or backticks not allowed)
  * Only simple data expressions are allowed — no function calls, bindings, or anything that involves actual computation
  * Comments are not allowed

```javascript
{
	"squirrel": false,
	"events": ["work", "touched tree", "pizza", "running"]
}
```

JavaScript gives us the functions `JSON.stringify` and `JSON.parse` to convert data to and from this format.

```javascript
let string = JSON.stringify({squirrel: false, events: ["weekend"]});
console.log(string);
// -> {"squirrel":false,"events":["weekend"]}
console.log(JSON.parse(string).events);
// -> ["weekend"]
```


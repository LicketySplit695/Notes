# Objects in JavaScript



* Values of the type object are a **set** of optional properties ( an empty `{  }` is also a valid object ). A property is a “key: value” pair, where key is a string (also called a “property name”), and value can be anything.

* Objects can be created as:

  ```javascript
  let testObj = {
      left: 1,
      right: 2,
      'John Doe': 'Testing'
  }

  let emptyObj = new Object();  // using object "constructor"
  let anotherEmptyObj = {};     // using object literal
  ```

  * Properties whose names aren’t valid binding names or valid numbers have to be quoted (either single or double - NO backtick) ('John Doe' in the example). To access them we need to use `[]` notation.

  * Reading a property that doesn’t exist gives `undefined`.

  * Property name could be anything (even 0, 1, etc.). However a property name `__proto__` can't have non-object value.

  * Property name is case sensitive.

  * Almost all JavaScript values have properties. The exceptions are `null` and `undefined`.

    ```javascript
    null.length;
    // -> TypeError: null has no properties
    ```

* Arrays are just a kind of object specialized for storing sequences of things. If we evaluate `typeof []`, it produces `object`.

* It's possible to create a new property or reassign a new value to an existing property by:

  ```javascript
  testObj.newProperty = 'RandomValue';
  console.log(testObj);
  // -> { left: 1, right: 2, 'John Doe': 'Testing', newProperty: 'RandomValue' }
  ```

* Properties grasp on to values as variables (bindings) do in JavaScript.

* Properties are ordered if they have 'integer' names (or string with only integer). If thats not the case, then they are displayed in the order in which they were created.

* `delete` can be used to remove a named property in an object

  ```javascript
  delete testObj['John Doe'];
  console.log(testObj['John Doe']);
  // -> undefined
  ```

* To check if a string is a valid property name (even if the property has `undefined` value) of an object we use `in` operator:

  ```javascript
  console.log(['John Doe'] in testObj);
  // -> false
  ```

* `Object.keys()` returns an array of property name.

  ```javascript
  console.log(Object.keys({x: 0, y: 0, z: 2}));
  // -> ['x', 'y', 'z']
  ```

*  `Object.assign()` copies all properties from one object into another (It creates a new object).

   ```javascript
   let objectA = {a: 1, b: 2};
   Object.assign(objectA, {b: 3, c: 4});
   console.log(objectA);
   // -> {a: 1, b: 3, c: 4}
   ```

   * ES2018 allows spread syntax to clone objects

    ```javascript
    let objectA = {a: 1, b: 2};
    let objectB = { ...objectA };
    ```



## `.` notation vs `[]` notation

* Ways to access a property:

  * **With a dot `.`**

    The word after the `.` is the literal name of the property. Ex. in `value.x`, `x` is the name of the property.

  * **With Square Brackets `[]`**

    The word/expression within the square brackets is **evaluated** to get the property name. Ex. in `value[x]`, the expression x is evaluated and the result is converted to a string, and used as the property name.

    * `[]` can have string literals or variables, whose value is computed at run time to access the property value.

      ```javascript
      let user = {};
      let propertyName = prompt('Give a name');
    
      user[propertyName] = propertyValue;
    
      user['Another prop'] = AnotherValue;
      ```

    * To access properties like `arrayObj.length` with bracket notation we use `arrayObj['length']`.

    * To create or access property with a space in its name use square brackets notation as:

      ```javascript
      let testObj = { left: 1, right: 2 };
      testObj['John Doe'] = 'Test';
      console.log(testObj);
      // -> { left: 1, right: 2, 'John Doe': 'Test' }
      ```

    * Dot notation can not be used with property names which are only numbers (like array indexes).

* The elements in an array are stored as the array’s properties, using numbers as property names.



### Computed Property

* We can use square brackets in an object literal, when creating an object. That’s called computed properties.

  ```javascript
  let fruit = prompt('Which fruit to buy?', 'apple');

  let bag = {
    [fruit]: 5, // the name of the property is taken from the variable fruit
  };
  
  alert( bag.apple ); // 5 if fruit='apple'
  ```



### Property value shorthand

* We can have a property with the same name as that of variable name and same value as variable value.

  ```javascript
  let x = 'Test';

  let obj = { x, left: 2 };
  
  console.log(x);   // { x: 'Test', left: 2 }
  ```



### `in` Operator

* `in` operator checks if a property exists in a object.

  ```javascript
  let obj = { left: 1, right: 2, hand: undefined };

  console.log(hand in obj);   // true
  ```

  * It comes in handy especially if we have properties that store `undefined`.



## Object Reference

* Objects are stored and copied “by reference”, as opposed to primitive values: strings, numbers, booleans, etc – that are always copied “as a whole value”.

  ```javascript
  let user = { name: 'John' };
  let admin = user;
  admin.name = 'Pete';
  alert(user.name); // 'Pete',
  ```

* A `const` object can be modified. This is so because we are storing the reference in const variable. Modifying the value doesn’t change the reference.

  ```javascript
  const obj = { left: 1 };
  obj.right = 2; // OK, obj = { left: 1, right: 2 }

  obj = { a: 1 }; // Error!
  ```



## Mutability

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


  * When we compare objects with JavaScript’s `==` operator, it compares by **identity**: it will produce `true` only if both objects are precisely the same value. Comparing different objects will return `false`, even if they have identical properties. There is no “deep” comparison operation built into JavaScript, which compares objects by contents.



### Garbage Collection

* Garbage collection is performed automatically. We cannot force or prevent it.

* Objects are retained in memory while they are reachable. It goes to the GC when it becomes unreachable from the root.



## Methods

* Properties that contain functions are generally called methods

  ```javascript
  let user = {
    name: 'John',
    age: 30
  };

  user.sayHello = function () { console.log('Hello!'); }

  user.sayHello();  // Hello!
  ```

* We also have a shorter way to declare methods inside objects.

  ```javascript
  let user = {
    name: 'John',
    age: 30,

    sayHello () {
      console.log('Hello!');
    }
  };
  ```



## `this` keyword

* To access the underlying object, a method can use the `this` keyword.

  ```javascript
  let user = {
    name: 'John',
    age: 30,

    sayHello () {
      console.log(`Hello ${ this.name }!`);
    }
  };
  ```

* `this` in JavaScript is not bound. The value of `this` is evaluated during the run-time, depending on the context.

  ```javascript
  let user = { name: 'John' };
  let admin = { name: 'Admin' };
  
  function sayHello() {
    alert( this.name );
  }
  
  user.f = sayHello;
  admin.f = sayHello;
  
  user.f(); // John  (this == user)
  admin.f(); // Admin  (this == admin)
  
  admin['f'](); // Admin 
  ```

  * If we don't have an underlying object, `this` is `undefined` in strict mode.

    ```javascript
    function sayHello() {
      console.log(this);    // undefined in strict mode
      console.log(this.name);   // Error in strict mode
    }
    ```

  * In non-strict mode the value of `this` in such case will be the *global object*.



### Arrow functions have no `this`

* Arrow functions  don’t have their “own” `this`. If we reference `this` from such a function, it’s taken from the outer “normal” function.

  ```javascript
  let user = {
    firstName: 'John',
    sayHello() {
      let arrow = () => alert(this.firstName);
      arrow();
    }
  };
  
  user.sayHello(); // John
  ```

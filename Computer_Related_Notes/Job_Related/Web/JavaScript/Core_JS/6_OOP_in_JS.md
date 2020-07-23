# Object Oriented Concepts in JavaScript



## Encapsulation

* The idea of Encapsulation is to divide programs into smaller pieces and make each piece responsible for managing its own state.

* Different pieces of such a program interact with each other through interfaces, limited sets of functions or bindings that provide useful functionality at a more abstract level, hiding their precise implementation.

* Properties that are part of the interface are called public. The others, which outside code should not be touching, are called private.

* **Separating interface from implementation** is a great idea. It is usually called encapsulation.

* Encapsulation may be implemented by using IIFE and Closure. Ex:

  ```javascript
  var counter = (function() {
    var privateCounter = 0;
    function changeBy(val) {
      privateCounter += val;
    }
  
    return {
      increment: function() { changeBy(1); },
  
      decrement: function() { changeBy(-1); },
  
      value: function() { return privateCounter; }
    };
  })();
  
  console.log(counter.value());  // 0.
  
  counter.increment();
  console.log(counter.value());  // 1.
  
  counter.decrement();
  console.log(counter.value());  // 0.
  ```

  `privateCounter` and `changeBy()` are inaccessible outside the block.




### Methods

* Methods are properties that hold function values.

* When a function is called as a method — looked up as a property and immediately called, as in `object.method()` — the binding called `this` in its body automatically points at the object that it was called on.

* Since each function has its own `this` binding, whose value depends on the way it is called, you cannot refer to the `this` of the wrapping scope in a regular function defined with the `function` keyword.

  * Arrow functions are different—they do not bind their own `this` but can see the `this` binding of the scope around them.

    ```javascript
    function normalize() {
    	console.log(this.coords.map(n => n / this.length));
    }
    normalize.call({coords: [0, 2, 3], length: 5});
    // -> [0, 0.4, 0.6]
    ```

    To implement this with `function` keyword we need to use a reference of `this` inside the function.



### Prototypes

* A prototype is another object that is used as a fallback source of properties. 

* When an object gets a request for a property that it does not have, its prototype will be searched for the property, then the prototype’s prototype, and so on.

* The entity behind all objects is the `Object.prototype`. 
  `Object.getPrototypeOf()` returns the prototype of the object.

  ```javascript
  console.log(Object.getPrototypeOf({}) == Obejct.prototype);
  // -> true
  console.log(Object.getPrototypeOf(Object.prototype));
  // -> null
  ```

* The prototype relations of JavaScript objects form a tree-shaped structure, and at the root of this structure sits `Object.prototype`. It provides a few methods that show up in all objects, such as `toString()`, which converts an object to a string representation.

  * Other objects such as functions derive from `Function.prototype`, and arrays derive from `Array.prototype`.

    ```javascript
    console.log(Object.getPrototypeOf(Math.max) == Function.prototype);
    // -> true
    console.log(Object.getPrototypeOf([]) == Array.prototype);
    // -> true
    ```

    * Such a prototype object will itself have a prototype, often `Object.prototype`, so that it still indirectly provides methods like `toString`.

* To create an object from another object's prototype we use the `Object.create`.

    ```javascript
let protoRabbit = {
        speak(line) {
        	console.log(`The ${this.type} rabbit says '${line}'`);
        }
    };
    let killerRabbit = Object.create(protoRabbit);
    killerRabbit.type = "killer";
    killerRabbit.speak("SKREEEE!");
    // -> The killer rabbit says 'SKREEEE!'
    ```
    
    * A property like `speak(line)` in an object expression is a shorthand way of defining a method. It creates a property called speak and gives it a function as its value.
    * The “proto” rabbit acts as a container for the properties that are shared by all rabbits.



## Classes in JavaScript

* A class defines the shape of a type of object—what methods and properties it has. Such an object is  called an instance of the class.



### Class Notation

* Post 2015 JavaScript has a `Class` notation

```javascript
class Rabbit {
    constructor(type) {
    	this.type = type;
    }
    speak(line) {
    	console.log(`The ${this.type} rabbit says '${line}'`);
    }
}
let killerRabbit = new Rabbit("killer");
let blackRabbit = new Rabbit("black");
```

* The class keyword starts a class declaration, which allows us to define a `constructor` and a set of methods all in a single place. 

* Any number of methods (only methods as of now) may be written inside the declaration’s braces. The one named constructor. 

  * The `constructor` provides the actual constructor function, which will be bound to the name Rabbit. 
  * The other methods are packaged into that constructor’s prototype.

* Class can be used in statements and in expressions (similar to functions)

  * When used as an expression, it doesn’t define a binding but just produces the constructor as a value. 
  * Class name may be omitted in such cases.

  ```javascript
  let object = new class { getWord() { return "hello"; } }; // class name omitted
  console.log(object.getWord());
  // -> hello
  ```

  

---

#### Pre - 2015 Class representation

* Pre-2015 Classes are implemented using functions in JavaScript.

* Prototypes were useful for defining properties for which all instances of a class share the same value, such as methods.

* Prototypes can be used to implement Inheritance among classes. Using prototype we can implement a constructor (i.e., a function that returns object):

  ```javascript
  function makeRabbit(type) {
      let rabbit = Object.create(protoRabbit);
      rabbit.type = type;
      return rabbit;
  }
  ```

* When a `new` keyword is put before the function call, the function is treated as a constructor. 

  * The object of the right prototype is automatically created, the `this` is bound to the function and returned at the end of the function.
  * The prototype object used when constructing objects is found by taking the prototype property of the constructor function.
  * Constructors (all functions, in fact) automatically get a property named prototype, which by default holds a plain, empty object that derives from `Object.prototype`.

  ```javascript
  function Rabbit(type) {
  	this.type = type;
  }
  Rabbit.prototype.speak = function(line) {
  	console.log(`The ${this.type} rabbit says '${line}'`);
  };
  let weirdRabbit = new Rabbit("weird");
  ```

  * There is a difference in the way a prototype is associated with a constructor (through its `prototype` property) and the way objects have a prototype (which can be found with `Object.getPrototypeOf`). The actual prototype of a constructor is `Function.prototype` since constructors are functions. Its prototype property holds the prototype used for instances created through it.

    ```javascript
    console.log(Object.getPrototypeOf(Rabbit) == Function.prototype);
    // -> true
    console.log(Object.getPrototypeOf(weirdRabbit) == Rabbit.prototype);
    // -> true
    ```

---




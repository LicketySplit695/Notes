# Object Oriented Concepts in JavaScript



# Encapsulation

* The idea of Encapsulation is to divide programs into smaller pieces and make each piece responsible for managing its own state.

* Different pieces of such a program interact with each other through interfaces, limited sets of functions or bindings that provide useful functionality at a more abstract level, hiding their precise implementation.

* Properties that are part of the interface are called public. The others, which outside code should not be touching, are called private.

* **Separating interface from implementation** is called encapsulation.

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
    
    * It is possible to create objects with **no prototype**. Such objects don't derive from `Object.prototype`.
    
      ```javascript
      let emptyObj = Object.create(null);
      console.log(Object.getPrototypeOf(empty)); // -> null
      ```
    
      
    
    * A property like `speak(line)` in an object expression is a shorthand way of defining a method. It creates a property called speak and gives it a function as its value.
    
    * **Note:** `killerRabbit` **doesn't get** those properties of `protoRabbit`. It only contains properties defined for itself (`type` here).
    
    * The “proto” rabbit acts as a container for the properties that are shared by all rabbits.
    
    * `Object.keys` returns only an object’s own keys, not those in the prototype.



## Classes in JavaScript

* A class defines the shape of a type of object—what methods and properties it has. Such an object is  called an instance of the class.



### Class Notation

* Post 2015 JavaScript has a `Class` notation

```javascript
class Rabbit {
    constructor(type) {
    	this.type = type; // -> type is a property. Not a field
    }
    speak(line) {
    	console.log(`The ${this.type} rabbit says '${line}'`);
    }
}
let killerRabbit = new Rabbit("killer");
let blackRabbit = new Rabbit("black");
```

* The **class keyword** starts a class declaration, which allows us to define a `constructor` and a set of methods all in a single place. 

* Any number of methods (only methods as of now) may be written inside the declaration’s braces including one named constructor. 

  * The `constructor` provides the actual constructor function, which will be bound to the name Rabbit. 
  * The other methods are packaged into that constructor’s prototype.

* Class can be used in statements and in **expressions** (similar to functions)

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

  ```javascript
  function Person(name)
  {
      this.name = name;  
      this.changeName = newName => this.name = newName; 
  }
  ```

  * `Person` can be used a class/constructor. We can use it by declaring with the `new` keyword.

    ```javascript
    let p = new Person("Angshuman");
    ```

* When a `new` keyword is put before the function call, the function is treated as a constructor. 

  * The object of the right prototype is automatically created (it is different from `Object.create()`, as properties of prototypes are created for the new object), the `this` is bound to the function and returned at the end of the function.
  * The prototype object used when constructing objects is found by taking the prototype property of the constructor function.
  * **Constructors** (all functions, in fact) **automatically get a property named prototype**, which by default holds a plain, empty object that derives from `Object.prototype`.

  ```javascript
  function Rabbit(type) {
  	this.type = type;
  }
  Rabbit.prototype.speak = function(line) {
  	console.log(`The ${this.type} rabbit says '${line}'`);
  };
  let weirdRabbit = new Rabbit("weird");
  ```

  

* A prototype is **associated with a constructor** (through its `prototype` property) and **objects have a prototype** (which can be found with `Object.getPrototypeOf`). The <u>actual prototype of a constructor is `Function.prototype`</u> since constructors are functions. Its prototype property holds the prototype used for instances created through it.

  ```javascript
  console.log(Object.getPrototypeOf(Rabbit) == Function.prototype); // -> Prototype of Constructor
  // -> true
  console.log(Object.getPrototypeOf(weirdRabbit) == Rabbit.prototype); // -> Prototype associated with the Ctor
  // -> true
  ```

---



---

#### Fields in Class

* There could be public or private fields in a class.

  ```javascript
  // Public fields
  class Rectangle {
    height = 0;
    width;
    constructor(height, width) {    
      this.height = height;
      this.width = width;
    }
  }
  ```

  ```javascript
  // Private Fields
  class Rectangle {
    #height = 0;
    #width;
    constructor(height, width) {    
      this.#height = height;
      this.#width = width;
    }
  }
  ```

  * It's an error to reference private fields from outside of the class; they can only be read or written within the class body.
  * Private fields can only be declared up-front in a field declaration.
  * Private fields cannot be created later through assigning to them, the way that normal properties can.

---



---

#### Hash-tables with `map` class

* It is safer to use `Map` over plain objects to store a hash-table. Difference can be found [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map).

* The **`Map`** object holds key-value pairs and remembers the original insertion order of the keys. Any value (both objects and [primitive values](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)) may be used as either a key or a value.

* Although the following seems to work, but **all map functions will not work** if the following is used.

  ```javascript
  let ages = new Map();
  ages["Boris"] = 39;
  ages.Julia = 62;
  ```

* The methods `set`, `get`, and `has` are part of the interface of the Map object.

  ```javascript
  let ages = new Map();
  ages.set("Boris", 39);
  ages.set("Julia", 62);
  
  ages.get("Julia");
  
  ages.delete("Boris");
  
  console.log(ages.size); // -> 1
  ```

  * In case of map `NaN` can be a key. `NaN` is indistinguishable here.

---



## *Getters* and *Setters* and `static`

* *Getters* and *Setters* are properties that hide a function call. Static methods are called without [instantiating ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript#The_object_(class_instance))their class and **cannot** be called through a class instance.

  ```javascript
  class Temperature {
  	constructor(celsius) {
  		this.celsius = celsius; // -> Proptery
  	}
  
      get fahrenheit() {			// -> Getter
  		return this.celsius * 1.8 + 32;
  	}
  
      set fahrenheit(value) {		// -> Setter
  		this.celsius = (value - 32) / 1.8;
  	}
  	
      static fromFahrenheit(value) {			// -> Static method
  		return new Temperature((value - 32) / 1.8);
  	}
  }
  
  let temp = new Temperature(22);
  console.log(temp.fahrenheit);	// -> 71.6
  
  temp.fahrenheit = 86;
  console.log(temp.celsius);	// -> 30
  
  console.log(Temperature.fromFahrenheit(98.6)); // -> Temperature { celsius: 36.99999999999999 }
  ```



# Polymorphism

Examples of use of polymorphism in JS is as follows:

### Over-riding derived properties

* When you add a property to an object, whether it is present in the prototype or not, the property is added to the object itself. 

* If there was already a property with the same name in the prototype, it will be hidden behind the object’s own property.

  ```javascript
  let person = {
      name: 'Angshuman'
  };
  
  let anotherPerson = Object.create(person);
  console.log(anotherPerson.name); // -> Angshuman
  console.log(Object.keys(anotherPerson)); // -> []
  
  anotherPerson.name = 'Rahul';
  anotherPerson.age = 23;
  console.log(Object.keys(anotherPerson)); // -> [ 'name', 'age'] 
  ```

  

## Symbols

* Symbols are values created with the `Symbol()` function. Newly created symbols are unique—we cannot create the same symbol twice.
* Property names are strings, and can also be symbols but nothing else.

```javascript
let sym = Symbol("name");
console.log(sym == Symbol("name")); // -> false
Rabbit.prototype[sym] = 55;
console.log(blackRabbit[sym]); // -> 55
```

* `name` is just a name (description) for the symbol. Note: - multiple symbols may have the same name.

  ```javascript
  const toStringSymbol = Symbol("toString");
  Array.prototype[toStringSymbol] = function() {
  	return `${this.length} cm of blue yarn`;
  };
  console.log([1, 2].toString()); // -> 1,2
  console.log([1, 2][toStringSymbol]()); // -> 2 cm of blue yarn
  ```

* To get the description we need to use the description property, or `toString()`

  ```javascript
  alert(toStringSymbol);
  alert(toStringSymbol.toString()); // Symbol(toString)
  alert(toStringSymbol.description); // toString
  ```

* It is possible to include symbol properties in object expressions and classes by using square brackets around the property name.

```javascript
let stringObject = {
    [id]: 123, // not "id": 123
	[toStringSymbol]() { return "a jute rope"; } // -> A function binding whose name is a symbol
};
console.log(stringObject[toStringSymbol]()); // -> a jute rope
```

* Symbolic properties do not participate in `for..in` loop.
* `Object.keys()` also neglect symbols. [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) however copies both string and symbol properties.



### Iterators

* The object given to a `for`/`of` loop is expected to be *iterable*, ie. it has a method named with the `Symbol.iterator` symbol. 
* For any object `obj` to be made iterable
  * `obj` must have a function with Symbol - `Symbol.iterator`
    * This function must return an object with a function `next()`. The system will work for the object only (ie. the one returned by the `System.iterator` function, which may be `obj` itself if it has a `next()` function.)
  * The `next()` method must return another object `{ done: boolean, value: any }`, where 
    * `done` will be false if the iteration is supposed to continue. If it becomes true, iteration stops.
    * `value` contains what we need in the temporary variable.

```javascript
let range = {
    from: 1,
    to: 4
}

range[Symbol.iterator] = function(){		// range must have a `Symbol.iterable` function
    return {	
        current: this.from,
        last: this.true,
        
        next()								// The function must return an object with a method `next()`
        {
            if(this.current > this.last) return { done: true };
            return { done: false, value: this.current++ };
        }
    };
};

for(let n of range){
    console.log(n);
}

/*
1
2
3
4
*/
```



# Inheritance

* Child class (called *subclass*) can inherit from parent class (called *superclass*) using the `extends` keyword.
* If we want to use parent class elements we use super - `super()` -> As the parent constructor, `super.property` for properties and methods

```javascript
class Animal { 
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // call the super class constructor and pass in the name parameter
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```

* It can also be used for older function based parent classes.

```javascript
function Animal (name) {
  this.name = name;  
}

Animal.prototype.speak = function () {
  console.log(`${this.name} makes a noise.`);
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.

// For similar methods, the child's method takes precedence over parent's method
```

* For plain objects we may use `Object.setPrototypeOf(SubClassName.prototype, SuperClassName)`. [Example](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)



### `instanceOf` operator

* It useful to know whether an object was derived from a specific class.

```javascript
// ... prev example
console.log(d instanceof Animal);
// -> true
```

* The operator can also be applied to standard constructors like Array. 
* Almost every object is an instance of Object.


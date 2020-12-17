# Objects - Other Topics



## Constructor, `new` operator

* Constructors are regular functions except 2 conventions

  1. Their names must start with capital letters.
  2. They should be executed with the `new` operator.

  ```javascript
  function User(name) {
    this.name = name;
    this.isAdmin = false;
  }
  
  let user = new User('Jack');
  
  console.log(user.name); // Jack
  console.log(user.isAdmin); // false
  ```

  * When a function (a constructor) is executed with `new`:

    1. A new empty object is created and assigned to `this`.
    2. The function body executes. Usually it modifies `this`, adds new properties to it.
    3. The value of `this` is returned.

  * Any function can be used with `new`.

  * We may add methods to `this` as well.

    ```javascript
    function User(name) {
      this.name = name;
    
      this.sayHello = function() {
        console.log( 'My name is: ' + this.name );
      };
    }
    
    let john = new User('John');
    
    john.sayHello(); // My name is: John
    ```

  * If we don't have any arguments for the constructor when calling with `new`, then the parenthesis may not be used.

    ```javascript
    let user = new User;  // OK
    ```



### Testing constructor mode

* We can use `new.target` (from inside the function) to test if the function has been called with `new`.

  ```javascript
  function User() {
    console.log(new.target);
  }

  User()  // undefined

  new User() // [Function: user]
  ```



### `return` in constructor

* Constructor usually returns nothing. However if we use a return statement:

  * If we return an 'object', it returns that object instead of `this`.
  * If we return any primitive value, it's ignored.

  ```javascript
  function BigUser() {
    this.name = 'John';
    return { name: 'Godzilla' };  // <-- returns this object
  }
  
  console.log( new BigUser().name );  // Godzilla
  ```



## Chaining with `?.`

* The optional chaining `?.` is a safe way to access nested object properties, even if an intermediate property doesn’t exist; i.e `value?.prop`:

  * is same as `value?.prop` if value exists
  * `undefined` (**No Error**) if value is `null` or `undefined`.

  ```javascript
  let user = {};
  console.log( user?.address?.street ); // undefined

  user = null; 
  console.log( user?.address ); // undefined
  console.log( user?.address?.street ); // undefined
  ```

* If the object to the left of `?.` is not defined then we will have an error.

* The `?.` immediately stops (“short-circuits”) the evaluation if the left part doesn’t exist.

* The optional chaining `?.` is not an operator, but a special syntax construct, that also works with functions and square brackets.

  ```javascript
  let userAdmin = {
    admin() {
      console.log('I am admin');
    }
  };
  
  let userGuest = {};
  
  userAdmin.admin?.(); // I am admin
  
  /* Since userGuest doesn't have an admin execution stops. Nothing is printed */
  userGuest.admin?.(); 
  ```

* Also we can use `?.` to delete property if object is not `null` or `undefined`.

  ```javascript
  delete user?.name;
  ```

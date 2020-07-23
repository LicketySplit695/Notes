# Values, Types and Operations

## Values

A Computer manipulates *'values'*. The kind of values that can be represented and manipulated in a programming language are known as *types*. When a program needs to retain a value for future use it assigns the value to a *variable*.

JavaScript has two categories of types

1. **Primitive types**
   1. Numbers
   2. Strings
   3. Boolean
   4. Special values (null and undefined)
2. **Object types** 
   1. Anything that is not a primitive type
   2. Arrays
   3. Functions etc.

There are other ways to categorize types for ex. **<u>mutable and immutable</u>** types. Objects and arrays are **mutable**. Numbers, booleans, strings,`null` and `undefined` are **immutable**. 

JavaScript variables are *untyped*.



### Numbers

* All numbers in JavaScript are floating point values using a **64 bit floating point ** format.



#### Integer Literals

* Hexadecimal literals begin with `0x` or `0X`.
* Octal numbers start with `0`.[^1]



#### Floating point literals

* To represent floating point numbers we may use the following format:

  `[digits][.digits][(E|e)[(+|-)]digits]`



#### Arithmetic Operations

* JS supports all operations including 'modulo'. `Math` class can be used to perform certain operations like
  1. `Math.pow(2, 53)`
  2. `Math.round(.6)` etc.

Arithmetic in JavaScript **doesn't raise error** in case of *overflow, underflow, or division by zero*. 

---

##### `Infinity`, `-Infinity`, and `NaN`

1. When the result of a numeric operation is larger than the largest representable number (overflow), the result is `Infinity`.

2. Similarly, when a negative value becomes larger than the largest representable negative number, the result `-Infinity`.

3. Adding, subtracting, multiplying or dividing anything with, from or by `Infinity`  results in `Infinity` (or may be `-Infinity`).

4. Underflow occurs when the result of a numeric operation is closer to zero than the smallest representable number. In this case, JavaScript returns 0. If underflow occurs from a negative number, JavaScript returns a special value known as *“negative zero.”*

5. Division by zero returns `Infinity` or `-Infinity`. However `0/0` is `NaN`

6. Similarly `Infinity/Infinity` , square root of negative numbers or using Arithmetic operators with non-numeric (non-resolvable) characters give `NaN`.

7.  `NaN` doesn't compare equal to any other value including itself. For ex:

   ```javascript
   let x = NaN;
   console.log(x == x); //prints false
   // To check if x in NaN
   console.log(x != x); //prints true
   ```

   `isNaN()` returns `true` if argument is `NaN`.  Similarly `isFinite()` returns `true`  for all values excepts `NaN`, `Infinity` and `-Infinity`.

8. Negative and positive zeros compare equal except when

   ```javascript
   let x = 0, y = -0;
   console.log(x === y); //prints true
   console.log(1/x === 1/y); //prints false as Infinity != -Infinity
   ```

---



#### Dates

JS includes a `Date()` constructor. Ex.:

```javascript
let then = new Date(2010, 0 , 1);
let later = new Date(2010, 0, 1, 17, 30, 10); //Date and time
let now = new Date(); //Current Date and Time
let elapsed = now - then; //Interval in milliseconds 

console.log(typeof elapsed); //prints number
now.getDay(); // => 0 - Sunday, 1 - Monday etc.
```



### String

A `string` is an **immutable** ordered sequence of 16-bit values, each of which typically represents a <u>Unicode</u> character. 

* The length of a string is the number of 16-bit values it contains.
* JavaScript’s strings use zero-based indexing.



#### String Literals

Both `""` and  `''` can be used to delimit string literals. However if we need one inside we may use the other to delimit the string. Ex:

```javascript
'name="myform"'
"Wouldn't you prefer O'Reilly's book?"
```

ECMAScript 5 allows the following:
```javascript
let x = "one\
long\
line";
```

`\` can be used as the *escape character*.

We may want to use ```` `` to delimit strings (usually called *template literals*) for using raw strings (string that spreads over multiple lines) and to embed other values. For ex:

```javascript
let x = `Hello
World`;
console.log(x); // Prints Hello\nWorld
console.log(`half of 100 is ${100 / 2}`); // Prints half of 100 is 50
```



### Booleans

Comparison can be performed as usual in JS. Some quirks include

* Strings can be compared

  ```javascript
  console.log("Aardvark" < "Zoroaster")
  // -> true
  console.log("Itchy" != "Scratchy")
  // -> true
  console.log("Apple" == "Orange")
  // -> false
  ```

  It's to be noted that `"Z" > "a"`.

* `NaN` is the only JavaScript value that is not equal to itself.

  ```javascript
  console.log(NaN == NaN)
  // -> false
  ```



#### Logical Operators

We have as usual `||` (or), `&&` (and), and `!=` (not). `||` has the lowest precedence, then comes `&&`, then the comparison operators (>, ==, and so on), and then the rest. Apart from that we also have the *ternary operator*.



##### Short Circuiting of Logical Operators

All values are converted as possible. 

* `||` returns the first true value.
* `&&` return the first false value.



#### `null` and `undefined`

These two denote the absence of a meaningful value. They are themselves values, but they carry no information. 

* `null` is a **language keyword** that evaluates to a special value that is usually used to indicate the absence of a value.

  * `typeof null` returns `object`.

* `undefined` is a **predefined global variable** (readonly from ECMAScript 5). It is the value of

  * variables that have not been initialized
  * an object property or array element that does not exist
  * functions that have no return value
  * function parameters for which no argument is supplied

  `typeof undefined` is `undefined`.

* `null == undefined` is `true`.  We need to use `===` to distinguish between them.

* Neither null nor undefined have any properties or methods. In fact, using . or [] to access a property or method of these values causes a `TypeError`.



#### Automatic type conversion

When an operator is applied to the “wrong” type of value, JavaScript will quietly convert that value to the type it needs, using a set of rules that often aren’t what you want or expect. This is called *type coercion*.

<table>
    <tr>
        <th rowspan="2" style="border-bottom: 2px solid black">Values</th>
        <th colspan="4">Converted to:</th>
    </tr>
    <tr style="border-bottom: 2px solid black">
        <th>String</th>
        <th>Number</th>
        <th>Boolean</th>
        <th>Object</th>
    </tr>
    <tr>
        <td>undefined</td>
        <td>"undefined"</td>
        <td>NaN</td>
        <td>false</td>
        <td>throws TypeError</td>
    </tr>
    <tr style="border-bottom: 2px solid black">
        <td>null</td>
        <td>"null"</td>
        <td>0</td>
        <td>false</td>
        <td>throws TypeError</td>
    </tr>
    <tr>
        <td>true</td>
        <td>"true"</td>
        <td>1</td>
        <td></td>
        <td>new Boolean(true)</td>
    </tr>
    <tr style="border-bottom: 2px solid black">
        <td>false</td>
        <td>"false"</td>
        <td>0</td>
        <td></td>
        <td>new Boolean(false)</td>
    </tr>
    <tr>
        <td>"" (empty string)</td>
        <td></td>
        <td>0</td>
        <td>false</td>
        <td>new String("")</td>
    </tr>
    <tr>
        <td>"1.2" (nonempty, numeric)</td>
        <td></td>
        <td>1.2</td>
        <td>true</td>
        <td>new String("1.2")</td>
    </tr>
    <tr style="border-bottom: 2px solid black">
        <td>"one" (nonempty, non-numeric)</td>
        <td></td>
        <td>NaN</td>
        <td>true</td>
        <td>new String("one")</td>
    </tr>
    <tr>
        <td>0</td>
        <td>"0"</td>
        <td></td>
        <td>false</td>
        <td>new Number(0)</td>
    </tr>
    <tr>
        <td>-0</td>
        <td>"0"</td>
        <td></td>
        <td>false</td>
        <td>new Number(-0)</td>
    </tr>
    <tr>
        <td>NaN</td>
        <td>"NaN"</td>
        <td></td>
        <td>false</td>
        <td>new Number(NaN)</td>
    </tr>
    <tr>
        <td>Infinity</td>
        <td>"Infinity"</td>
        <td></td>
        <td>true</td>
        <td>new Number(Infinity)</td>
    </tr>
    <tr>
        <td>-Infinity</td>
        <td>"-Infinity"</td>
        <td></td>
        <td>true</td>
        <td>new Number(-Infinity)</td>
    </tr>
    <tr style="border-bottom: 2px solid black">
        <td>1 (finite, non-zero)</td>
        <td>"1"</td>
        <td></td>
        <td>true</td>
        <td>new Number(1)</td>
    </tr>
    <tr>
        <td>{} (any object)</td>
        <td>see below</td>
        <td>see below</td>
        <td>true</td>
        <td></td>
    </tr>
    <tr>
        <td>[] (empty array)</td>
        <td>""</td>
        <td>0</td>
        <td>true</td>
        <td></td>
    </tr>
    <tr>
        <td>[9] (1 numeric elt)</td>
        <td>"9"</td>
        <td>9</td>
        <td>true</td>
        <td></td>
    </tr>
    <tr>
        <td>['a'] (any other array)</td>
        <td>use join() method</td>
        <td>NaN</td>
        <td>true</td>
        <td></td>
    </tr style="border-bottom: 2px solid black">
        <tr>
        <td>function(){} (any function)</td>
        <td>see below</td>
        <td>NaN</td>
        <td>true</td>
        <td></td>
    </tr>
</table>
---

##### Object to primitive Conversion

1.  all objects (including arrays and functions) convert to true. `new Boolean(false)` is an object and thus converts to `true`.
2.  Objects are converted to string based on 
    1.  If `toString()` method is present, then it's called, and the output is shown.
    2.  If `toString()` is not present or doesn't return a primitve value, then `valueOf()` is called.
    3.  If result of `valueOf()` can't be converted to string then `TypeError` is thrown.
3.  Objects are converted to numbers in the same way as they are converted to strings except that `valueOf()` is tried first, if that can be turned into a Number then that is shown, then `toString()` is tried. If both don't work `TypeError` is thrown.

---



`==` allows *type coercion*. `===` stops it.

JavaScript also allows explicit type conversion.

```javascript
Number("3")		// => 3
String(false)	// => "false" Or use false.toString()
Boolean([])		// => true
Object(3)		// => new Number(3)

//Weird Techniques
x + ""		// => Same as String(x)
+x			// => Same as Number(x). You may also see x-0
!!x			// =>  Same as Boolean(x). Note double !
```



### The Global Object

* The *globally* defined symbols that are available to a JavaScript program are *properties* of the global object.
* When browser starts (or any interpreter starts) the following is defined:
  * global properties like `undefined`, `Infinity`, and `NaN`
  * global functions like `isNaN()`, `parseInt()` and `eval()`
  * constructor functions like `Date()`, `RegExp()`, `String()`, `Object()`, and `Array()`
  * global objects like `Math` and `JSON`

---

[^1]: Support for Octal numbers is implementation defined. Hence it should be used with caution.




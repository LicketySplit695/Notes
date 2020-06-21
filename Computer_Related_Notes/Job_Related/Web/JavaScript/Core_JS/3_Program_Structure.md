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



## Held at pp 24 (36)
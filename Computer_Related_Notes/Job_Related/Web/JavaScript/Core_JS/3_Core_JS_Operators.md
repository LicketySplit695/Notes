# Operators


## Maths Operations

* The following are supported

  1. `+`, `-`, `*`, `/`
  2. Remainder: `%`
  3. Exponentiation: `**`, As in `2 ** 3 = 8`



### Other Operations

* String concatenation with `+` is supported. Even if any one is not a string, its converted to a string.

  ```javascript
  let s = '1' + 2 + 2;
  console.log(s);   // '122'
  s = 2 + 2 + '1' // '41'
  ```

* However if we have other operators instead of `+` the string is instead converted to a number.

  ```javascript
  let n = 6 - '1'; // 5
  n = '6' / '2'   // 3
  ```

* Adding a `+` before any object (string, boolean etc.) converts it into a number. It performs the same as `Number()`.

  ```javascript
  let x = '1.2';
  console.log( +x );  // 1.2

  let y = false;
  console.log( +y );  // 0
  ```



## Operator Precedence

<table class="fullwidth-table">
 <tbody>
  <tr>
   <th>Precedence</th>
   <th>Operator type</th>
   <th>Associativity</th>
   <th>Individual operators</th>
  </tr>
  <tr>
   <td>21</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Grouping">Grouping</a></td>
   <td>n/a</td>
   <td><code>( … )</code></td>
  </tr>
  <tr>
   <td colspan="1" rowspan="5">20</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors#Dot_notation">Member Access</a></td>
   <td>left-to-right</td>
   <td><code>… . …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors#Bracket_notation">Computed Member Access</a></td>
   <td>left-to-right</td>
   <td><code>… [ … ]</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/new"><code>new</code></a> (with argument list)</td>
   <td>n/a</td>
   <td><code>new … ( … )</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Guide/Functions">Function Call</a></td>
   <td>left-to-right</td>
   <td><code>… ( <var>… </var>)</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining">Optional chaining</a></td>
   <td>left-to-right</td>
   <td><code>?.</code></td>
  </tr>
  <tr>
   <td rowspan="1">19</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/new"><code>new</code></a> (without argument list)</td>
   <td>right-to-left</td>
   <td><code>new …</code></td>
  </tr>
  <tr>
   <td rowspan="2">18</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment">Postfix Increment</a></td>
   <td colspan="1" rowspan="2">n/a</td>
   <td><code>… ++</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Decrement">Postfix Decrement</a></td>
   <td><code>… --</code></td>
  </tr>
  <tr>
   <td colspan="1" rowspan="10">17</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Logical_NOT">Logical NOT (!)</a></td>
   <td colspan="1" rowspan="10">right-to-left</td>
   <td><code>! …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_NOT">Bitwise NOT (~)</a></td>
   <td><code>~ …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Unary_plus">Unary plus (+)</a></td>
   <td><code>+ …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Unary_negation">Unary negation (-)</a></td>
   <td><code>- …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment">Prefix Increment</a></td>
   <td><code>++ …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Decrement">Prefix Decrement</a></td>
   <td><code>-- …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/typeof"><code>typeof</code></a></td>
   <td><code>typeof …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/void"><code>void</code></a></td>
   <td><code>void …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/delete"><code>delete</code></a></td>
   <td><code>delete …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/await"><code>await</code></a></td>
   <td><code>await …</code></td>
  </tr>
  <tr>
   <td>16</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Exponentiation">Exponentiation (**)</a></td>
   <td>right-to-left</td>
   <td><code>… ** …</code></td>
  </tr>
  <tr>
   <td rowspan="3">15</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Multiplication">Multiplication (*)</a></td>
   <td colspan="1" rowspan="3">left-to-right</td>
   <td><code>… * …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Division">Division (/)</a></td>
   <td><code>… / …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Remainder">Remainder (%)</a></td>
   <td><code>… % …</code></td>
  </tr>
  <tr>
   <td rowspan="2">14</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Addition">Addition (+)</a></td>
   <td colspan="1" rowspan="2">left-to-right</td>
   <td><code>… + …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Subtraction">Subtraction (-)</a></td>
   <td><code>… - …</code></td>
  </tr>
  <tr>
   <td rowspan="3">13</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Left_shift">Bitwise Left Shift (&lt;&lt;)</a></td>
   <td colspan="1" rowspan="3">left-to-right</td>
   <td><code>… &lt;&lt; …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Right_shift">Bitwise Right Shift (&gt;&gt;)</a></td>
   <td><code>… &gt;&gt; …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Unsigned_right_shift">Bitwise Unsigned Right Shift (&gt;&gt;&gt;)</a></td>
   <td><code>… &gt;&gt;&gt; …</code></td>
  </tr>
  <tr>
   <td rowspan="6">12</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Less_than">Less Than (&lt;)</a></td>
   <td colspan="1" rowspan="6">left-to-right</td>
   <td><code>… &lt; …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Less_than_or_equal">Less Than Or Equal (&lt;=)</a></td>
   <td><code>… &lt;= …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than">Greater Than (&gt;)</a></td>
   <td><code>… &gt; …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than_or_equal">Greater Than Or Equal (&gt;=)</a></td>
   <td><code>… &gt;= …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/in"><code>in</code></a></td>
   <td><code>… in …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/instanceof"><code>instanceof</code></a></td>
   <td><code>… instanceof …</code></td>
  </tr>
  <tr>
   <td rowspan="4">11</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Equality">Equality (==)</a></td>
   <td colspan="1" rowspan="4">left-to-right</td>
   <td><code>… == …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Inequality">Inequality (!=)</a></td>
   <td><code>… != …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality">Strict Equality (===)</a></td>
   <td><code>… === …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Strict_inequality">Strict Inequality (!==)</a></td>
   <td><code>… !== …</code></td>
  </tr>
  <tr>
   <td>10</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_AND">Bitwise AND (&amp;)</a></td>
   <td>left-to-right</td>
   <td><code>… &amp; …</code></td>
  </tr>
  <tr>
   <td>9</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_XOR">Bitwise XOR (^)</a></td>
   <td>left-to-right</td>
   <td><code>… ^ …</code></td>
  </tr>
  <tr>
   <td>8</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_OR">Bitwise OR (|)</a></td>
   <td>left-to-right</td>
   <td><code>… | …</code></td>
  </tr>
  <tr>
   <td>7</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND">Logical AND (&amp;&amp;)</a></td>
   <td>left-to-right</td>
   <td><code>… &amp;&amp; …</code></td>
  </tr>
  <tr>
   <td>6</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR">Logical OR (||)</a></td>
   <td>left-to-right</td>
   <td><code>… || …</code></td>
  </tr>
  <tr>
   <td>5</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator">Nullish coalescing operator (??)</a></td>
   <td>left-to-right</td>
   <td><code>… ?? …</code></td>
  </tr>
  <tr>
   <td>4</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator">Conditional (ternary) operator</a></td>
   <td>right-to-left</td>
   <td><code>… ? … : …</code></td>
  </tr>
  <tr>
   <td rowspan="16">3</td>
   <td rowspan="16"><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Assignment_Operators">Assignment</a></td>
   <td rowspan="16">right-to-left</td>
   <td><code>… = …</code></td>
  </tr>
  <tr>
   <td><code>… += …</code></td>
  </tr>
  <tr>
   <td><code>… -= …</code></td>
  </tr>
  <tr>
   <td><code>… **= …</code></td>
  </tr>
  <tr>
   <td><code>… *= …</code></td>
  </tr>
  <tr>
   <td><code>… /= …</code></td>
  </tr>
  <tr>
   <td><code>… %= …</code></td>
  </tr>
  <tr>
   <td><code>… &lt;&lt;= …</code></td>
  </tr>
  <tr>
   <td><code>… &gt;&gt;= …</code></td>
  </tr>
  <tr>
   <td><code>… &gt;&gt;&gt;= …</code></td>
  </tr>
  <tr>
   <td><code>… &amp;= …</code></td>
  </tr>
  <tr>
   <td><code>… ^= …</code></td>
  </tr>
  <tr>
   <td><code>… |= …</code></td>
  </tr>
  <tr>
   <td><code>… &amp;&amp;= …</code></td>
  </tr>
  <tr>
   <td><code>… ||= …</code></td>
  </tr>
  <tr>
   <td><code>… ??= …</code></td>
  </tr>
  <tr>
   <td rowspan="2">2</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/yield"><code>yield</code></a></td>
   <td colspan="1" rowspan="2">right-to-left</td>
   <td><code>yield …</code></td>
  </tr>
  <tr>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/yield*"><code>yield*</code></a></td>
   <td><code>yield* …</code></td>
  </tr>
  <tr>
   <td>1</td>
   <td><a href="/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator">Comma / Sequence</a></td>
   <td>left-to-right</td>
   <td><code>… , …</code></td>
  </tr>
 </tbody>
</table>



* Modification in place (`+=`, `-=`, `*=`, `/=`) is supported.

* Postfix and Prefix increment `++` and decrement `--` work as usual.

* Assignment like any other language, returns the assigned value.

  ```javascript
  let a = 1, b = 2;

  let c = 3 - (a = b + 1);

  console.log(a);   // 3
  console.log(c);   // 0
  ```

* Bitwise operators, that treat arguments as 32-bit integer numbers, are supported too. They are

  * AND ( `&` )
  * OR ( `|` )
  * XOR ( `^` )
  * NOT ( `~` )
  * LEFT SHIFT ( `<<` )
  * RIGHT SHIFT ( `>>` )
  * ZERO-FILL RIGHT SHIFT ( `>>>` )

  Details can be found [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Bitwise).



#### Comma Operator

* Can be used to compute several expressions in the same line. (Similar to initialization of a for-loop).

* All expressions are computed, only the value of the last expression is returned.

  ```javascript
  let a = (1 + 2, 3 + 4); // a = 7
  ```

* `,` has a very low precedence.



## Comparisons

* All comparisons return either `true` or `false`. The value returned can be used.

  ```javascript
  let c = a > 4;
  ```

* String can be compared based on their Unicode order. (i.e. `'Z' < 'a' == true`)

* When comparing values of different types, JavaScript converts the values to numbers

  ```javascript
  console.log('01' == 1);   // true
  ```

* `===` or `!==` compares 2 operators without type conversion.

* `null` and `undefined` behaves weirdly when compared with 0. (More [here](https://javascript.info/comparison#strange-result-null-vs-0))



## Logical Operators

* `||` (OR) computes all expressions until it finds and returns the **first '*truthy*'** value. If no truthy value is found it returns the value of the last expression (which should be falsy).

* `&&` (AND) computes all expressions until it finds and returns the **first '*falsy*'** value. If no falsy value is found it returns the value of the last expression (which is truthy).

* `!` (NOT) negates the boolean value extracted from the expression.

* `!!` converts the expression to a boolean type. Its same as `Boolean()`

  ```javascript
  console.log(!!'Hello');   // true
  console.log(!!null);    // false

  console.log(Boolean('Hello'));  // true
  ```



### Null coalescing operator

* `??` returns the first expression that has a 'defined' value. (i.e its neither `null` nor `undefined`).

  ```javascript
  let name;
  console.log(name ?? 'Anonymous');   // 'Anonymous'
  ```
  ```javascript
  let firstName = null;
  let lastName = null;
  let nickName = "Supercoder";
  
  // shows the first truthy value:
  alert(firstName ?? lastName ?? nickName ?? "Anonymous"); // Supercoder
  ```

* The difference between `??` and `||` is that OR considers 0, null, undefined, '' etc. all as same, whereas `??` considers only `null` or `undefined`.

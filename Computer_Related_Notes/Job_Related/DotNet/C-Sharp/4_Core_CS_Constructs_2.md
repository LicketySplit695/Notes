# Core C# Programming Constructs - II

## Arrays

* An array is a set of contiguous data points of the same type.

* Arrays are declared as:

  ```c#
  int[] myInts = new int[3/*Known value at runtime*/];
  ```

*  If we declare an array but do not explicitly fill each index, each item will be set to the default value of the data type (array of `bools` will be set to `false` or an `array` of `ints` will be set to 0).

*  Array Initialization:

   ```c#
   string[] stringArray = new string[3]{ "one", "two", "three" }; 
   // 3 is optional. But if we put 4 it will be a compile time error
   bool[] boolArray = { false, false, true };
   ```

   * `new` is optional.
   * Length of the array is optional. But if declared it has to match the no. of elements in the initialization list.

*  Implicit typing using `var` keyword is OK

   ```c#
   var a = new[] { 1, 10, 100, 1000 };
   ```

   * Implicitly typed array is strongly typed. The following is an error.

     ```c#
     var d = new[] { 1, "one", 2, "two", false };
     ```

   * An array of `object` s may be defined if we want to put items of multiple type in an array. In such situation `System.Object.GetType()` method can be used to obtain the fully qualified name of the objects

     ```c#
     object[] myObjects = new object[3];
     myObjects[0] = 10;
     myObjects[1] = false;
     myObjects[2] = new DateTime(1969, 3, 24);
     
     foreach (object obj in myObjects)
     {
     	Console.WriteLine("Type: {0}, Value: {1}", obj.GetType(), obj);
     }
     
     // Type: System.Int32, Value: 10
     // Type: System.Boolean, Value: False
     // Type: System.DateTime, Value: 3/24/1969 12:00:00 AM
     ```

* We are free to pass an array as an argument or receive it as a member return value. Arrays in C# are Class level types and don't misbehave as in C.



### Multidimensional Arrays

#### Rectangular Array

* An array of multiple dimensions, where each row is of the same length.

```c#
int[,] myMatrix;
myMatrix = new int[3,4];

// Populate the Matrtix
for(int i = 0; i < 3; i++)
    for(int j = 0; j < 4; j++)
    	myMatrix[i, j] = i * j;

// Printing the matrix
for(int i = 0; i < 3; i++)
{
    for(int j = 0; j < 4; j++)
    	Console.Write(myMatrix[i, j] + "\t");
    Console.WriteLine();
}

/*
0 0 0 0
0 1 2 3
0 2 4 6
*/
```



#### Jagged Array

* Jagged arrays contain some number of inner arrays, each of which may have a different upper limit.

```c#
// Here we have an array of 5 different arrays.
int[][] myJagArray = new int[5][];

// Create the jagged array.
for (int i = 0; i < myJagArray.Length; i++)
	myJagArray[i] = new int[i + 7];

// Print each row (remember, each element is defaulted to zero!).
for(int i = 0; i < 5; i++)
{
    for(int j = 0; j < myJagArray[i].Length; j++)
    	Console.Write(myJagArray[i][j] + " ");
    Console.WriteLine();
}

/*
0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0 0
*/
```



### The `System.Array` base class

* Every array we create gathers much of its functionality from the `System.Array` class. A few members of it are as follows

  <table>
  	<thead>
  		<tr>
  			<th>Member of Array Class</th>
  			<th>Meaning in Life</th>
  		</tr>
  	</thead>
  	<tbody>
  		<tr>
  			<td>Clear()</td>
  			<td>This <strong>static</strong> method sets a range of elements in the array to empty values (0 for numbers, null for object references, false for Booleans).</td>
  		</tr>
  		<tr>
  			<td>CopyTo()</td>
  			<td>This method is used to copy elements from the source array into the destination array.</td>
  		</tr>
  		<tr>
  			<td>Length</td>
  			<td>This property returns the number of items within the array.</td>
  		</tr>
  		<tr>
  			<td>Rank</td>
  			<td>This property returns the number of dimensions of the current array.</td>
  		</tr>
  		<tr>
  			<td>Reverse()</td>
  			<td>This <strong>static</strong> method reverses the contents of a one-dimensional array.</td>
  		</tr>
  		<tr>
  			<td>Sort()</td>
  			<td>This <strong>static</strong> method sorts a one-dimensional array of intrinsic types. If the elements in the array implement the IComparer interface, we can also sort your custom types.</td>
  		</tr>
  	</tbody>
  </table>

A few examples:

```c#
string[] gothicBands = {"Tones on Tail", "Bauhaus", "Sisters of Mercy"};

Array.Reverse(gothicBands);
// gothicBands is reversed in place

// Clear out all but the first member.
Array.Clear(gothicBands, 1, 2);
```



## Methods

* Methods can be implemented within the scope of classes or structures (as well as prototyped within interface types) and may be decorated with various keywords (e.g., static, virtual, public, new) to qualify their behavior. 

```c#
// static returnType MethodName(paramater list) { /* Implementation */ }
static int Add(int x, int y)
{
	return x + y;
}
```



### Return Values and Expression Bodied Members

* C# 6 introduced expression-bodied members that shorten the syntax for single-line methods. The above method can be re-written as

```c#
static int Add(int x, int y) => x + y;
```

* C# 7 expanded this capability to include single-line constructors, finalizers, and get and set accessors on properties and indexer.
* The above is a syntactic sugar, meaning that the generated IL is no different.



### Method Parameter Modifiers

* The default manner in which a parameter is sent into a function is by value.

<table>
	<thead>
		<tr>
			<th>Parameter Modifier</th>
			<th>Meaning in Life</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>(None)</td>
			<td>If a parameter is not marked with a parameter modifier, it is assumed to be passed by value, meaning the called method receives a copy of the original data.</td>
		</tr>
		<tr>
			<td>out</td>
			<td>Output parameters must be assigned by the method being called and, therefore, are passed by reference. If the called method fails to assign output parameters we are issued a compiler error.</td>
		</tr>
		<tr>
			<td>ref</td>
			<td>The value is initially assigned by the caller and may be optionally modified by the called method (as the data is also passed by reference). No compiler error is generated if the called method fails to assign a ref parameter.</td>
		</tr>
		<tr>
			<td>params</td>
			<td>This parameter modifier allows us to send in a variable number of arguments as a single logical parameter. A method can have only a single params modifier, and it must be the final parameter of the method. We should avoid using the params modifier; however, it is to be noted that numerous methods within the base class libraries do make use of this C# language feature.</td>
		</tr>
	</tbody>
</table>



#### The `out` Modifier

* Methods that have been defined to take output parameters (via the `out` keyword) are under obligation to assign them to an appropriate value before exiting the method scope (if we fail to do so, we will receive compiler errors).

```c#
static void Add(int x, int y, out int ans)
{
	ans = x + y; // must assign a value to the ouput parameter
}

// Inside Calling Function
Add(90, 90, out int ans); // No need to assign `ans`
Console.WriteLine("90 + 90 = {0}", ans);
```

* Calling a method with output parameters also requires the use of the `out` modifier. 

* The local variables that are passed as output variables are not required to be assigned before passing them in as output arguments. If done so after the function call we lose the assigned value.

* Starting with C# 7, out parameters do not need to be declared before using them. In other words, they can be declared inside the method call (as shown in the example).

* `out` is one of the ways to return multiple values from a method.

*  If we don’t care about the value of an `out` parameter, we can use a discard (`_`) as a placeholder.

  ```c#
  if (DateTime.TryParse(dateString, out _)
  {
  	//do something
  }
  ```



---

#### Discards

Discards are temporary, dummy variables that are intentionally unused. They are unassigned, don’t have a value, and might not even allocate any memory. This can provide a performance benefit but, at the least, can make your code more readable. Discards can be used with out parameters, with tuples, with pattern matching, or even as stand-alone variables.

---



#### The `ref` Modifier

* Reference parameters are necessary when we want to allow a method to operate on (and usually change the values of) various data points declared in the caller’s scope (such as a sorting or swapping routine).
* The distinction between `ref` and `out` are as follows:
  1. Output parameters do not need to be initialized before they are passed to the method. The reason for this is that the method must assign output parameters before exiting.
  2. Reference parameters must be initialized before they are passed to the method. The reason for this is that you are passing a reference to an existing variable. If we don’t assign it to an initial value, that would be the equivalent of operating on an unassigned local variable.

```c#
public static void SwapStrings(ref string s1, ref string s2)
{
    string tempStr = s1;
    s1 = s2;
    s2 = tempStr;
}

// In the calling function
string str1 = "Flip", str2 = "Flop"; // refs have to be initialized
SwapStrings(ref str1, ref str2);
```



##### `ref` Locals and Returns

*  C# 7 introduces the ability to use and return references to variables defined elsewhere.

* The following example illustrates the need to return references:

  ```c#
  public static string SimpleReturn(string[] strArray, int position)
  {
  	return strArray[position]; // Returns by value
  }
  
  // In callig function
  string[] stringArray = { "one", "two", "three" };
  int pos = 1;
  Console.WriteLine("Before: {0}, {1}, {2} ", stringArray[0], stringArray[1], stringArray[2]);
  var output = SimpleReturn(stringArray, pos); // The output is modified
  output = "new";
  Console.WriteLine("After: {0}, {1}, {2} ", stringArray[0], stringArray[1], stringArray[2]); // But the still holds original values 
  
  /*
  Before: one, two, three
  After: one, two, three
  */
  ```

* To solve it instead of a straight return [value to be returned], the method must do a return ref [reference to be returned]. The second change is that the method declaration must also include the ref keyword.

* Calling this method also requires the use of the `ref` keyword, both for the return variable and for the method call itself.

  ```c#
  // Returning a reference.
  public static ref string SampleRefReturn(string[] strArray, int position)
  {
  	return ref strArray[position];
  }
  
  // Calling method returning a reference
  ref var refOutput = ref SampleRefReturn(stringArray, pos);
  refOutput = "new";
  
  // Output if this function is used
  /*
  Before: one, two, three
  After: one, new, three
  */
  ```

* The following rules apply in case of returning a reference:

  1. Standard method results cannot be assigned to a ref local variable. The method must have been created as a ref return method.

  2. A local variable inside the ref method can’t be returned as a ref local variable.
     The following code does not work:

     ```c#
     ThisWillNotWork(string[] array)
     {
     	int foo = 5;
     	return ref foo;
     }
     ```

  3. This new feature doesn’t work with 'async' methods. 



#### The `params` Modifier

* The `params` keyword allows you to pass into a method a variable number of identically typed parameters (or classes related by inheritance) as a single logical parameter.
* Arguments marked with the `params` keyword can be processed if the caller sends in a strongly typed array or a comma- delimited list of items.

```c#
static double PrintListOfDoubles(params double[] values)
{
	Console.WriteLine("You sent me {0} doubles.", values.Length);
}

// Both are valid
PrintListOfDoubles(4.0, 3.2, 5.7, 64.22, 87.2);

double[] data = { 4.0, 3.2, 5.7 };
PrintListOfDoubles(data);
```

* To avoid any ambiguity, C# demands a method support only a single `params` argument, which must be the final argument in the parameter list.



#### Optional Parameters

* As with C++ functions, C# methods also take optional parameters

  ```c#
  static void EnterLogData(string message, string owner = "Programmer")
  {
      Console.WriteLine("Error: {0}", message);
      Console.WriteLine("Owner of Error: {0}", owner);
  }
  ```

* The value assigned to an optional parameter must be known at compile time and cannot be resolved at runtime.

* To avoid ambiguity, optional parameters must always be packed onto the end of a method signature. It is a compiler error to have optional parameters listed before non-optional parameters.



#### Invoking Methods Using Named Parameters

* These feature is useful if all parameters of a method are optional. Given that each argument has a default value, named arguments allow the caller to specify only the parameters for which they do not want to receive the defaults.
* Named arguments allow us to invoke a method by specifying parameter values in any order we choose.

```c#
static void DisplayFancyMessage(ConsoleColor textColor, ConsoleColor backgroundColor, string message)
{
    ...
}

// Invoking using named parameters
DisplayFancyMessage(message: "Wow! Very Fancy indeed!",
textColor: ConsoleColor.DarkRed,
backgroundColor: ConsoleColor.White);
```

* If we begin to invoke a method using positional parameters, you must list them before any named parameters. 

```c#
// This is an ERROR, as positional args are listed after named args.
DisplayFancyMessage(message: "Testing...",
backgroundColor: ConsoleColor.White,
ConsoleColor.Blue);

// This is OK, as positional args are listed before named args.
DisplayFancyMessage(ConsoleColor.Blue,
message: "Testing...",
backgroundColor: ConsoleColor.White);
```



### Method Overloading

* Identically named methods that differ by the number (or type) of parameters, the method in
  question is said to be *overloaded*.

```c#
class Program
{
    static void Main(string[] args)
    {
    }
    
    // Overloaded Add() method.
    static int Add(int x, int y)
    { return x + y; }
    
    static double Add(double x, double y)
    { return x + y; }
    
    static long Add(long x, long y)
    { return x + y; }
}
```



---

### Local Functions

* In C# 7 we can create methods within methods, and it is referred to officially
  as local functions. 
* A local function is a function declared inside another function.

```C#
// Instead of doing this
static int Add(int x, int y)
{
	return x + y;
}

// We May like to do this
static int AddWrapper(int x, int y)
{
    return Add();
    
    int Add()
    {
    	return x + y;
    }
}
```

* This feature was added into the C# specification for custom iterator methods and asynchronous methods.

---



## The Enum type

* Enums are symbolic names that map to known numerical values.
* Enum with enumerator are completely different concepts. An enum is a custom data type of name-value pairs. An enumerator is a class or structure that implements a .NET interface named `IEnumerable`. 

```c#
enum EmpType
{
    Manager, // = 0
    Grunt = 103,
    Contractor, // = 104
    VicePresident // = 105
}

static void Main()
{
    Emptype em = EmpType.Contractor;
    Console.WriteLine(em);		// Contractor
    Console.WriteLIne(Convert.ToInt32(em)); // 104
}
```

* Enumerations do not necessarily need to follow a sequential ordering and do not need to have unique values.
  * Enums like Classes are types (Enum is a value type), however unlike classes (which is a reference type) enums can't be created as static types. Enums if accesible can be instatiated as follows

    ```c#
    class Program
    {
        public enum EmpType { Manager = 103, Grunt = 103, Contractor, VicePresident }
        
        // .... 
    }
    
    class TestClass
    {
        Program.EmpType em = Program.EmpType.Contractor;
        // ...
    }
    ```



### Underlying Storage for an enum

* By default, the storage type used to hold the values of an enumeration is a System.Int32 (the C# int); however, we are free to change this.

* To set the underlying storage value of EmpType to some othert type of number say a  byte rather than an int

  ```c#
  enum EmpType : byte
  {
      Manager = 10,		// Values should be within range else compile time error will be generated
      Grunt = 1,
      Contractor = 100,
      VicePresident = 9
  }
  ```

* The following is to be noted:

  ```c#
  EmpType em = (EmpType)"Grunt"; // Error
  EmpType em = (EmpType)103 // OK Even if there is no name for 103
  ```

* To get the name and value of an enum variable programmatically we may use

  ```c#
  Console.WriteLine("{0} = {1}", emp.ToString(), (byte)emp);
  ```

  

### The `System.Enum` Type

* .NET enumerations gain their functionality from the `System.Enum` class type.

* A static method `Enum.GetUnderlyingType()` returns the data type used to store the values of the enumerated type.

  ```c#
  Console.WriteLine(Enum.GetUnderlyingType(em.GetType()));
  // ->  System.Byte
  ```

  * In place of `emp.GetType()`, we may want to use the `typeof()` operator. In that case we don't need a variable.

    ```c#
    Console.WriteLine(typeof(EmpType));
    ```

* `System.Enum` has a static method `GetValues()`.  This method returns an instance of `System.Array`. Each item in the array corresponds to a member of the specified enumeration.

  ```c#
  Array enumData = Enum.GetValues(em.GetType());
  foreach(var x in enumData)
  {
      System.Console.WriteLine(x);
  }
  
  /*
  Grunt	
  Grunt			// In case we have same values for both names
  Contractor
  VicePresident
  */
  ```



## The Structure Type

* Structures are types that can contain any number of data fields and members that operate on these fields.
* Structure supports encapsulation but cannot be used to build a family of related types. (It doesn't support inheritance).

```c#
struct Point
{
    // Fields of the structure. (It can be private as well)
    public int X;
    public int Y;
    
    // Add 1 to the (X, Y) position.
    public void Increment()
    {
    	X++; Y++;
    }
    
    // Display the current position.
    public void Display()
    {
   		Console.WriteLine("X = {0}, Y = {1}", X, Y);
    }
}

static void Main()
{
    Point myPoint;
    myPoint.X = 349;
    myPoint.Y = 76;
    myPoint.Display();
}
```

* If we do not assign each piece of public field data (X and Y in this case) before using the structure, we may receive a compiler error. (Depends on C# version) [^1]

* To avoid this we may instantiate the structure using the `new` keyword which uses the default constructor of the structure.

  ```c#
  Point p1 = new Point();
  
  p1.Display();
  // X = 0, Y = 0
  ```

  * We may also have a customised constructor as well. Remember that by the time we are done with the constructor we must <u>fully assign values to all the fields</u> of the structure whether they are private or public.

    ```c#
    struct Point
    {
        // Fields of the structure.
        int X;
        int Y;
        // A custom constructor.
        public Point(int XPos, int YPos)
        {
            X = XPos;
            Y = YPos;
        }
        ...
    }
    ```



## Value types and Reference Type

The following outline provides an overview of C#'s type system.

- [Value types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types)
  - Simple types
    - Signed integral: `sbyte`, `short`, `int`, `long`
    - Unsigned integral: `byte`, `ushort`, `uint`, `ulong`
    - Unicode characters: `char`
    - IEEE binary floating-point: `float`, `double`
    - High-precision decimal floating-point: `decimal`
    - Boolean: `bool`
  - [Enum types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/enum)
    - User-defined types of the form `enum E {...}`
  - [Struct types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct)
    - User-defined types of the form `struct S {...}`
  - [Nullable value types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/nullable-value-types)
    - Extensions of all other value types with a `null` value
  - [Tuple value types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples)
    - User-defined types of the form `(T1, T2, ...)`
- [Reference types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/reference-types)
  - [Class types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/class)
    - Ultimate base class of all other types: `object`
    - Unicode strings: `string`
    - User-defined types of the form `class C {...}`
  - [Interface types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface)
    - User-defined types of the form `interface I {...}`
  - [Array types](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/)
    - Single- and multi-dimensional, for example, `int[]` and `int[,]`
  - Delegate types
    - User-defined types of the form `delegate int D(...)`



### Notes on Value and Reference types

* A variable of a value type contains an instance of the type while variables of reference types store references to their data (objects).
* C# structures do not have an identically named representation in the .NET library (that is, there is no `System.Structure` class) but are implicitly derived from `System.ValueType`. Simply put, the role of `System.ValueType` is to ensure that the derived type (e.g., any structure) is allocated on the stack, rather than the garbage-collected heap i.e. to override the virtual methods defined by `System.Object` to use value-based versus reference-based semantics.



#### Assignment

* By default, on [assignment](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/assignment-operator), passing an argument to a method, and returning a method result, variable values are copied. In the case of value-type variables, the corresponding type instances are copied. 

* With reference types, two variables can reference the same object; therefore, operations on one variable can affect the object referenced by the other variable. With value types, each variable has its own copy of the data, and it is not possible for operations on one variable to affect the other (except in the case of in, ref and out parameter variables).

* As discussed in previous point value based types are always declared in stack even if `new` keyword is used.

  ```c#
  Point p1 = new Point(10, 10);
  Point p2 = p1;
  
  p1.X = 100;
  
  p1.Display();
  // X = 100, Y = 10
  p2.Display();
  // X = 10, Y = 10
  ```

  If we implement point using a class both the variables will show the change.



#### Value Type Containing a Reference Type

* If a value type contains a data member of a reference type, only the reference to the instance of the reference type is copied when a value-type instance is copied. Both the copy and original value-type instance have access to the same reference-type instance.  In this way, we have two independent structures (value-type), each of which contains a reference pointing to the same object in memory (i.e., a shallow copy). 
  * `ICloneable` interface can be implemented to perform a deep copy in such a situation.



#### Passing Reference Types by value and by reference

* If a reference type is passed by reference, the callee may change the values of the object’s state data, as well as the object it is referencing.
* If a reference type is passed by value, the callee may change the values of the object’s state data but not the object it is referencing.



#### Summary

<table>
	<thead>
		<tr>
			<th>Question</th>
			<th>Value Type</th>
			<th>Reference Type</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Where are objects allocated?</td>
			<td>Allocated on the stack.</td>
			<td>Allocated on the managed heap.</td>
		</tr>
		<tr>
			<td>How is a variable represented?</td>
			<td>Value type variables are local copies.</td>
			<td>Reference type variables are pointing to the memory occupied by the allocated instance.</td>
		</tr>
		<tr>
			<td>What is the base type?</td>
			<td>Implicitly extends System.ValueType.</td>
			<td>Can derive from any other type (except System.ValueType), as long as that type is not “sealed”</td>
		</tr>
		<tr>
			<td>Can this type function as a base to other types?</td>
			<td>No. Value types are always sealed and cannot be inherited from.</td>
			<td>Yes. If the type is not sealed, it may function as a base to other types.</td>
		</tr>
		<tr>
			<td>What is the default parameter passing behavior?</td>
			<td>Variables are passed by value (i.e., a copy of the variable is passed into the called function).</td>
			<td>For reference types, the reference is copied by value.</td>
		</tr>
		<tr>
			<td>Can this type override System.Object.Finalize()?</td>
			<td>No.</td>
			<td>Yes, indirectly.</td>
		</tr>
		<tr>
			<td>Can I define constructors for this type?</td>
			<td>Yes, but the default constructor is reserved (i.e., the custom constructors must all have arguments).</td>
			<td>Yes.</td>
		</tr>
		<tr>
			<td>When do variables of this type die?</td>
			<td>When they fall out of the defining scope.</td>
			<td>When the object is garbage collected.</td>
		</tr>
	</tbody>
</table>


## Nullable Types

* Value types can never be assigned the value of `null`, as that is used to establish an empty object reference.
* To address this a nullable type can represent all the values of its underlying type, plus the value `null`.
* To define a nullable variable type, the question mark symbol (?) is suffixed to the underlying datatype. Do note that this syntax is legal only when applied to value types.

```c#
int? nullableInt = 10;
```

* In C#, the ? suffix notation is a shorthand for creating an instance of the generic `System.Nullable<T>` structure type which provides a set of members that all nullable types can make use of. Thus the following declaration is valid

```c#
Nullable<int> nullableInt = 10;
```



### Null Coalescing Operator

* Any variable that might have a `null` value (i.e., a reference-type variable or a nullable value-type variable) can make use of the C# `??` operator, which is formally termed the null coalescing operator. This operator allows us to assign a value to a nullable type if the retrieved value is in fact null.

```c#
int myData = dr.GetIntFromDatabase() ?? 100;
```



### Null Conditional Operator

* The following will result a runtime exception

  ```c#
  int[] arr = null;
  System.Console.WriteLine($"The Length of array is { arr.Length }");
  ```

* null conditional operator token (a question mark placed after a variable type but before an access operator) can be used to avoid a runtime error

  ```c#
  int[] arr = null;
  System.Console.WriteLine($"The Length of array is { arr?.Length }");
  // The Length of array is 
  int[] arr = null;
  System.Console.WriteLine($"The Length of array is { arr?.Length ?? 0 }");
  // The Length of array is 0
  ```



## Tuples

* Tuples, which are lightweight data structures that contain multiple fields, were actually added in C# 6 but in a very limited way. In C# 7, tuples use the new `ValueTuple` data type instead of reference types, potentially saving significant memory. The `ValueTuple` data type creates different structs based on the number of properties for a tuple. An additional feature added in C# 7 is that each property in a tuple can be assigned a specific name.

* Tuple can be declared as

  ```c#
  (string, int, string) values = ("a", 5, "c");
  var values = ("a", 5, "c");
  ```

   they don’t all have to be the same data type.

* By default, the compiler assigns each property the name `ItemX`, where X represents the one based position in the tuple.

  ```c#
  Console.WriteLine($"First item: {values.Item1}");
  // First item: a
  ```

* Specific names can also be added to each property in the tuple on either the right side or the left side of the statement. While it is not a compiler error to assign names on both sides of the statement, if we do, the right side will be ignored, and only the left-side names are used.

  ```c#
  (string FirstLetter, int TheNumber, string SecondLetter) valuesWithNames = ("a", 5, "c");
  var valuesWithNames2 = (FirstLetter: "a", TheNumber: 5, SecondLetter: "c");
  ```

  Now the properties on the tuple can be accessed using the field names as well as the ItemX notation.

* When setting the names on the right, we must use the keyword `var`. Setting the data typesspecifically (even without custom names) triggers the compiler to use the left side, assign the properties using the ItemX notation, and ignore any of the custom names set on the right.

  ```c#
  (int, int) example = (Custom1:5, Custom2:7);
  (int Field1, int Field2) example = (Custom1:5, Custom2:7);
  ```

  Custom1 and Custom2 names are ignored here.

* Custom field names exist only at compile time and aren’t available when inspecting the tuple at runtime using reflection.

* C# 7.1 allows to infer the variable names of tuples under certain conditions.

  ```c#
  var foo = new {Prop1 = "first", Prop2 = "second"};
  var bar = (foo.Prop1, foo.Prop2);
  // The follwoing Line will give an error if C# 7.1 is not used
  Console.WriteLine($"{bar.Prop1};{bar.Prop2}");
  ```



### Tuples as Method return values

```c#
static (int a,string b,bool c) FillTheseValues()
{
	return (9,"Enjoy your string.",true);
}

var samples = FillTheseValues();
Console.WriteLine($"Int is: {samples.a}");
Console.WriteLine($"String is: {samples.b}");
Console.WriteLine($"Boolean is: {samples.c}");
```



### Discards with Tuples

Suppose we don't care about the value of `string b`.  Discards (`_`) placeholder can be used for that.

```c#
var (first, _, last) = FillTheseValues();
Console.WriteLine($"{first}:{last}");
// 9:true
```

The string is simply discarded.



### Deconstructing Tuples

Deconstructing is the term given when separating out the properties of a tuple to be used individually.

```c#
struct Point
{
    // Fields of the structure.
    public int X;
    public int Y;
    
    // A custom constructor.
    public Point(int XPos, int YPos)
    {
        X = XPos;
        Y = YPos;
    }
    
    public (int XPos, int YPos) Deconstruct() => (X, Y);
}
```

Notice the new `Deconstruct()` method, which can be named anything, but by convention it is typically named `Deconstruct()`. This allows a single method call to get the individual values of the structure by returning a tuple.

```c#
Point p = new Point(7,5);
var pointValues = p.Deconstruct();
Console.WriteLine($"X is: {pointValues.XPos}");
Console.WriteLine($"Y is: {pointValues.YPos}");
```



---

[^1]: In the Current version of C# we may use the structure without assigning values to the one or all the fields, whether or not we have a constructor defiened. In such case the field has the default value assigned to it.




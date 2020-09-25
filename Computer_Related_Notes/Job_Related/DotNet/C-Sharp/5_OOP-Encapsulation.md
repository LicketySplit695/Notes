# OOP - Encapsulation in C#



# The pillars of OOP

The following are the 3 pillars of OOP and their motivation:

1. **Encapsulation**: Hide internal implementation & preserve data integrity.

   * Functions allow encapsulation of actions
   * keywords - `protected`, `private` etc. & properties (getters & setters) can help in data integrity.

2. **Inheritance**: Code reuse

   Allows:

   * "is - a" functionality (Circle 'is - a' shape etc).

   * "has - a" functionality (exposing the functionality of other class as if its own).

3. **Polymorphism**: Treat related objects in a similar way.

   * Function overloading
   * Overriding virtual members



# Encapsulation



## Class

* A class is a **user-defined** type that is composed of field data (often called member variables) and members that operate on this data (such as constructors, properties, methods, events, and so forth).

  *  The set of field data represents the “state” of a class instance (otherwise known as an object).

  ```c#
  class car
  {
      double speed;
      
      //Using the expression-bodied member syntax introduced in C# 6
      public void PrintState() => Console.WriteLine($"The speed is { speed }kmph.")
  }
  ```

* Objects must be allocated into memory using the `new` keyword.

  ```c#
  Car myCar = new Car();
  // or
  Car myCar; // A reference to a yet-to-be-determined Car object.
  myCar = new Car(); // Reference points to a valid object in memory.
  ```

  

## Constructors

* A constructor is a special method of a class that is called indirectly when creating an object using the `new` keyword. 
  * Constructors never have a return value <u>(not even void)</u> and are always named identically to the class they are constructing. 



### Default Constructor

* Every C# class is provided with a default constructor that we can redefine if need be. 
  * A default constructor <u>never takes arguments</u>.
  * After allocating the new object into memory, the default constructor ensures that all field data of the class is set to an appropriate default values.

```c#
class car
{
    // Custom Default Constructor
    public car()
    {
        speed = 1;
    }
}
```

* As soon as we define a custom constructor with any number of parameters, the default constructor is silently removed from the class and is no longer available.
* We must explicitly redefine the default constructor if we need an instance with the default constructor.



### Custom Constructors

* What makes one constructor different from another (in the eyes of the C# compiler) is its signature i.e., the number of and/or type of constructor arguments. (Method/Constructor Overloading).

```c#
public Car(double speed)
{	
    // Custom Constructor
	this.speed = speed;
}
```

* `Constructors`, `finalizers`, and get/set accessors on `properties` and `indexers` from C# 7 accept **Expression-Bodied Members**.

```c#
public Car(double speed) => this.speed = speed;
```

*  Expression bodied members are designed for one-line methods, i.e., we can't put a semicolon and write multiple statements within the body just yet.

---

#### Constructors with Optional & Named Arguments

Just as with methods, we can have constructors with optional arguments

```c#
public Motorcycle(int intensity = 0, string name = "") { }
```

or call a constructor with Named arguments

```c#
Motorcycle m2 = new Motorcycle(name:"Tiny");
```

---



## `this` Keyword

* `this` keyword that provides access to the current class **instance**. 
  * A possible use of the this keyword is to resolve scope ambiguity.
  * When a class wants to access its own data fields or members, a `this` is implied.



### Chaining constructor call using the `this` keyword

* We may want to use a *master constructor*, that performs certain common task that all the constructors that need to perform (We may also use a separate function for the same).

* In such a situation the remaining constructors can make use of the `this` keyword to forward the incoming arguments to the master constructor and provide any additional parameters as necessary.

  The following is an example to illustrate the above:

	```c# 
	class MotorCycle
	{
	    int driverIntensity;
		string driverName;
		
	    // Chaining constructors
	    public MotorCycle(int intensity)
		: this(intensity, "") { Console.WriteLine("Constructor with Int"); }
		public MotorCycle(string name)
		: this(0, name) {}
		
	    public MotorCycle(int intensity, string name)
		{
	        Console.WriteLine("Master Constructor");
			if (intensity > 10) intensity = 10;
			
	        driverIntensity = intensity;
			driverName = name;
	    }
	}
	```
	
	* Once a constructor passes arguments to the designated *master constructor* (and that constructor has processed the data), the constructor invoked originally by the caller will finish executing any remaining code statements. 
	  To illustrate, if we run the above example, we get the following output:
	
	  ```c#
	  MotorCycle m = new MotorCycle(11);
	  /*
	  Master Constructor
	  Constructor with Int
	  */
	  ```



## `static` Keyword

* The `static` members are **invoked directly from the class level**, rather than from an object reference variable. For example:

  ```c#
  Console.WriteLine("Hello World"); // -> WriteLine() is a static function of Console Class
  ```

* `static` can be:

  * Data

  * Method

  * Property

  * A Constructor

  * Entire Class

    

---

#### Where to use a `static` member

1. `static` member can be used for 'utlity' classes like `Console`. Classes which are meant to add functionalty to other classes.
2. `static` member can be used to denote a 'state' or 'functionality' which is common for all instances like 'interestRate' etc.

---



### `static` field data

* Static data, is allocated once and shared among all objects of the same class category. 

```c#
public static double currInterestRate = 0.04;
```

* When we create new instances of a class, the value of the static data is not reset, as the CLR allocates the static data into memory exactly one time. After that point, all objects of the class operate on the same value of the static field.
* `static` field is always auto initialized to the default value (0 for int, False for bool etc).



### `static` methods and properties

* `static` methods are useful to operate on `static` data member.
* It is a **compiler error** for a static member to reference nonstatic members in its implementation. It is an error to use the `this` keyword on a static member because `this` implies an object.

    ```c#
    class Program
    {
        int x = 4;
        // -> Illegal to use a non static member without object reference
        static void Main(){ Console.WriteLine(x);} 
        
        // -> to use `x` it must be declared `static`
    }
    ```



### `static`  constructors

* The CLR calls all static constructors before the first use (and never calls them again for that instance of the application)

* Useful to set the values of `static` data members.

* A given class may define only a single static constructor. In other words, the static constructor **cannot be overloaded**.

* A static constructor **does not take an access modifier** and cannot take any parameters.

* The runtime invokes the static constructor when it creates an instance of the class or before accessing the first static member invoked by the caller. The static constructor executes before any instance-level constructors.

  ```c#
  class Test
  {
      static int t;
      // -> No Access specifiers like `public`
      public static Test(int p) // -> NO Parameters
      {
          t = p;
      }
  }
  ```



### `static`  classes

* When a class has been defined as `static`, it is not created using the `new` keyword.
* It <u>can contain only members or data fields marked with the static keyword</u>. Else, we will receive compiler errors.
* 'Utility' classes can be created static.



---

#### Importing `static` members by `using` keyword

Post C# 6 we can import static members with the using keyword.

```c#
using static System.Console; //-> Console is a static class (We can  import static types only)

static void Main()
{
    WriteLine("Hello World");
}
```

In the code we don't need to write `Console` anymore. 

---



# Encapsulation specific tools



## Access Modifiers

The following are the access modifiers supported by C#.

<table>
    <thead>
        <tr>
            <th><b>Access Modifier</b></th>
            <th><b>May be Applied to</b></th>
            <th><b>Comments</b>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>public</b></td>
            <td>Types or type members</td>
            <td>Accessible everywhere.</td>
        </tr>
        <tr>
            <td><b>private</b></td>
            <td>Type members or nested types</td>
            <td>Only inside the class.</td>
        </tr>
        <tr>
            <td><b>protected</b></td>
            <td>Types or type members</td>
            <td>Accessible by class or any child class.</td>
        </tr>
        <tr>
            <td><b>internal</b></td>
            <td>Types or type members</td>
            <td>Accessible inside the <b>assembly</b>.</td>
        </tr>
        <tr>
            <td><b>protected internal</b></td>
            <td>Types or type members</td>
            <td>protected <b>or</b> internal.</td>
        </tr>
</table>

* Although CLR supports *protected and internal*, C# doesn't support it.



### Default Access Modifiers

* **Types**  (classes, interfaces, structures, enumerations, and delegates) are implicitly **internal**.
  * They may be `internal` or `public` only.
* **Members** (properties, methods, constructors, and fields)  are implicitly **private**.
  * They may be any thing.



### Access modifiers for Nested Types

* Nested type is implicitly private. We may apply any access modifier we may want.

```c#
// Inside Program class
TestClass.NestedEnum x = TestClass.NestedEnum.left; // -> error: Nested enum is private

class TestClass
{
    enum NestedEnum { left, right }
}
```



## Properties

* Properties are just a simplification for “real” getter and setter methods.

```c#
// Inside a class Employee
string name;

// business rule
public string Name
{
    get { return name; }
    set
    {
        if(value.Length > 30) Console.WriteLIne("Too long");
        else
            name = value;
    }
}

// no business rule
public string Name
{
    get { return name; }
    set { name = value; }
}

// In another class
Employee emp = new Employee();
emp.Name = "Angshuman";
Console.WriteLine(emp.Name);
```

* This token `value` is a <u>contextual keyword</u> i.e. not a true C# keyword. 

  * When the token value is within the set scope of the property, it always represents the value being assigned by the caller, and it will always be the same underlying data type as the property itself.

* It's advisable to write to field only through properties, even when inside the class, so that the business rule may be respected.

* `get` and `set` blocks may have access modifiers ([details](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/restricting-accessor-accessibility)).

  ```c#
  private string _name = "Hello";
  
  public string Name
  {
      get{ return _name; }
      protected set{ _name = value; }
  }
  ```

  Restrictions are as follows:

  * We cannot use accessor modifiers on an interface or an explicit [interface](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface) member implementation.
  * We can use accessor modifiers only if the property or indexer has **both** `set` and `get` accessors. In this case, the modifier is permitted on **only one** of the two accessors.
  * If the property or indexer has an [override](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/override) modifier, the accessor modifier must match the accessor of the overridden accessor, if any.
  * The accessibility level on the accessor must be **more restrictive** than the accessibility level on the property or indexer itself.



#### New Syntax for single expression properties

Note: Only **single-line** methods can be written using the new syntax.

```c#
public int Age
{
	get => empAge;
	set => empAge = value;
}
```



### Read-only and Write-only properties

* For a *read-only* property drop the `set` block.

  ```c#
  public string SocialSecurityNumber
  {
  	get { return empSSN; }
  }
  ```

* For a *write-only* property drop the `get` block.

  ```c#
  public string SocialSecurityNumber
  {
  	set { empSSN = value; }
  }
  ```

  

### `static` properties

* It's relevant for encapsulating static fields

  ```c#
  // Inside class SavingsAccount
  public static double InterestRate
  {
  	get { return currInterestRate; }
  	set { currInterestRate = value; }
  }
  ```

* They may be accessed at class level

  ```c#
  Console.WriteLine("Interest Rate is: {0}", SavingsAccount.InterestRate);
  ```

  

### Automatic Properties

* The following is an automatic property, which creates a field of same name (initialized by default value).

  ```c#
  public string PetName { get; set; }
  ```
  *  If we use automatic property syntax to wrap another class variable, it will also be set to a default value of `null`.

* If we want a property with business logic, we can't use automatic properties.

* Since C# 6, it is possible to define a “read-only automatic property” by omitting the set scope.
  However, it is **not** possible to define a **write-only property**.

  ```c#
  public int MyReadOnlyProp { get; } // -> OK
  ```



#### Initialiazing automatic properties

* Starting C# 6 the following is valid

  ```c#
  public int NumberOfCars { get; set; } = 1;
  public Car MyAuto { get; set; } = new Car();
  ```

  * We may achieve the same by initializing the properties inside constructor.




### Initializing objects with properties

* For classes that has public write-able properties or public fields, we may use the object initialization syntax:

  ```c#
  // Inside main method
  TestClass x = new TestClass{ firstname = "Angshuman", lastname = "Ghosh" };
  Console.WriteLine(x.firstname + ' ' + x.lastname);
  
  class TestClass
  {
      public string firstname{ get; set; }
      public string lastname;
  }
  ```

  * Behind the scenes, the type’s default constructor is invoked, followed by setting the values to the specified properties.

* We can call any constructor of the class along with the object initialization syntax.

  ```c#
  Point goldPoint = new Point(PointColor.Gold){ X = 90, Y = 20 }; // Object Initialization syntax with a constructor
  ```



## `const` field data

* `const` keyword defines constant data, which can never change after the initial assignment.
  * Initial value assigned to the constant must be specified at the time of definition **only**.
* Constant fields of a class are **implicitly static**. Thus they are called at the class level.

```c#
class MyMathClass
{
	public const double PI;
    
	public MyMathClass()
    {
        // Not possible- must assign at time of declaration.
        PI = 3.14;
    }
}
```



## `readonly` field data

* A read-only field cannot be changed after the initial assignment.
  * The value assigned to a read-only field can be determined at **runtime** and, therefore, can legally be assigned within the scope of a **constructor** but nowhere else.
* Unlike a constant field, read-only fields are **not implicitly static**.
* Any attempt to make assignments to a field marked `readonly` outside the scope of a constructor results in a compiler error.

```c#
class MyMathClass
{
    public readonly double PI;
    public MyMathClass ()
    {
    	PI = 3.14;
    }
    
    // Error!
    public void ChangePI() { PI = 3.14444;}
}
```



### Static read-only fields

* If a `readonly` field needs to be associated with the class and not with the object we may use `static readonly`.

  * To initialize such field we need static constructors.

  ```c#
  class MyMathClass
  {
  	public static readonly double PI;
  	
      static MyMathClass(){ PI = 3.14; }
  }
  ```

* If the value of `readonly` field is known at compile-time its better to use `const`.



## Partial Classes

* We may split the definition of class to multiple files using `partial` keyword before each definition

  ```c#
  // file1
  public partial class Employee
  {
      public void DoWork()
      {
      }
  }
  
  // file2
  public partial class Employee
  {
      public void GoToLunch()
      {
      }
  }
  ```

  * The `partial` keyword indicates that other parts of the class, struct, or interface can be defined in the namespace. 
  * All the parts must use the `partial` keyword. 
  * All the parts must be available at compile time to form the final type. 
  * All the parts must have the same accessibility, such as `public`, `private`, and so on.
  * If any part is declared abstract, then the whole type is considered abstract. If any part is declared sealed, then the whole type is considered sealed. If any part declares a base type, then the whole type inherits that class.

* We can also have partial methods. A detailed description can be found [here](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods).


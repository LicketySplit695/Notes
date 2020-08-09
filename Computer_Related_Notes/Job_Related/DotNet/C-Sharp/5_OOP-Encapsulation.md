# OOP - Encapsulation in C#



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
* `static` field is always auto initialzed to the default value (0 for int, False for bool etc).



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
using static System.Console; //-> Console is a static class

static void Main()
{
    WriteLine("Hello World");
}
```

In the code we don't need to write `Console` anymore. 

---


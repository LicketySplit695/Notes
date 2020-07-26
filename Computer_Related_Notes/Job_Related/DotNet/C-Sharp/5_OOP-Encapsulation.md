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

* `this` keyword that provides access to the current class instance. 
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




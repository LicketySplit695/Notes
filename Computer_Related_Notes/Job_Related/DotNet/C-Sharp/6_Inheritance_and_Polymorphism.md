# Inheritance & Polymorphism



# Inheritance and Code reuse

**Code reuse** can be done in two forms

1. Inheritance: "is - a" relationship
2. Containment - delegation: "has - a" relationship.



## Parent Class and Child Class

* The idea of inheritance is **Child Class "is - a" Parent Class** (represented by `:`).
  * For ex. if Employee is the parent class, and Manager is the child class, then Manager "is - a" Employee.
* Parent class is also called 'base class'. Child class is also called 'derived class'.

```c#
// Manager "is - a" Employee
class Manager: Employee
{ }
```

**Note:**

* In C# we only have public inheritance.
* Inheritance preserves encapsulation i.e. private members of parent class can never be accessed inside child class or from an object reference.
* The constructor of the base class is called **before** derived class comes into existence.
* A derived class **never** inherits the constructors of a parent class. The parent class constructor may be called from the child class.
* A given class have exactly **one** direct base class. Multiple inheritance with classes is not supported. A class or interface can inherit from multiple discreet interfaces.



### `protected` keyword

* Protected data or protected members are accessible by any descendant. They are not accessible by other classes.
* For the object user, protected data is regarded as private (as the user is “outside” the family).

```c#
class Employee
{
	protected int age;
    protected int name;
	
    // ... etc.
}
```



### `base` keyword

* To access public or protected members of the parent class, we may use the `base` keyword.

  * A common use is to call the constructor of the parent class to initialize fields of the parent class.

  ```c#
  public Manager(string name, int age, int noOfOptions)
  :base(name, age)
  {
      // StockOption is a property of Manager Class
      StockOption = noOfOptions;
  }
  ```

  * We may use any public or protected member of the parent class:

  ```c#
  // Inside Manager class
  public int findAge(){ return base.getAge(); }
  
  // Inside Employee Class
  public int getAge(){ return age; }
  ```



### `sealed` keyword

* A class marked with `sealed` can't be inherited.

  ```c#
  sealed class TemporaryEmployee: Employee{}
  
  // Error!!
  class AnotherEmployee: TemporaryEmployee{}
  ```

* Sealed classes are useful:

  * For designing utility classes
  * When a further inheritance logically makes no sense.

* C# structures are always implicitly sealed. Therefore, we can never derive one structure from another structure, a class from a structure, or a structure from a class.



## Containment - Delegation

* This model implements the **"has - a"** logic which in most cases is an alternative to inheritance.

* Having objects of a different class is containment. Exposing/Using the functionality of contained object as if its own is delegation.

* Delegation can be viewed as a relationship between objects where one object forwards certain method calls to another object, called its delegate.

  ```C#
  // Inside Main
  Printer pr = new Printer();
  pr.printMessage("Hello World");
  
  class Printer
  {
      // Contain a RealPrinter -> The "delegator"
      RealPrinter p = new RealPrinter();
  	
      // Delegating -> Exposing the function if RealPrinter
      public void printMessage(string message)
      {
          p.print(message);
      }
  }
  
  class RealPrinter
  {
      // The delegate
      public void print(string message)
      {
          Console.WriteLine(message);
      }
  }
  ```

  * This may be done by inheritance as well. See [here](https://www.geeksforgeeks.org/delegation-vs-inheritance-java/).



## Nested Type

* Type (`enum`, `class`, `interface`, `struct`, or `delegate`) which are defined directly within the scope of a <u>class</u> or <u>structure</u>.
* The nested (or “inner”) type is considered a member of the nesting (or “outer”) class and in the eyes of the runtime can be manipulated like any other member (fields, properties, methods, and events).

```c#
// Inside Main
OuterClass.PublicInnerClass p = new OuterClass.PublicInnerClass();

// Error!
OuterClass.PrivateInnerClass p = new OuterClass.PrivateInnerClass();

public class OuterClass
{
    // A public nested type can be used by anybody.
    public class PublicInnerClass {}
    
    // A private nested type can only be used by members of the containing class.
    private class PrivateInnerClass {}
}
```

**Note:**

* Nested types allow you to gain complete control over the access level of the inner type.
* Often, a nested type is useful only as a helper for the outer class and is not intended for use by the outside world.
* We must qualify nested type by the scope of the nesting type. 
* The nesting process can be as “deep” as we require.



# Polymorphism

## Method Overriding

### `virtual` and `override` keywords

*  If methods in base class that **may** be (but does not have to be) overridden by a child class (*virtual* methods), then it has to be marked by a `virtual` keyword.
* If the child class wants to change the implementation of a virtual method, it can be done by `override` keyword.

```c#
//Main method
Printer pr = new Printer();
pr.print("Hello World");

class Printer: RealPrinter
{
    public override void print(string message)
    {
        base.print(message);
        Console.WriteLine("Printer class: " + message);
    }
}

class RealPrinter
{
    public virtual void print(string message)
    {
        Console.WriteLine("RealPrinter class: " + message);
    }
}

/*
RealPrinter class: Hello World
Printer class: Hello World
*/
```

* If we dont provide `virtual` or `override` keyword, a warning will be shown saying that to hide the base class mthood explicitly, use the `new` keyword.



### Sealing virtual members

* Sealed methods can't be overridden. Doing so generates a compile-time error.

```c#
class SalesPerson : Employee
{
    ...
    public override sealed void GiveBonus(float amount) // Its defined in the employee class as virtual.
    {
    ...
    }
}

// Can't override GiveBonus in classes that inherit SalesPerson
```



## Abstract Class

* We may define a class as `abstract` if the entity it doesn't define a tangible entity. For ex. *shape*.
* Abstract classes 
  * Can't be implemented on its own.
  * **May** have an *abstract* method. However an *abstract* method **must** be inside an *abstract* class.
  * The class inheriting an abstract class **must** implement all the **abstract** members. If some abstract members are not implemented, then the implementing class must be abstract as well.
  * can have any number of constructors.
* Abstract methods/members:
  * do not provide any implementation
  * can't be private.
  * has to be explicitly overridden using the `override` keyword. Method hiding can't be used to implement the abstract method. While overriding the access specifier of the abstract method can't be changed.
* Creating references of abstract class is perfectly legal.

```c#
square sq = new square("sq_1");
sq.draw();

class square: shape
{
    public square(string name): base(name) {}

    public override void draw() // explicitly overriding
    {
        Console.WriteLine($"A { Name } square is drawn.");
    }
}

abstract class shape
{
    public shape(string name) => Name = name;

    public string Name { get; set; }

    public abstract void draw();
}

// A sq_1 square is drawn.
```



### Shadowing or hiding

* To reimplement a method of a base class we should use the `new` keyword. If not used we will get a warning.

```:Wc#
class square: shape
{
    public square(string name): base(name) {}

    public new void draw()
    {
        Console.WriteLine($"A { Name } square is drawn.");
    }
}

class shape
{
    public shape(string name) => Name = name;

    public string Name { get; set; }

    public void draw()
    {
        Console.WriteLine("Shape Class");
    }
}
```

* We can apply the `new` keyword to any member type inherited from a base class (field, constant, static member, or property).

* To trigger the base class implementation of a shadowed member we can use an explicit cast.

  ```c#
  square sq = new square();
  ((shape)sq).draw();
  ```



## Base/derived class casting

* 'Implicit cast' is allowed. When two classes are related by an “is-a” relationship, it is always safe to store a *derived object* within a *base class* reference.

    ```c#
    shape sh = new square("sq_1"); // valid
    sh.draw();      //"Shape Class"
    ```
  * Note that in the above example although the `sh` is instantiated by the **child class**, but since the **reference** is of the <u>parent class</u> we see that the parent class member is executed. We can <u>only use the members of the referenced class</u> (`shape` here).

* Explicit cast needs to be applied when we want to use the member of a child class:

    ```c#
    object sh = new square("sq_1");
    sh.draw();  // Error
    ((square)sh).draw(); // OK
    ```
    * Explicit casting is **evaluated at runtime**. Thus the following code is valid at the compile-time:
      
        ```c#
        object frank = new Manager();   // Manager is-an Employee
        Hexagon hex = (Hexagon)frank;   // Hexagon is-a shape
        ```



### `as` Keyword

* `as` keyword **type-casts** an object into a type (both reference and nullable) if possible. If not it returns a `null` (which can be checked to determine if the casting was successfull).

    ```c#
    object[] things = new object[4];
    things[0] = new Hexagon();
    things[1] = false;
    things[2] = new Manager();
    foreach (object item in things)
    {
        Hexagon h = item as Hexagon;    // Cast to Hexagon if possible
        if (h == null) Console.WriteLine("Item is not a hexagon");
        else h.Draw();
    }
    ```
    
    * Unlike `cast` expressions an `as` doesn't throw and exception.



### `is` Keyword

* The `is` operator checks if the runtime type of an expression result is compatible with a given type. If its is compatible the expression returns `true`.

    ```c#
    public class Base { }
    public class Derived : Base { }
    public static class IsOperatorExample
    {
        public static void Main()
        {
            object b = new Base();
            Console.WriteLine(b is Base);  // output: True
            Console.WriteLine(b is Derived);  // output: False
    
            object d = new Derived();
            Console.WriteLine(d is Base);  // output: True
            Console.WriteLine(d is Derived); // output: True
        }
    }
    ```

* Beginning C# 7 we also have the following:

    ```
    E is T v
    ```

    * If the result of E is non-null and can be converted to T by a reference, boxing, or unboxing conversion, the E is T v expression returns true and the converted value of the result of E is assigned to variable v.

        ```c#
        if(emp is Manager m)
        {
            Console.WriteLine($"{ emp.name } has { m.stockOptions } stocks");
        }
        ```

    * This new step avoids the "double-cast" problem of traditional `is` keyword. 



## `System.Object` in .NET

* Every type ultimately derives from a base class named `System.Object` which is represented by C# `object` keyword.
* The interface of the `object` can be represented as:

    ```c#
    public class object
    {
        // Virtual Memebers
        public virtual bool Equals(object obj);
        protected virtual void Finalize();
        public virtual int GetHashCode();
        public virtual string ToString();

        // Instance-level members
        public Type GetType();
        protected object MemberwiseClone();

        // static members
        public static bool Equals(object objA, object objB);
        public static bool ReferenceEquals(object objA, object objB);
    }
    ```



* To test thw above we use the following example class 

    ```c#
    class Person
    {
        public string FirstName { get; set; } = "";
        public string LastName { get; set; } = "";
        public int Age { get; set; }
        public Person(string fName, string lName, int personAge)
        {
            FirstName = fName;
            LastName = lName;
            Age = personAge;
        }
        public Person(){}
    }
    ```



### `ToString()` method

* It should return a string textual representation of the type’s current state.



#### Overriding `ToString()`

* A proper ToString() override should also account for any data defined up the chain of inheritance.
* For custom base class, we may want to obtain the `ToString()` of the base class using `base` keyword. We can use that in our child class.

    ```c#
    public override string ToString() => $"[First Name: {FirstName}; Last Name: {LastName}; Age: {Age}]";
    ```
    * The above convention is followed in many library classes.



### `Equals()` method

* The default behavior of Equals() is to test (by returning `true`) whether two variables are pointing to the same object in memory.

    ```c#
    Person p1 = new Person();
    Person p2 = p1;
    object o = p2;
    if (o.Equals(p1) && p2.Equals(o))
    {
        Console.WriteLine("Same instance!");
    }
    //Same instance
    ```

* If we intend to override `Equals()` we should override `GetHashCode()` as well.



#### Overriding `Equals()`

```c#
public override bool Equals(object o)
{
    if(o != null && o is Person p)
    {
        return (p.FirstName == this.FirstName && p.LastName == this.LastName && p.Age == this.Age);
    }

    return false;
}
```



### `GetHashCode()` function

* A hash code is a numerical value that represents an object as a *particular state*.
* By default, `System.Object.GetHashCode()` uses your object’s **current location in memory** to yield the hash value.
    * Thus we should override the `GetHashCode()` function whenever we are overriding the `Equals()` method.
* The `GetHashCode()` method should reflect the `Equals` logic; the rules are:
    1. if two things are equal (`Equals(...) == true`) then they must return the same value for `GetHashCode()`
    2. if the `GetHashCode()` is equal, it is not necessary for them to be the same; this is a collision, and `Equals` will be called to see if it is a real equality or not.
* What value should be used to override the hashCode is not always the same. A common example is as follows:

    ```c#
    public override int GetHashCode()
    {
        return base.GetHashCode();
    }
    ```
    * The following [thread](https://stackoverflow.com/questions/371328/why-is-it-important-to-override-gethashcode-when-equals-method-is-overridden) may be consulted.



### The static functions of `System.Object`

#### `object.Equals(object, object)`

* Value based equality. Behaves same as instance based virtual function.



#### `object.ReferenceEquals(object. object)`

* Reference based equality checker. If two objects are of different instance, just same in value yields `false`, although `Equals()` return `true`.

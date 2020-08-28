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

```c#
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

  
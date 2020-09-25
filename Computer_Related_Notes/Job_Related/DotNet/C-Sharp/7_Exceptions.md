# Exception Handling



### Notes

* Exceptions are runtime anomalies over which a programmer has little control (for ex. database connection that no longer exists etc.).
* The .NET platform provides a standard technique to send and trap runtime exceptions which is common to all languages.
* The syntax used to throw and catch exceptions across assemblies and machine boundaries is identical.
* Exceptions are objects in .NET.



## `System.Exception` Base Class

* All exceptions derive from `System.Exception` base class.
* The interface of the class (public members) are as follows:
  
  ```c#
  public class Exception: ISerializable, _Exception
  {
      // Constructors
      public Exception(string message, Exception innerException);
      public Exception(string message);
      public Exception();
  ...
  
      // Methods
      public virtual Exception GetBaseException();
      public virtual void GetObjectData(SerializationInfo info, StreamingContext context);

      // Properties
      // Collection of key-value pairs for additional programmer defined information, empty by default
      public virtual IDictionary Data { get; }
      // Programmer defined URL for additional help.
      public virtual string HelpLink { get; set; }
      // Information about the previous exception that caused the current exception to occur.
      // The can be recorded by passing into the constructor.
      public Exception InnerException { get; }
      // Message can be passed in as the constructor.
      public virtual string Message { get; }
      // This property gets or sets the name of the assembly, or the object, that threw the current exception.
      public virtual string Source { get; set; }
      // StackTrace is a string
      public virtual string StackTrace { get; }
      public MethodBase TargetSite { get; }
  ...
  }
  ```



## `try`, `catch` and `throw`

* `try` block is a section of statements that may throw an exception during execution.
* If an exception is detected the control flows to the **appropriate** `catch` block.
* If the code within a try block does not trigger an exception, the catch block is *skipped entirely*.
* After an exception has been handled, the application is free to continue on from the point *after* the `catch` block.

    ```c#
    int x1 = 1, x2 = 0, n;

    try
    {
        n = x1 / x2;
    }
    catch(Exception ex)     // Error -> We need to have more specific exception class to the top
    {
        Console.WriteLine("Test");
    }
    catch(DivideByZeroException ex)
    {
        Console.WriteLine(ex.Message);
    }
    ```
* We can *throw* an exception type object.
* Prior to C# 7, `throw` was a statement, with C# 7, `throw` is an expression.
* We can customize the exception object before throwing as follows:

    ```c#
    Exception ex = new Exception("Test Exception");
    // HelpLink
    ex.HelpLink = "http://www.test.com";
    // Data 
    ex.Data.Add("TimeStamp",$"The exception happened at {DateTime.Now}");
    ex.Data.Add("Cause", "A Test.");

    throw ex;
    ```



### Notes on Creating Custom Exceptions

* Custom Exceptions should be public.
* We may add custom constructors, and custom properties.
* We can also use the Constructor to pass the values to the base class.

    ```c#
    public class newException: Exception
    {
        // Custom fields and properties
        public DateTime ErrorTimeStamp { get; set; }

        public newException(){}

        // Custom Constructor
        public newException(string message, DateTime time)
          : base(message)   // Note Exception class has a constructor that takes only a string
        {
            ErrorTimeStamp = time;
        }
    }
    ```
* To `throw` the above exception we can:

    ```c#
    class c
    {
    ...
        public void troubleMaker()
        {
        ...
            if(Something is Wrong)
            {
                newException ex = new newException("Something Wrong", DateTime.Now);
                ex.HelpLink = "www.google.co.in";
                throw ex;
            }
        }
    }

    // Inside main
    c obj = new c();
    try
    {
        c.troubleMaker();
    }
    catch(newException ex)
    {
        Console.WriteLine(ex.Message);      // Pre-defined Property
        Console.WriteLine(ex.ErrorTimeStamp);   // User defined Property
    }
    ```
* Often the purpose of building a custom exception is to create a stringly named type so that the error can be identified properly.
* Best Practices for creatng exceptions in .NET are:
    1. Derive from `Exception`/`ApplicationException`
    2. Mark with `[System.Serializable]` attribute
    3. Define a default Constructor
    4. Define a constructor that sets the inherited `Message` property
    5. Define a constructor to handle *'inner excpeptions'*.
    6. Define a constructor to handle serialization



### General Catch statement

* C# also supports general catch statement.

    ```c#
    try
    {
        c.troubleMaker();
    }
    catch
    {
        Console.WriteLine("Something Happened");
    }
    ```



## Rethrowing Exception

* We can *'rethrow'* an exception by using the `throw` within the `catch` block.
* This may be done if we are handling the exception within the `catch` block partially.

    ```c#
    try
    {
        int x = 1/0;
    }
    catch(DivideByZeroException ex)
    {
        Console.WriteLine(ex.Message);
        throw;
    }

    /*
    Attempted to divide by zero.
    Unhandled exception. System.DivideByZeroException: Attempted to divide by zero.
    at CSharpTest.Program.Main() in /home/angshuman/Test/dotnet/CSharpTest/Program.cs:line 12
    */
    ```

* Although not explicitly mentioned we are 're-throwing' the `DivideByZeroException`. We also get all the original information along with the object.



## Inner Exceptions

* When we encounter an exception B *while* processing another exception A:
  
  * Record exception A as an *inner* exception of B.
* The following code is an example

    ```c#
    // Inside main function
    try
    {
        try
        {
            var num = int.Parse("abc"); // Throws FormatException 
        }
        catch(FormatException fe)
        {
            try
            {
                var openLog = File.Open("DoesNotExist", FileMode.Open);
            }
            catch(FileNotFoundException)
            {
                throw new FileNotFoundException("NestedExceptionMessage: File `DoesNotExist` not found.", fe);
            }
        }
    }
    catch(FormatException)
    {
        // FormatException isn't caught because it's stored "inside" the FileNotFoundException 
    }
    catch(FileNotFoundException fnfex)
    {
        string inMsg = "", outMsg;
        if(fnfex.InnerException != null)
            inMsg = fnfex.InnerException.Message; // Inner exception (FormatException) message

        outMsg = fnfex.Message;

        Console.WriteLine($"Inner Exception Message: {inMsg}");
        Console.WriteLine($"Outer Exception Message: {outMsg}");
    }

    /*
    Inner Exception Message: Input string was not in a correct format.
    Outer Exception Message: NestedExceptionMessage: File `DoesNotExist` not found.
    */
    ```

* The **Outer** exception refers to the **most deeply nested exception** that is **ultimately** thrown. The **Inner** exception refers the **most shallow** (in scope) exception.



## `finally` block

* `finally` block is optional.
* It ensures that a set of code statements will always execute.
* When we need to dispose of objects, close a file, or detach from a database, a `finally` block may be used.



## Exception filters

* Exception Filters (C# 6) ensure that the statements within a catch block are executed only if some condition holds true.
* This is done by adding a `when` clause in the catch scope.
* The expression in the `when` must evaluate to a Boolean value.

    ```c#
    // In main method
    int x = 1, y = 0, z;
    try
    {
        z = x / y;
    }
    catch(DivideByZeroException) when (x != 1)
    {
        Console.WriteLine("Test");
    }
    catch(Exception)
    {
        Console.WriteLine("Another Test");
    }

    //Another Test
    ```

    * The single Boolean statement in the when clause must be wrapped in parentheses.

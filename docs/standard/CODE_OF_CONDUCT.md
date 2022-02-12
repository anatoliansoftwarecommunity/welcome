# Code of Conduct

This document contains a set of rules outlining the community's norms, rules, and responsibilities or proper practices. All the members of the community must pay attention to this document while developing any kind of application for the community.

This document contains only `C#` rules because the community will start developing with that programming language.

## Casing

The naming standards are defined under this section that is used while developing an application [[1]].

- The name of the classes, enumerations, delegates, methods, constructors, namespaces, and property must be Pascal case.
- The name of the variables and method parameters must be Camel case.

```csharp
  namespace MyNamespace
  {
    public void delegate MyDelegate(int x, int y);

    public class MyClass
    {
      private int numberOfItem;

      public MyClass(int constructorParameter)
      {
      }

      public void MyFirstMethodOfMyClass()
      {
      }

      public void MySecondMethod(int numberOfItem, string nameOfTheCompany)
      {
      }
    }

    public enum MyEnum
    {
        None = 0,
        Value = 1
    }
  }
```

- For the test methods the same rules apply. However, underscore `_` can be used for test methods.
- All the test methods must end with the `Test` suffix.

## Naming Namespace

The name of the namespace must be singular.

## Naming Methods That Read Data

If the method reads data from databases or services (endpoints), the name of the method must be started with the `Retrieve` prefix. On the other hand, other methods that read data must start with `Get` prefix.

## Usage of `using`

The `using` keyword that is used for including libraries must be inside the namespace [[2]][[3]].

The unnecessary `using` keywords should be removed.

## Usage of `static`

The `static` keyword can be used only in the following situations.

- For the `Main` method that is the entry point of the application [[4]].
- For operator overloading [[5]].
- For extension methods [[6]].

## Usage of `this` and `base`

Every method and variable must be used with the `this` keyword if the development is in the same class. Even if the class is inherited from the base class, the `this` keyword must be used.

The `base` keyword must only be used if needed to call the base method and base variable. In addition, for calling non-default constructors, the `base` keyword must be used.

```csharp
public class BaseClass
{
  private int count;

  protected BaseClass(int parameter1)
  {
      this.count = parameter1;
  }

  public void MyBaseClassMethod()
  {
      this.count *= 2;

      this.MyMethod2();

      //...
  }

  public virtual void MyMethod2()
  {
    // Do something
    // ...
  }
}

public class DerivedClass: BaseClass
{
  public DerivedClass()
    : base(15)
  {
  }

  public void MyMethod()
  {
    this.MyBaseClassMethod();
  }

  public override MyMethod2()
  {
    base.MyMethod2();

    // Do some other thing
    // ...
  }
}
```

## Usage of `const`

While developing an application must be used `static readonly` rather than `const` [[7]].

## Usage of `sealed`

If the need is closing class for the inheritance, should use the `sealed` keyword [[8]]. Otherwise, it should not be used.

## Access Modifiers

The ordinal of the methods and variables must be in the following order in the class [[9]].

1. public
2. internal
3. protected internal
4. protected
5. private protected
6. private

## Abstract Classes

While defining an abstract class constructor, the access modifier of it must be `protected` [[10]].

There is no restriction of defining a default constructor for the abstract classes.

## Defining Interface

The name of the interfaces must start with the `I` prefix.

```csharp
public interface IMoveable
{
  // Correct
}

public interface MovableInterface
{
  // Wrong
}
```

An interface must contain at least one method, at most ten methods. Additionally, an interface must add a behavior to the class.

The empty interfaces cannot be used.

## Using `class` or `struct`

While developing an application, only `class` can be used. The `struct` keyword is used by the framework.

## Generic Types

While the naming of the generic types, must use the `T` prefix and must continue with the parameter name that expresses it.

```csharp
public abstract class BaseEntity<TEntity>
{
  // Correct
}

public abstract class BaseDictionary<TKey, TValue>
{
  // Correct
}

public abstract class BaseDictionary<K, V>
{
  // Wrong
}
```

## Control Statements

- The `?:`, `??`, `??=`, `?.`, and `?[]` control statements cannot be used [[11]] [[12]] [[13]].
- The `if`, `else`, and `else if` control blocks must start and end with curly brackets.
- The `for`, `foreach`, `switch`, `while`, and `do..while` loop blocks must start and end with curly brackets.

```csharp
// Wrong
if(...)
  DoWork();
else if(...)
  DoOtherWork();
else
  DoWorkWorkWork();

// Correct
if(...)
{
  DoWork();
}
else if(...)
{
  DoOtherWork();
}
else
{
  DoWorkWorkWork();
}
```

## Type Conversion and Checking

While converting a type, the `as` keyword must be used for reference type objects. After casting the result must check against null value [[14]] [[15]].

If the variable is a value typed object, converting `()` (cast expression) must be used.

## Usage of `var`

While developing an application, the `var` keyword cannot be used.

## Usage of `dynamic`

While developing, the `dynamic` keyword should be used, if there is not an alternative solution. Before the keyword must be added a comment line for an explanation of usage `dynamic` keyword reason.

## Usage of `readonly`

If the value of the variable will not be changed in the code while initializing the class, the variable should be marked as read-only using the `readonly` keyword.

## Inline Variable Defining

While developing an application, the inline variable definition is prohibited.

```csharp
// Wrong
UserCreateResponse response = this.service.Execute(new UserCreateRequest{ ... });

// Correct
UserCreateRequest request = new UserCreateRequest{...};
UserCreateResponse response = this.service.Execute(request);
```

## Semantics Grouping & Separation

The code blocks that are semantically paired must not be separated.

```csharp
// Wrong
var list = new List<User>();


foreach(var user in list)
{
  ...
}

// Correct
var list = new List<User>();
foreach(var user in list)
{
  ...
}
```

The code blocks that are semantically not paired must be a blank line between them. For explaining the block should be added a comment line at the beginning of it.

```csharp
// Wrong
public void CalculateInvoice(int restaurantId)
{
  // Definition
  var invoice = new Invoice();
  invoice.Lines = this.invoiceLineRepository.RetrieveByRestaurantId(restaurantId);
  invoice.Total = invoice.Lines.Sum(x=> x.Amount);
  decimal invoice.Discount = 0M;
  if(total > 100M)        {    invoice.Discount = total * 0.05M;  }  
  else if(total > 200M)   {    invoice.Discount = total * 0.1M;   }
  invoice.Tax = (invoice.Total - invoice.Discount) * 0.18M;
  this.invoiceRepository.Save(invoice);
}

// Correct
public void CalculateInvoice(int restaurantId)
{
  // Definition
  var invoice = new Invoice();

  // Retrieve lines
  invoice.Lines = this.invoiceLineRepository.RetrieveByRestaurantId(restaurantId);

  // Retrieve Total value.
  decimal total = invoice.Lines.Sum(x=> x.Amount);
  invoice.Total = total;
  
  // Retrieve Discount value.
  decimal discount = 0M;
  if(total > 100M)
  {
    discount = total * 0.05M;
  }
  else if(total > 200M)
  {
    discount = total * 0.1M;
  }
  invoice.Discount = discount;

  // Calculate Tax value.
  invoice.Tax = (total - discount) * 0.18M;

  // Save invoce.
  this.invoiceRepository.Save(invoice);
}
```

## Indentation

While developing code indentation must be used `TAB` [[16]].

## Logging

While developing an application the first step is creating usable and meaningful log records. This part is related to logging.

The following log message types can be used for logging [[17]] [[18]].

- Trace
- Debug
- Info (Information)
- Warn (Warning)
- Error
- Fatal

### Trace

Just can be used for tracing the code or specific features.

### Debug

Finding the problem can be used. At this level, the log messages must provide information that helps developers find solutions.

### Info (Information)

Usually, giving general information about applications such as the application started or ended, the application processed how many records, etc.

### Warn (Warning)

Used for exceptions that have occurred, but automatically will be recovered. For example; one message could not be processed properly. The next cycle it will be tried again.

### Error

If the exception is important for business, but not important for service, the error level must be used for logging. For example; cannot respond to any requests or cannot open a file for reading.

In this condition, log messages must be tracked and if it is needed, must take action.

While using the error level, the notification message should be sent to the developer team.

### Fatal

If the following conditions have occurred, the fatal level must be used in the following conditions.

- Any exception that causes data loss.
- Any exception that causes corruption.

For example; deleting a record without a way of recovery.

While using the fatal level, the notification message should be sent to the developer team, business owners, and all related parties.

## Saving Log Messages

The produced log message must be saved in the disk drive of the machine in which the application also works.

One application log message must be kept separately from other application log messages.

The logging library or code at least must provide the following interface.

```csharp
namespace <ProjectName>.Common
{
  public interface ITrace
  {
    void LogTrace(string message, params object[] parameters);

    void LogDebug(string message, params object[] parameters);

    void LogInfo(string message, params object[] parameters);

    void LogWar(string message, params object[] parameters);

    void LogError(string message, params object[] parameters);

    void LogFatal(string message, params object[] parameters);
  }
}
```

## Telemetry

While developing an application, tracing the application with the performance view may be required. In this case, telemetry can be used.

The telemetry library or code at least must provide the following interface.

```csharp
namespace <ProjectName>.Common
{
  public interface ITelemetry
  {
    void CreateEvent(string eventName);

    void CreateEvent(string eventName, Dictionary<string,string> properties);
    
    void CreateMetric(string metricName, double value);

    void CreateMetric(string metricName, double value, Dictionary<string,string> properties);

    void Measure(string methodName, Action callback);

    T Measure<T>(string methodName, Func<T> callback);
  }
}
```

## Naming Test Methods

Naming the test method must be followed this pattern below [[19]].

`Should_ExpectedBehavior_When_StateUnderTest`

## Test Method Structure

The test method must be separated into 3 parts. Assert, Act, and Arrange.

```csharp
public void Test_Method()
{
    //Arrange
    // setup test

    //Act
    // execute the test

    //Assert
    // assertions
}
```

## Testing Framework and Libraries

For developing test should be used a framework. The community suggests `XUnit` for testing framework, `Moq` for mocking framework.

## Naming of Mock Objects

Naming the mock object may be important while reading the test methods. So the correct naming must be selected for objects [[20]] [[21]].

## Naming Variable Inside The Test Method

The name of the variables that are declared in the test method must be meaningful that has same meaning with the test method name.

```csharp
public void Should_ThrowsException_When_PositiveAndNegative()
{
    //Arrange
    int positiveNumber = 1;
    int negativeNumber = -1;

    //Act
    sut.AddNumbers(positiveNumber, negativeNumber);

    // Assert

}

public void Should_ThrowsException_When_AddContact_With_InvalidEmail()
{
    // Arrange
    string anyCorrectPhoneNumber = "1234567891";
    string invalidEmail = "asc@example.cöm";
            
    // Act
    sut.AddContact(anyCorrectPhoneNumber, invalidEmail);

    // Assert

}
```

## Arranging Test Methods

While developing testing some arrangement code blocks may be duplicated. There is no problem for that, because each unit test must be isolated [[22]].

## References

1. [Most Common Programming Case Types][1]
2. [StyleCop Documentation - SA1200][2]
3. [Stackoverflow - Should 'using' directives be inside or outside the namespace?][3]
4. [Stackoverflow - Why is the class "Program" declared as static?][4]
5. [Operator overloading (C# reference)][5]
6. [Extension Methods (C# Programming Guide)][6]
7. [Const vs. Readonly vs. Static][7]
8. [C# Tweaks – Why to use the sealed keyword on classes?][8]
9. [Stackoverflow - Order of items in classes: Fields, Properties, Constructors, Methods][9]
10. [Stackoverflow - Why should constructors on abstract classes be protected, not public?][9]
11. [?: operator (C# reference)][11]
12. [?? and ??= operators (C# reference)][12]
13. [Null-conditional operators ?. and ?[]][13]
14. [Casting and type conversions (C# Programming Guide)][14]
15. [Type-testing operators and cast expression (C# reference)][15]
16. [Developers Who Use Spaces Make More Money Than Those Who Use Tabs][16]
17. [Stackoverflow - When to use the different log levels][17]
18. [Wikipedia - Syslog][18]
19. [7 Popular Unit Test Naming Conventions][19]
20. [Test Double][20]
21. [Mocks Aren't Stubs][21]
22. [Why Good Developers Write Bad Unit Tests][22]

[1]: https://www.chaseadams.io/posts/most-common-programming-case-types/
[2]: https://documentation.help/StyleCop/SA1200.html
[3]: https://stackoverflow.com/questions/125319/should-using-directives-be-inside-or-outside-the-namespace/151560#151560
[4]: https://stackoverflow.com/questions/19092394/why-is-the-class-program-declared-as-static/19093177#19093177
[5]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/operator-overloading
[6]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods
[7]: https://www.pluralsight.com/guides/const-vs-readonly-vs-static-csharp
[8]: https://gmamaladze.wordpress.com/2011/07/22/c-tweaks-why-to-use-the-sealed-keyword-on-classes-2/
[9]: https://stackoverflow.com/questions/150479/order-of-items-in-classes-fields-properties-constructors-methods/310967#310967
[10]: https://stackoverflow.com/questions/761854/why-should-constructors-on-abstract-classes-be-protected-not-public/761864#761864
[11]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-operator
[12]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator
[13]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-
[14]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions
[15]: https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/type-testing-and-cast
[16]: https://stackoverflow.blog/2017/06/15/developers-use-spaces-make-money-use-tabs/
[17]: https://stackoverflow.com/questions/2031163/when-to-use-the-different-log-levels/2031209#2031209
[18]: https://en.wikipedia.org/wiki/Syslog
[19]: https://dzone.com/articles/7-popular-unit-test-naming
[20]: https://martinfowler.com/bliki/TestDouble.html
[21]: https://martinfowler.com/articles/mocksArentStubs.html
[22]: https://mtlynch.io/good-developers-bad-tests/

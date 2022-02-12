# Code of Conduct

This document contains a set of rules outlining the norms, rules, and responsibilities or proper practices of the community. All the members of the community must pay attention to this document while developing any kind of application for the community.

This document contains only `C#` rules, because the community will start developing with that programming language.

## Readability

The purpose of this section is while reading the code blocks must be easy to track and fast to read.

### Casing

The naming standards are defined under this section that use while developing an application [[1]].

- The name of the classes, enumerations, delegates, methods, constructors, namespaces, and property must be Pascal case.
- The name of the variables, and method parameters muet be Camel case.

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

- For the test methods same rules apply. However, underscore `_` can use for in test methods.
- All the test methods must end with `Test` suffix.

### Naming Namespace

The name of the namespace must be singular.

### Naming Methods That Read Data

If the method reads data from databases or services (endpoints), the name of the method must be started with `Retrieve` prefix. On the other hand, other methods that read data must start with `Get` prefix.

### Usage of `using`

The `using` keyword that use for including libraries must be inside the namespace [[2]][[3]].

The unnecessary `using` keywords should remove.

### Usage of `static`

The `static` keyword can be used only following situations.

- For the `Main` method that is entry point of the application [[4]].
- For operator overloading [[5]].
- For extension methods [[6]].

### Usage of `this` and `base`

For every methods and variables must use with the `this` keyword if the development is in same class. Even if the class inherited from base class, must also use `this` keyword.

The `base` keyword must only use, if need to call base method and base variable.In addition, for calling non default constructors can be used `base` keyword.

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

### Usage of `const`

While developing an application must be used `static readonly` rather than `const` [[7]].

### Usage of `sealed`

If the need are closing class for inheritance, should use `sealed` keyword [[8]]. Otherwise, it should not be used.

### Access Modifiers

The ordinal of the methods and variables must be in the following order in the class [[9]].

1. public
2. internal
3. protected internal
4. protected
5. private protected
6. private

### Abstract Classes

While defining an abstract class constructor, the access modifier of it must be `protected` [[10]].

There is no restriction of defining default constructor for the abstract classes.

### Defining Interface

The name of the interfaces must start with `I` prefix.

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

An interface must contain at least one method, at most ten methods. Additionally, an interface must add a behaviour to the class.

The empty interfaces cannot be used.

### Using `class` or `struct`

While developing an application, only `class` can be used. The `struct` keyword is used by framework.

### Generic Types

While naming of the generic types, must use `T` prefix and must continue with parameter name that express it.

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

### Control Statements

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

### Type Conversion and Checking

While converting a type, the `as` keyword must use for reference type objects. After casting the result must check against null value [[14]] [[15]].

If the variable is a value typed object, for converting `()` (cast expression) must use.

### Usage of `var`

While developing an application, the `var` keyword cannot be used.

### Usage of `dynamic`

While developing, the `dynamic` keyword should be used, if there is not any alternative solution. Before the keyword must be added a comment line for explanation of usage `dynamic` keyword reason.

### Usage of `readonly`

If the value of the variable will not be changed in the code while initializing class, the variable should be marked as readonly with using `readonly` keyword.

### Inline Variable Defining

While developing an application, the inline variable definition is prohibited.

```csharp
// Wrong
UserCreateResponse response = this.service.Execute(new UserCreateRequest{ ... });

// Correct
UserCreateRequest request = new UserCreateRequest{...};
UserCreateResponse response = this.service.Execute(request);
```

### Semantics Grouping & Separation

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

### Indentation

While developing code indentation must use with `TAB` [[16]].

## Design

## Maintainability

## Traceability

The purpose of this section is make the application easy to trace and easy to find problematical code block.

### Logging

While developing an application the first step is creating usable and meaningful log records. This part is related to logging.

The following log message types can be used for logging [[17]] [[18]].

- Trace
- Debug
- Info (Information)
- Warn (Warning)
- Error
- Fatal

#### Trace

Just can be used for tracing the code or specific feature.

#### Debug

For finding the problem can be used. With this level log messages must provide information that help developer to find solution.

#### Info (Information)

Usually, use for giving general information about application such as application started or ended, application processed how many records, etc.

#### Warn (Warning)

Used for exceptions which have been occurred, but automatically will be recovered situations. For example; one message could not be processed properly. The next cycle it will be tried again.

#### Error

If the exception is important for business, but it is not important for service, error level must be used for logging. For example; cannot respond any requests or cannot open a file for reading.

In this condition log messages must be track and if it is needed, must take an action.

While using error level, the notification message should be sent to the developer team.

#### Fatal

If the following conditions are occured, fatal level must be used in the following conditions.

- Any exception that causes data loss.
- Any exception that causes corruption.

For example; deleting a record without way of recovery.

While using fatal level, the notification message should be sent to the developer team, business owners and all related parties.

### Saving Log Messages

The produced log message must save in the disk drive of the machine which application also works in.

One application log messages must keep separately from other application log messages.

The logging library or code at least must provide following interface.

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

### Telemetry

While developing an application, tracing the application with performance view may  be required. In this case, telemetry can be used.

The telemerty library or code at least must provide following interface.

```csharp
namespace Yemeksepeti.Anchovy.Trace
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

## Performance

## Security

## Right Business

## Testability

## Portability

## Cloud Ready

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
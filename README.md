# Built-in Delegates in C#: Part 1 - Predicate

## Introduction to Built-in Delegates

In C#, there are three main built-in delegate types that cover most common scenarios, eliminating the need to declare custom delegates in many cases. These are:

1. Predicate
2. Func
3. Action

This document focuses on the Predicate delegate, which is commonly used for filtering operations.

## Predicate Delegate

The Predicate delegate is used to reference a method that takes one input parameter and returns a boolean value. It's particularly useful for filtering operations.

### Definition

```csharp
public delegate bool Predicate<in T>(T obj);
```

### Key Points

- Takes one parameter of any type (generic)
- Always returns a boolean value
- Commonly used in filtering methods like `List<T>.FindAll()`

### Usage Example

Let's modify our previous `FindElements` method to use the built-in Predicate delegate instead of a custom delegate:

```csharp
public class ElementOperations
{
    public static List<T> FindElements<T>(List<T> elements, Predicate<T> condition)
    {
        List<T> result = new List<T>();
        if (elements != null && condition != null)
        {
            for (int i = 0; i < elements.Count; i++)
            {
                if (condition(elements[i]))
                    result.Add(elements[i]);
            }
        }
        return result;
    }
}
```

Now we can use this method with the Predicate delegate:

```csharp
List<int> numbers = Enumerable.Range(1, 10).ToList();

Predicate<int> isOdd = n => n % 2 == 1;
List<int> oddNumbers = ElementOperations.FindElements(numbers, isOdd);

Console.WriteLine("Odd numbers:");
foreach (int odd in oddNumbers)
    Console.Write($"{odd} ");  // Output: 1 3 5 7 9
Console.WriteLine();

Predicate<int> isEven = n => n % 2 == 0;
List<int> evenNumbers = ElementOperations.FindElements(numbers, isEven);

Console.WriteLine("Even numbers:");
foreach (int even in evenNumbers)
    Console.Write($"{even} ");  // Output: 2 4 6 8 10
Console.WriteLine();
```

### Using Predicate with Existing Methods

Many built-in C# methods use the Predicate delegate. For example, the `List<T>.FindAll()` method:

```csharp
List<string> names = new List<string> { "Ahmed", "Mahmoud", "Mai", "Omar", "Ali" };

Predicate<string> longerThanFour = name => name.Length > 4;
List<string> longNames = names.FindAll(longerThanFour);

Console.WriteLine("Names longer than 4 characters:");
foreach (string name in longNames)
    Console.WriteLine(name);  // Output: Ahmed Mahmoud
```

## Benefits of Using Predicate

1. **Standardization**: Using built-in delegates promotes consistent code across different projects and teams.
2. **Readability**: Other developers will immediately understand the purpose of the delegate when they see `Predicate<T>`.
3. **Compatibility**: Many built-in methods in C# use Predicate, making your code more compatible with the standard library.
4. **Simplicity**: No need to declare custom delegates for simple boolean-returning functions.

## Conclusion

The Predicate delegate is a powerful tool in C# for working with methods that take a single parameter and return a boolean value. It's especially useful for filtering operations and is widely used in both custom and built-in methods. By using Predicate, you can write more standardized and readable code without the need for custom delegate declarations.

In the next parts of this series, we'll explore the other built-in delegates: Func and Action.


# Built-in Delegates in C#: Part 2 - Func and Action

## Introduction

Following our discussion of the Predicate delegate, we'll now explore two other important built-in delegates in C#: Func and Action. These delegates provide flexible ways to work with methods that have various numbers of parameters and return types.

## Func Delegate

The Func delegate is used to reference methods that have a return value and can take up to 16 input parameters.

### Key Points

- Can have 0 to 16 input parameters
- Must have a return value
- The last type parameter always represents the return type

### Syntax

```csharp
public delegate TResult Func<out TResult>();
public delegate TResult Func<in T, out TResult>(T arg);
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
// ... up to Func<T1, T2, T3, ..., T16, TResult>
```

### Usage Example

Let's modify our previous comparison example to use Func:

```csharp
class CompareFunctions
{
    public static bool SortDescending(int x, int y) { return x > y; }
    public static bool SortDescending(string x, string y) { return string.Compare(x, y) > 0; }
}

class Program
{
    static void Main()
    {
        // Using Func for integer comparison
        Func<int, int, bool> intCompare = CompareFunctions.SortDescending;
        Console.WriteLine($"5 > 3: {intCompare(5, 3)}");  // Output: True

        // Using Func for string comparison
        Func<string, string, bool> stringCompare = CompareFunctions.SortDescending;
        Console.WriteLine($"'b' > 'a': {stringCompare("b", "a")}");  // Output: True

        // Using Func with lambda expression
        Func<int, bool> isEven = x => x % 2 == 0;
        Console.WriteLine($"Is 4 even? {isEven(4)}");  // Output: True
    }
}
```

## Action Delegate

The Action delegate is similar to Func, but it's used for methods that don't return a value (void methods).

### Key Points

- Can have 0 to 16 input parameters
- Always represents void methods (no return value)

### Syntax

```csharp
public delegate void Action();
public delegate void Action<in T>(T obj);
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
// ... up to Action<T1, T2, T3, ..., T16>
```

### Usage Example

```csharp
class Program
{
    static void Main()
    {
        // Action with no parameters
        Action sayHello = () => Console.WriteLine("Hello, World!");
        sayHello();  // Output: Hello, World!

        // Action with one parameter
        Action<string> greet = name => Console.WriteLine($"Hello, {name}!");
        greet("Alice");  // Output: Hello, Alice!

        // Action with multiple parameters
        Action<int, int> add = (x, y) => Console.WriteLine($"{x} + {y} = {x + y}");
        add(3, 5);  // Output: 3 + 5 = 8
    }
}
```

## Comparison of Built-in Delegates

Here's a quick comparison of the three built-in delegates we've covered:

1. **Predicate<T>**: Always takes one parameter and returns a boolean.
2. **Func<T1, T2, ..., TResult>**: Takes 0 to 16 parameters and always returns a value.
3. **Action<T1, T2, ...>**: Takes 0 to 16 parameters and never returns a value (void).

## Benefits of Using Built-in Delegates

1. **Standardization**: Using built-in delegates promotes consistent code across different projects and teams.
2. **Flexibility**: These delegates cover a wide range of method signatures, reducing the need for custom delegates.
3. **Compatibility**: Many C# methods and LINQ operations use these delegates, making your code more compatible with the standard library.
4. **Readability**: Other developers will immediately understand the purpose and signature of methods using these delegates.

## Conclusion

Func and Action delegates, along with Predicate, form a powerful trio of built-in delegates in C#. They provide a flexible and standardized way to work with methods of various signatures, reducing the need for custom delegate declarations and promoting more readable and maintainable code.

By understanding and utilizing these built-in delegates, you can write more efficient and standardized C# code, leveraging the full power of delegate-based programming without the need for custom delegate declarations in most scenarios.


# Built-in Delegates in C#: Practical Examples

This guide provides practical examples of using the three main built-in delegates in C#: Predicate, Func, and Action. We'll use a common `TestFunctions` class to demonstrate each delegate type.

## TestFunctions Class

First, let's define a class with methods that we'll use throughout our examples:

```csharp
public class TestFunctions
{
    public static bool Test(int number)
    {
        return number > 0;
    }

    public static string Cast(int number)
    {
        return number.ToString();
    }

    public static void Print(string name)
    {
        Console.WriteLine($"Hello {name}");
    }
}
```

## Predicate<T> Example

Predicate represents a method that takes one parameter and returns a boolean. It's often used for filtering or testing conditions.

```csharp
Predicate<int> predicate = TestFunctions.Test;

// Using Invoke method
bool result1 = predicate.Invoke(10);
Console.WriteLine($"Is 10 > 0? {result1}");  // Output: Is 10 > 0? True

// Using shorthand notation
bool result2 = predicate(5);
Console.WriteLine($"Is 5 > 0? {result2}");  // Output: Is 5 > 0? True

bool result3 = predicate(-3);
Console.WriteLine($"Is -3 > 0? {result3}");  // Output: Is -3 > 0? False
```

In this example, `Predicate<int>` represents a method that takes an integer and returns a boolean. It's used to test if a number is greater than zero.

## Func<T, TResult> Example

Func represents a method that takes between 0 and 16 input parameters and returns a value. The last type parameter always represents the return type.

```csharp
Func<int, string> func = TestFunctions.Cast;

// Using Invoke method
string result1 = func.Invoke(5);
Console.WriteLine($"String representation of 5: {result1}");  // Output: String representation of 5: 5

// Using shorthand notation
string result2 = func(10);
Console.WriteLine($"String representation of 10: {result2}");  // Output: String representation of 10: 10
```

In this example, `Func<int, string>` represents a method that takes an integer and returns a string. It's used to convert an integer to its string representation.

## Action<T> Example

Action represents a method that takes between 0 and 16 input parameters and doesn't return a value (void).

```csharp
Action<string> action = TestFunctions.Print;

// Using Invoke method
action.Invoke("Ahmed");  // Output: Hello Ahmed

// Using shorthand notation
action("Sarah");  // Output: Hello Sarah
```

In this example, `Action<string>` represents a method that takes a string parameter and doesn't return a value. It's used to print a greeting message.

## Comparing Delegate Invocations

For all three delegate types, you can use either the `Invoke()` method or the shorthand notation (treating the delegate like a method). The shorthand notation is more commonly used due to its conciseness.

## Using Delegates with Lambda Expressions

You can also use lambda expressions to create delegate instances inline:

```csharp
Predicate<int> isEven = x => x % 2 == 0;
Console.WriteLine($"Is 4 even? {isEven(4)}");  // Output: Is 4 even? True

Func<double, double> square = x => x * x;
Console.WriteLine($"Square of 3.5: {square(3.5)}");  // Output: Square of 3.5: 12.25

Action<int> printSquare = x => Console.WriteLine($"Square of {x} is {x * x}");
printSquare(7);  // Output: Square of 7 is 49
```

## Conclusion

These examples demonstrate how Predicate, Func, and Action delegates can be used in C#. Each serves a specific purpose:

- `Predicate<T>` is used for methods that test a condition and return a boolean.
- `Func<T, TResult>` is used for methods that take parameters and return a value.
- `Action<T>` is used for void methods that perform an action.

By using these built-in delegates, you can write more flexible and reusable code, often eliminating the need to define custom delegates for common scenarios.




# Delegates in C#: Anonymous Functions and Lambda Expressions

While C# is primarily an object-oriented language, it also supports functional programming concepts. Anonymous functions and lambda expressions are features that allow for more concise and flexible code, similar to JavaScript's approach to functions.

## Anonymous Functions (C# 2.0, 2005)

Anonymous functions allow you to create inline method definitions. This is useful when you want to create a simple method without formally defining it in a class.

### Syntax:

```csharp
delegate (parameters) { method-body }
```

### Examples with Built-in Delegates:

```csharp
// Predicate
Predicate<int> predicate = delegate (int number) { return number > 0; };

// Func
Func<int, string> func = delegate (int number) { return number.ToString(); };

// Action
Action<string> action = delegate (string name) { Console.WriteLine($"Hello {name}"); };

// Usage
Console.WriteLine(predicate(5));  // Output: True
Console.WriteLine(func(10));      // Output: "10"
action("Alice");                  // Output: Hello Alice
```

## Lambda Expressions (C# 3.0, 2007)

Lambda expressions provide an even more concise way to write anonymous functions. They're often referred to as "fat arrow" functions, similar to JavaScript's arrow functions.

### Syntax:

For a single expression:
```csharp
(parameters) => expression
```

For multiple statements:
```csharp
(parameters) => { statements }
```

### Examples with Built-in Delegates:

```csharp
// Predicate
Predicate<int> predicate = number => number > 0;

// Func
Func<int, string> func = number => number.ToString();

// Action
Action<string> action = name => Console.WriteLine($"Hello {name}");

// Usage
Console.WriteLine(predicate(5));  // Output: True
Console.WriteLine(func(10));      // Output: "10"
action("Bob");                    // Output: Hello Bob
```

## Advanced Lambda Examples

### Multiple Parameters

```csharp
Func<int, int, int> add = (a, b) => a + b;
Console.WriteLine(add(3, 5));  // Output: 8
```

### Type Inference

C# can often infer the types of the parameters in a lambda expression:

```csharp
var multiply = (int x, int y) => x * y;
Console.WriteLine(multiply(4, 5));  // Output: 20
```

### Expression Bodied Members (C# 6.0, 2015)

This syntax can be used for methods, properties, and other members in classes:

```csharp
public class Calculator
{
    public static int Add(int a, int b) => a + b;
    public int Multiply(int x, int y) => x * y;
}
```

## Benefits of Lambda Expressions

1. **Conciseness**: Lambdas allow you to write short, expressive code.
2. **Readability**: For simple operations, lambdas can be more readable than full method definitions.
3. **Flexibility**: Lambdas can be used inline, making code more compact and reducing the need for separate method definitions.
4. **LINQ Integration**: Lambdas are extensively used with LINQ for querying data.

## Conclusion

Anonymous functions and lambda expressions in C# provide a way to write more concise and flexible code, similar to functional programming paradigms seen in languages like JavaScript. These features are particularly useful when working with delegates and LINQ, allowing for more expressive and readable code without the need for formal method definitions in every case.

By using these features, C# developers can blend object-oriented and functional programming styles, choosing the most appropriate approach for each situation.

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

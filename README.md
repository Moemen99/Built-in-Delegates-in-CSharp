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

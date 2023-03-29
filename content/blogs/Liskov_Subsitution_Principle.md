---
title: "# The Liskov Substitution Principle in C#: The L of SOLID"
date: 2023-03-28T23:29:21+05:30
draft: false
# github_link: "https://github.com/Erik-Z/Project"
author: "Erik Zhou"
tags:
  - Liskov Subsitution
  - Design Principles
  - SOLID
  - Software Design
image: /images/Liskov_Subsitution_Principle.png
description: "The Liskov Substitution Principle (LSP) is one of the five principles of object-oriented programming and design known as SOLID. It was introduced by Barbara Liskov in 1987 and states that objects of a superclass should be able to be replaced by objects of a subclass without affecting the correctness of the program. In other words, child classes should be substitutable for their parent classes. In this blog post, we'll explore the LSP in depth and see how it can be applied in C#.
"
toc:
---

The Liskov Substitution Principle (LSP) is one of the five principles of object-oriented programming and design known as SOLID. It was introduced by Barbara Liskov in 1987 and says that objects of a superclass should be able to be replaced by objects of a subclass without affecting the correctness of the program. In other words, subclasses should be substitutable for their parent classes. In this blog post, we'll take a look at the LSP and see how it can be applied in C#.

## Understanding the LSP

The LSP promotes the use of polymorphism and enforces a strong relationship between the parent class and its child classes. It helps ensure that the child classes do not violate the behavior, attributes, or semantics of the parent class. By adhering to the LSP, we can create code that is more maintainable, reusable, and less prone to errors.

Here's a formal definition of the LSP:

> Let φ(x) be a property provable about objects x of type T. Then φ(y) should be true for objects y of type S, where S is a subtype of T.

In simpler terms, if a class S is a subclass of class T, an object of class T should be replaceable by an object of class S without altering the properties of the program.

## Applying the LSP in C#

Let's look at an example to see how the LSP can be applied in C#. Consider the following class hierarchy:

```csharp
public abstract class Bird
{
    public abstract void Fly();
}

public class Pigeon : Bird
{
    public override void Fly()
    {
        // Pigeon's implementation of flying
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        // Penguins can't fly!
    }
}
```

In this example, we have an abstract `Bird` class with a `Fly` method. The `Pigeon` and `Penguin` classes inherit from the `Bird` class and provide their own implementations of the `Fly` method. However, we have a problem: not all birds can fly, as demonstrated by the `Penguin` class. This means that the `Penguin` class is violating the LSP because it cannot be substituted for a `Bird` object without potentially causing issues in our program.

To fix this issue, we can refactor our class hierarchy to better adhere to the LSP:

```csharp
public abstract class Bird
{
    // Common properties and methods for all birds
}

public interface IFlyable
{
    void Fly();
}

public class Pigeon : Bird, IFlyable
{
    public void Fly()
    {
        // Pigeon's implementation of flying
    }
}

public class Penguin : Bird
{
    // Penguins don't need to implement the Fly method
}
```

In this refactored version, we have separated the `Fly` method into a separate `IFlyable` interface. Now, only birds that can fly will implement this interface. This ensures that our class hierarchy adheres to the LSP because `Penguin` objects can now be substituted for `Bird` objects without causing issues.

## Benefits of the LSP

By adhering to the LSP, we can create code that is more:

1. **Maintainable**: Changes to the parent class or its child classes will have fewer ripple effects throughout the codebase.
2. **Reusable**: child classes can be easily reused in different parts of the program without causing unexpected behavior.
3. **Robust**: The code is less prone to errors because the behavior and semantics of the parent class and its child classes are consistent.

## LSP Violations and How to Avoid Them

To help avoid LSP violations in your C# code, watch out for these common pitfalls:

1. **Preconditions**: Child classes should not strengthen the preconditions of a parent class method. This means that the requirements for calling a method in the child class should not be stricter than the parent class method.

```csharp
public class Rectangle
{
    public virtual void SetWidth(int width) { /* ... */ }
    public virtual void SetHeight(int height) { /* ... */ }
}

public class Square : Rectangle
{
    // This method violates the LSP because it strengthens the precondition
    // by requiring the width to be equal to the height
    public override void SetWidth(int width)
    {
        base.SetWidth(width);
        base.SetHeight(width);
    }

    // Similarly for SetHeight...
}
```

To fix this violation, we could try refactoring the class hierarchy or using [composition](https://erikzhou.com/blogs/composition_vs_inheritance/) over inheritance.

2. **Postconditions**: Child classes should not weaken the postconditions of a parent class method. This means that the guarantees made by a method in the parent class should also be true for the child class method.

```csharp
public class FileWriter
{
    public virtual void Save(string content) { /* ... */ }
    public virtual bool IsSaved { get; protected set; }
}

public class TimedFileWriter : FileWriter
{
    // This method violates the LSP because it weakens the postcondition
    // by not guaranteeing that the content will be saved immediately
    public override void Save(string content)
    {
        // Save the content after a delay
        Task.Delay(1000).ContinueWith(_ => base.Save(content));
    }
}
```
To fix this violation, we could refactoring the class hierarchy or adjusting the behavior to meet the postconditions of the parent class method.

3. **Invariants**: Child classes should not modify the invariants of the parent class. Invariants are properties or conditions that must always hold true for an object. Changing invariants in a child class can lead to unexpected behavior and LSP violations.

```csharp
public class Stack<T>
{
    protected List<T> items = new List<T>();
    public virtual void Push(T item) { /* ... */ }
    public virtual T Pop() { /* ... */ }
    public int Count => items.Count;
}

public class LimitedStack<T> : Stack<T>
{
    private int limit;

    public LimitedStack(int limit)
    {
        this.limit = limit;
    }

    // This method violates the LSP because it modifies the invariant
    // that the Count should always increase after a successful Push
    public override void Push(T item)
    {
        if (Count >= limit)
        {
            throw new InvalidOperationException("Stack is full");
        }

        base.Push(item);
    }
}
```

To fix this violation, we could refactor the class hierarchy or using composition over inheritance.

By being aware of these potential LSP violations and following the guidelines in this post, you can write C# code that follows the Liskov Substitution Principle.

## Conclusion

The Liskov Substitution Principle (LSP) is an essential concept in object-oriented programming and a core principle of SOLID. By following the LSP, you can ensure that your C# classes and their child classes are consistent and interchangeable, leading to more maintainable, reusable, and robust code.
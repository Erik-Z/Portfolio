---
title: "Inheritance vs Composition in C#: When to Use Which and Why"
date: 2023-03-23T19:27:00+05:30
draft: false
author: "Erik Zhou"
tags:
  - Inheritance
  - Composition
  - Software Design
  - Programming Patterns
  - Software Development
  - Best Practices
image: /images/composition_vs_inheritance.jpg
description: "Inheritance and composition are two fundamental techniques in object-oriented programming (OOP) for creating modular, reusable, and maintainable code. In this blog post, we’ll explore the differences between inheritance and composition, and discuss when to use each approach in C#. We’ll also provide code examples to illustrate the concepts."
toc: 
---

## Introduction

Inheritance and composition are two fundamental techniques in object-oriented programming (OOP) for creating modular, reusable, and maintainable code. In this blog post, we'll explore the differences between inheritance and composition, and discuss when to use each approach in C#. We'll also provide code examples to illustrate the concepts.

## Inheritance

Inheritance is an "is-a" relationship that allows one class (subclass) to inherit properties and methods from another class (parent class). This mechanism enables code reuse, simplifies code maintenance, and promotes consistency by allowing you to define shared behavior in a single place.

### Example: Inheritance in C#

```csharp
public abstract class Animal
{
    public string Name { get; set; }

    public abstract void MakeSound();
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine($"{Name} barks.");
    }
}

public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine($"{Name} meows.");
    }
}
``` 
## Potential Problems with Inheritance

Inheritance, while a powerful technique, can lead to several issues if not used carefully. Here are some potential problems with inheritance:

### 1. Fragile Base Class Problem

When changes are made to the base class, it can inadvertently affect all derived classes. This can lead to unexpected behavior and bugs in the derived classes, making the code fragile and harder to maintain.

### 2. Tight Coupling

Inheritance creates a tight coupling between the base class and its derived classes. This tight coupling can make it difficult to modify, extend, or reuse individual classes, as changes in one class may require changes in other related classes.

### 3. Inheritance Hierarchy Complexity

Deep inheritance hierarchies can make the code difficult to understand and maintain. Navigating through multiple levels of inheritance to find the relevant code or behavior can be challenging, and may lead to code duplication or logic spread across multiple classes.

### 4. Overuse of Inheritance for Code Reuse

Inheritance is sometimes overused for code reuse, even when it's not the most appropriate solution. This can result in inflexible designs, tight coupling, and increased complexity. Composition and interfaces are often more suitable alternatives for achieving code reuse and modularity without the drawbacks of inheritance.

Now what exactly is composition?


## Composition

Composition is a "has-a" relationship, in which one class (composite class) contains instances of other classes (component classes). This approach allows you to create complex objects by combining simpler, more focused classes.

### Example: Composition in C#

```csharp
public interface ISoundMaker
{
    void MakeSound();
}

public class Barking : ISoundMaker
{
    public void MakeSound()
    {
        Console.WriteLine("Barks");
    }
}

public class Meowing : ISoundMaker
{
    public void MakeSound()
    {
        Console.WriteLine("Meows");
    }
}

public abstract class Animal
{
    public string Name { get; set; }
    public ISoundMaker SoundMaker { get; set; }

    public void MakeSound()
    {
        SoundMaker.MakeSound();
    }
}

public class Dog : Animal
{
    public Dog()
    {
        SoundMaker = new Barking();
    }
}

public class Cat : Animal
{
    public Cat()
    {
        SoundMaker = new Meowing();
    }
}
```

## When to Use Inheritance vs Composition

In general, you should prefer composition over inheritance. Composition offers greater flexibility, easier maintenance, and better adherence to the Single Responsibility Principle. However, there are cases where inheritance is a more suitable choice, such as when you need to define shared behavior or override base class methods.

-   Use inheritance when:
    -   You have a clear "is-a" relationship between classes.
    -   You need to reuse or extend existing functionality.
    -   The base class provides a well-defined interface that the derived class can override or extend.
-   Use composition when:
    -   You have a "has-a" or "uses-a" relationship between classes.
    -   You want to create complex objects by combining simpler, focused components.
    -   You need to change behavior at runtime or easily swap components.

## Conclusion

Inheritance and composition are powerful techniques in object-oriented programming that allow you to create reusable and maintainable code. By understanding the differences between the two and knowing when to use each, you can design more robust and flexible software.

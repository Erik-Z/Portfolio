---
title: "The Open-Closed Principle in C#: The O of SOLID"
date: 2023-03-26T22:23:21+05:30
draft: false
# github_link: "https://github.com/gurusabarish/hugo-profile"
author: "Erik Zhou"
tags:
  - Open Close
  - Design Principles
  - SOLID
  - Software Design
image: /images/Open_Close_Principle.jpg
description: "The Open-Closed Principle (OCP) is one of the five principles of object-oriented programming and design known as SOLID. It states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. In this blog post, we will dive deep into the Open-Closed Principle and learn how to apply it in C#."
toc:
---

The Open-Closed Principle (OCP) is one of the five principles of object-oriented programming and design known as SOLID. It states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. In this blog post, we will take a look into the Open-Closed Principle and learn how to apply it in C#.

## Understanding the Open-Closed Principle

The main idea behind the Open-Closed Principle is that an entity should be able to extend its behavior without modifying its source code. In other words, a class should be designed in such a way that it can accommodate new requirements or changes without changing its existing code. This can be done by using inheritance, interfaces, or other mechanisms that enable polymorphism.

By following the OCP, we can create more robust, maintainable, and flexible software systems that are less prone to bugs and easier to extend.

## Applying the Open-Closed Principle in C#

Let's consider this example: Suppose we have a class called `Shape` and two subclasses, `Circle` and `Rectangle`. We also have a method called `CalculateArea()` that calculates the area of different shapes.

### Example without OCP

```csharp
public class Shape
{
    public int Type { get; set; }
}

public class Circle : Shape
{
    public double Radius { get; set; }
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
}

public class AreaCalculator
{
    public double CalculateArea(Shape shape)
    {
        if (shape.Type == 1) // Circle
        {
            Circle circle = (Circle)shape;
            return Math.PI * circle.Radius * circle.Radius;
        }
        else if (shape.Type == 2) // Rectangle
        {
            Rectangle rectangle = (Rectangle)shape;
            return rectangle.Width * rectangle.Height;
        }

        throw new NotSupportedException("Shape type not supported.");
    }
}
```

The above example violates the Open-Closed Principle because if we want to add a new shape (e.g., Triangle), we need to change the `AreaCalculator` class. We will need to add another if statement to accommodate the triangle class.

### Refactoring to follow OCP

To apply the OCP, we can introduce an interface called `IShape` with a method called `CalculateArea()`. Then, we can make each shape class implement this interface.

```csharp
public interface IShape
{
    double CalculateArea();
}

public class Circle : IShape
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}

public class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double CalculateArea()
    {
        return Width * Height;
    }
}

public class AreaCalculator
{
    public double CalculateArea(IShape shape)
    {
        return shape.CalculateArea();
    }
}
```
Now, if we need to add a new shape (e.g., Triangle), we can create a new class that implements the `IShape` interface without modifying the existing code. This design adheres to the Open-Closed Principle and makes our system more maintainable and extendable.

## Conclusion

The Open-Closed Principle is a crucial principle of object-oriented programming and design that helps create flexable software systems. By designing our C# classes to be open for extension but closed for modification, we can accommodate new requirements without changing the existing code.
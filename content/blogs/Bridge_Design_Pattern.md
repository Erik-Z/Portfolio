---
title: "The Bridge Design Pattern"
date: 2023-08-22T23:29:21+05:30
draft: false
author: "Erik Zhou"
tags:
  - Structural Design Pattern
  - Bridge
  - Software Development
  - Design Patterns
image: /images/Bridge_Design_Pattern.jpg
description: "Design patterns are solutions to common in software design problems. One design pattern that lets you separate closely related objects is the Bridge Design Pattern. In this post, we'll dive deep into this pattern and it's use-cases."
toc:
---

## What is the Bridge Design Pattern?

What the bridge pattern basically does is allow you to separate closely related objects into two categories: Abstraction and Implementation. This prevents complexity from increasing exponentially as you add more and more different classes into your application.

## Why Use the Bridge Pattern?

Imagine you're developing software for drawing shapes, and you need to account for different types of shapes and the ability to draw them on various platforms. Without a pattern like Bridge, you'd end up with a class for every combination, e.g., `CircleOnWindows`, `CircleOnLinux`, `SquareOnWindows`, and so forth. The Bridge Pattern prevents this kind of complexity from getting out of control.

## How to Implement the Bridge Pattern

The Bridge Pattern usually involves four components:

1. **Abstraction:** The interface that defines the high-level abstraction.
2. **Refined Abstraction:** Extends the abstraction to provide more specific implementations.
3. **Implementor:** The interface for the implementation classes.
4. **Concrete Implementor:** Implements the Implementor interface and provides a concrete implementation.

### Example: Drawing Shapes on Different Platforms

Let's take a look at how to use the Bridge pattern by creating a drawing tool.

```csharp
// Implementor
public interface IDrawingAPI
{
    void DrawCircle(float x, float y, float radius);
}

// Concrete Implementor: Drawing on Windows
public class WindowsDrawingAPI : IDrawingAPI
{
    public void DrawCircle(float x, float y, float radius)
    {
        Console.WriteLine($"Windows - Drawing circle at {x},{y} with radius {radius}");
    }
}

// Concrete Implementor: Drawing on MacOS
public class MacOSDrawingAPI : IDrawingAPI
{
    public void DrawCircle(float x, float y, float radius)
    {
        Console.WriteLine($"MacOS - Drawing circle at {x},{y} with radius {radius}");
    }
}

// Abstraction
public abstract class Shape
{
    protected IDrawingAPI drawingAPI;

    protected Shape(IDrawingAPI api)
    {
        this.drawingAPI = api;
    }

    public abstract void Draw();
}

// Refined Abstraction
public class Circle : Shape
{
    private float x, y, radius;

    public Circle(float x, float y, float radius, IDrawingAPI api) : base(api)
    {
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    public override void Draw()
    {
        drawingAPI.DrawCircle(x, y, radius);
    }
}

// Client
public class Program
{
    public static void Main()
    {
        Shape circle1 = new Circle(1, 2, 3, new WindowsDrawingAPI());
        circle1.Draw();

        Shape circle2 = new Circle(1, 2, 3, new MacOSDrawingAPI());
        circle2.Draw();
    }
}
```

We created an interface `IDrawingAPI` (Implementor) that contains a method to draw a circle. We then provided concrete implementations for Windows and MacOS platforms. The `Shape` class (Abstraction) should contain a `IDrawingAPI`. Finally, we refined the abstraction using the `Circle` class. Now, the `Circle` class can utilize any drawing API to draw itself.

By doing this we avoided having to create two new classes for Windows and MacOS everytime we add a new shape. If we add Linux or some other operating system, we would have to create 4 new classes for every additional shape we add!

I hope this was a helpful overview of the Bridge design pattern! Happy coding!
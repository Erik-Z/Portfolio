---
title: "Boxing and Unboxing in C#"
date: 2023-04-13T23:29:21+05:30
draft: false
author: "Erik Zhou"
tags:
    - Boxing
    - Unboxing
    - Value Types
    - Reference Types
    - Performance
    - Generics
    - .NET
    - Programming
image: /images/boxing_unboxing_csharp.png
description: "Boxing and unboxing are important concepts in C# that deal with converting value types into reference types and vice versa. In this blog post, we'll take a look into the use cases of boxing and unboxing, and explore why they're useful."
toc:
---
Boxing and unboxing are important concepts in C# that deal with converting value types into reference types and vice versa. In this blog post, we'll take a look into the use cases of boxing and unboxing, and explore why they're useful. Keep in mind that excessive use of boxing and unboxing can lead to performance issues, so it's essential to understand when to use them appropriately.

## What are Boxing and Unboxing?

### Boxing
Boxing is the process of converting a value type (e.g., int, float, bool, etc.) into a reference type (object). This involves creating a new object on the heap and copying the value of the value type into that object. Here's an example:

```csharp
int num = 42;
object boxedNum = num; // Boxing
```

## Unboxing
Unboxing is the reverse process of boxing, where a reference type (object) is converted back into a value type. It involves extracting the value from the object and assigning it to the value type. Here's an example:

```csharp
object obj = 42;
int unboxedNum = (int)obj; // Unboxing
```

## Use Cases and Why It's Useful
Boxing and unboxing can be useful in certain scenarios, such as:

1. <b>Storing value types in collections:</b> Before the generics were available in C#, boxing, and unboxing were used to store value types in non-generic collections like ArrayList. This lets the collection store different value types as objects.

```csharp
ArrayList list = new ArrayList();
list.Add(42); // Implicit boxing
int retrievedNum = (int)list[0]; // Explicit unboxing
```
2. <b>Using value types with interfaces:</b> If a value type implements an interface, you can box the value type to interact with methods that require the interface. This enables polymorphism with value types.

```csharp
public interface IExample
{
    void DoSomething();
}

public struct MyStruct : IExample
{
    public void DoSomething()
    {
        // Do Something
    }
}

MyStruct myStruct = new MyStruct();
IExample boxedStruct = myStruct; // Boxing
```
3. <b>Passing value types as objects:</b> Sometimes, you need to pass a value type as an object parameter to a method. In this case, boxing is used to convert the value type to an object.

```csharp
public void ProcessObject(object obj)
{
    // Implementation
}

int value = 42;
ProcessObject(value); // Implicit boxing
```
Although boxing and unboxing can be useful, they can also impact performance when used excessively. It's important to be careful when using them and to prefer using generics when possible to avoid unnecessary boxing and unboxing.
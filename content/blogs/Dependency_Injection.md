---
title: "Dependency Injection in C#: The D of SOLID"
date: 2023-04-02T22:53:00+05:30
draft: false
author: "Erik Zhou"
tags:
  - Dependency Injection
  - SOLID Principles
  - Design Patterns
  - Software Design
image: /images/Dependency_Injection.jpg
description: "Dependency Injection (DI) is one of the five principles of SOLID. In this blog post, we will explore the concept of Dependency Injection, focusing on its applications within C# programming. We'll cover the basics of DI, dive into its advantages, and provide examples to demonstrate how it can be implemented in real-world scenarios."
toc: 
---

Dependency Injection (DI) is one of the five principles of SOLID. In this blog post, we will explore the concept of Dependency Injection, focusing on its applications within C# programming. We'll cover the basics of DI, dive into its advantages, and provide examples to demonstrate how it can be implemented in real-world scenarios.

## Introduction to Dependency Injection

Dependency Injection is a design pattern that promotes the Inversion of Control (IoC) principle. It tries to decouple objects and their dependencies, making the code more flexible and maintainable. DI is about injecting dependencies into objects, rather than creating them within the object itself.

Imagine a class that relies on an external service, such as a data repository. Traditionally, the class would create an instance of the repository, making it dependent on the specific implementation. With Dependency Injection, the repository instance is created outside the class and injected into it, allowing for more flexibility and easier testing.
## Benefits of Dependency Injection

1. **Loose coupling**: DI promotes the separation of concerns and reduces tight coupling between classes. This makes it easier to swap out dependencies or update their implementations without affecting the dependent classes.
Easier testing: DI allows you to easily substitute dependencies with mock or stub implementations for unit testing, making it simple to isolate and test specific components.
2. **Improved maintainability**: By decoupling dependencies from classes, the code becomes easier to maintain, as changes to one component have a minimal impact on the rest of the system.
More readable code: DI results in more self-documenting code, as the dependencies and their relations, are explicitly declared in constructors, properties, or methods.

## Dependency Injection Patterns

There are three main patterns for injecting dependencies: constructor injection, property injection, and method injection.

### Constructor Injection

Constructor injection is the most common form of Dependency Injection. It involves passing dependencies through the constructor of a class. This makes sure that the class has all the required dependencies before it is instantiated.

```csharp
public interface IRepository
{
    // Repository methods
}

public class UserRepository : IRepository
{
    // UserRepository implementation
}

public class UserService
{
    private readonly IRepository _repository;

    public UserService(IRepository repository)
    {
        _repository = repository;
    }

    // UserService methods
}
```

In the example above, the `UserService` class depends on the `IRepository` interface. The dependency is provided through the constructor, and the class is now decoupled from the specific implementation of the repository.

### Property Injection

Property injection is another way to inject dependencies. Instead of providing dependencies through the constructor, they are set through public properties.

```csharp
public class UserService
{
    public IRepository Repository { get; set; }

    // UserService methods
}
```
While this approach can be useful in certain scenarios, it can also lead to incomplete or inconsistent objects, as the dependency can be changed after the object is instantiated.

### Method Injection

Method injection involves providing dependencies as parameters to the methods that require them. This pattern is less common and is generally used when dependencies are needed only for specific methods, rather than the entire class.

```csharp
public class UserService
{
    public void PerformOperation(IRepository repository)
    {
        // Use the repository to perform an operation
    }
}
```
In this example, the `PerformOperation` method requires an `IRepository` implementation. The dependency is injected directly into the method as a parameter, rather than being stored as a class member.

### Using a DI Container

A DI container is responsible for managing dependencies and their lifetimes. It automates the process of creating and resolving dependencies, making it easier to apply DI throughout your application. There are several DI containers available for C#, including:

- [Autofac](https://autofac.org/)
- [Unity](https://github.com/unitycontainer/unity)
- [Ninject](http://www.ninject.org/)
- [Castle Windsor](https://github.com/castleproject/Windsor)
- [Microsoft.Extensions.DependencyInjection](https://github.com/dotnet/runtime/tree/main/src/libraries/Microsoft.Extensions.DependencyInjection)

In this example, we'll use Microsoft.Extensions.DependencyInjection, which is built into ASP.NET Core and can also be used in standalone applications.

First, add the `Microsoft.Extensions.DependencyInjection` NuGet package to your project.

Next, set up the DI container by creating a `ServiceCollection` and registering your dependencies:

```csharp
using Microsoft.Extensions.DependencyInjection;

class Program
{
    static void Main(string[] args)
    {
        var services = new ServiceCollection();

        services.AddScoped<IRepository, UserRepository>();
        services.AddScoped<UserService>();

        var serviceProvider = services.BuildServiceProvider();

        var userService = serviceProvider.GetService<UserService>();

        // Use userService to perform operations
    }
}

```

In this example, we register the `IRepository` interface with the `UserRepository` implementation and the `UserService` class. The `AddScoped` method indicates that these services should have a scoped lifetime, meaning a new instance will be created for each scope. Other lifetime options include AddSingleton and AddTransient.

Finally, we build the `ServiceProvider` and use it to resolve an instance of the UserService class, which has the IRepository dependency automatically injected.

## Conclusion

Dependency Injection is a powerful design pattern that improves code maintainability and flexibility. By injecting dependencies through constructors, properties, or methods, you can create loosely coupled and modular applications.
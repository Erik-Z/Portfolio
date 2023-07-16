---
title: "The Singleton Design Pattern"
date: 2023-07-14T22:23:21+05:30
draft: false
author: "Erik Zhou"
tags:
  - Singleton Pattern
  - Design Principles
  - Programming
  - Software Design
  - Code Optimization
image: /images/Singleton_Design_Pattern.jpg
description: "Hey! In this blog post, we'll explore the Singleton pattern with a special emphasis on its implementation in C# for managing database connections."
toc:
---

Hey! In this blog post, we'll explore the Singleton pattern with a special emphasis on its implementation in C# for managing database connections.

## What's the Singleton Design Principle?

The Singleton pattern is a creational design pattern and restricts the instantiation of a class to a single object. This means throughout your application, only a single instance of a specific class exists, providing a global point of access to it.

This pattern is very useful when a only single object is needed to control actions. It is commonly used in logging, driver objects, caching, thread pool, database connections, etc.

## Singleton in C# with Database Connection

Let's use an application that needs to connect to a database as an example. You wouldn't want to open a new connection every time you interact with the database. It's more efficient to use a single, shared connection. Singleton is perfect for this.

Here's how you might implement a Singleton Database Connection Manager in C#:

```csharp
public sealed class DatabaseConnectionManager
{
    private static DatabaseConnectionManager instance = null;
    private static readonly object padlock = new object();

    private SqlConnection connection = null;

    DatabaseConnectionManager()
    {
        connection = new SqlConnection(ConfigurationManager.AppSettings["DbConnectionString"]);
        connection.Open();
    }

    public static DatabaseConnectionManager Instance
    {
        get
        {
            lock (padlock)
            {
                if (instance == null)
                {
                    instance = new DatabaseConnectionManager();
                }
                return instance;
            }
        }
    }

    public SqlConnection Connection
    {
        get
        {
            return connection;
        }
    }
}
```
In this code:

- The `DatabaseConnectionManager` class is `sealed` to prevent inheritance.
- The `instance` variable holds the singleton instance.
- The `padlock` object is used for thread safety when checking and creating the instance.
- The private constructor `DatabaseConnectionManager()` opens the database connection.
- The `Instance` property returns the singleton instance.
- The `Connection` property provides access to the shared `SqlConnection` object.

## When Should You Use Singleton?

The Singleton pattern is a good choice when you want to limit the instantiation of a class to a single object. This can be used when:

- Managing access to a shared resource, like a database connection.
- You want to limit concurrent access to certain parts of your application.
- When a single instance should be used for all requests.

## Be Aware of the Disadvantages

The Singleton pattern does have its drawbacks. Here are some key things to be aware of:

1. **Global State**: The Singleton pattern can create issues because it introduces a global state into an application, which can result in unexpected dependencies between components and make code harder to debug.
2. **Testability**: Because Singletons introduce global state into applications, they can make unit tests harder to write and understand. (The book, "Zero to Production in Rust" introduces a neat way around this. By creating a new database for each test to ensure independence and isolation. In an enviornment where performance isn't needed, like unit testing, this is a very nice solution).
3. **Concurrency**: If not implemented properly, Singletons can cause problems with multi-threaded environments.

Now you have another design pattern to use in your toolbox! Happy coding!

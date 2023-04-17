---
title: "Logging in C#"
date: 2023-04-15T16:53:33+05:30
draft: false
author: "Erik Zhou"
tags:
  - .NET
  - Error detection
  - Logging
  - Software development
  - Debugging
image: /images/logging_csharp.jpg
description: "Logging is an important part of any software application. It helps developers monitor, diagnose, and debug issues that may arise during application execution. In this blog post, we'll take a look into logging in C#, talking about best practices and providing examples to help you implement logging effectively."
toc:
---

Logging is an important part of any software application. It helps developers monitor, diagnose, and debug issues that may come up during application execution. In this blog post, we'll take a look into logging in C#, talking about best practices and providing examples to help you implement logging effectively.

## Introduction to Logging in C#

In C#, logging can be implemented in a lot of ways. With the creation of .NET Core, Microsoft introduced a built-in logging framework that provides an easy way to add logging to your applications.

## Built-in Logging with .NET Core

.NET Core has a built-in logging framework that supports a lot of logging providers like Console, Debug, EventSource, and EventLog. Here's how you can get started with logging in a .NET Core application:

### 2.1 Adding Logging to a .NET Core Console Application

1. Create a new console application:


   ```
   dotnet new console -n LoggingExample
   cd LoggingExample
   ```

2. Add the required NuGet packages:


   ```
   dotnet add package Microsoft.Extensions.Logging
   dotnet add package Microsoft.Extensions.Logging.Console
   ```

3. Update the `Program.cs` file:


   ```csharp
   using System;
   using Microsoft.Extensions.Logging;

   namespace LoggingExample
   {
       class Program
       {
           static void Main(string[] args)
           {
               using ILoggerFactory loggerFactory = LoggerFactory.Create(builder =>
               {
                   builder
                       .AddFilter("Microsoft", LogLevel.Warning)
                       .AddFilter("System", LogLevel.Warning)
                       .AddFilter("LoggingExample", LogLevel.Debug)
                       .AddConsole();
               });

               ILogger logger = loggerFactory.CreateLogger<Program>();
               logger.LogInformation("This is an information log.");
               logger.LogWarning("This is a warning log.");
               logger.LogError("This is an error log.");
           }
       }
   }
   ```

4. Run the application:


   ```
   dotnet run
   ```

### 2.2 Logging in ASP.NET Core Applications

ASP.NET Core applications have built-in support for logging. You can [inject](https://erikzhou.com/blogs/dependency_injection/) the `ILogger<T>` interface into your classes to access the logging functionality. Here's an example of using logging in an ASP.NET Core MVC controller:

1. Create a new ASP.NET Core MVC application:


    ```
    dotnet new mvc -n MvcLoggingExample
    cd MvcLoggingExample
    ```

2. Update the HomeController.cs file:


    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Logging;

    namespace MvcLoggingExample.Controllers
    {
        public class HomeController : Controller
        {
            private readonly ILogger<HomeController> _logger;

            public HomeController(ILogger<HomeController> logger)
            {
                _logger = logger;
            }

            public IActionResult Index()
            {
                _logger.LogInformation("Index action executed");
                return View();
            }

            // Other actions
        }
    }
   ```

## 3. Logging Best Practices

To get the most out of logging in your C# applications, follow these best practices:

1. **Use the right log levels**: Log levels help you categorize and filter log messages. Use appropriate log levels to show the severity of the message (e.g., Debug, Information, Warning, Error, Critical).

2. **Avoid logging sensitive information**: Be careful about logging sensitive data, such as user credentials, personal information, or financial data. This could lead to security risks.

3. **Structure your log messages**: Structured logging allows you to include additional metadata in your log messages, making them easier to analyze and query.

4. **Include context in log messages**: Contextual information, such as the user, the action being performed, or the source of the error, can help you find issues more effectively.

## 5. Conclusion

By understanding and implementing effective logging practices, you can improve the reliability of your applications by being able to find and fix bugs more effectively. Remember to choose the right tools and techniques that best suit your application's needs and requirements.


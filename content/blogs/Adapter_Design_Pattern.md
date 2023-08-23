---
title: "The Adapter Design Pattern"
date: 2023-08-10T23:29:21+05:30
draft: false
author: "Erik Zhou"
tags:
  - Structural Design Pattern
  - Adapter
  - Software Development
  - Design Patterns
image: /images/Adapter_Design_Pattern.jpg
description: "Design patterns are solutions to common in software design problems. One design pattern that comes in handy when combining components or libraries that were not designed to work together is the Adapter Design Pattern. In this post, we'll dive deep into this pattern and it's use-cases."
toc:
---

## What is the Adapter Design Pattern?

Imagine you are traveling to a different country, and the plugs for your devices don't fit the local power sockets. What do you do? You take out your trusty European to North American power outlet adapter. This is the essence of the Adapter Design Pattern. To bridge the gap between two incompatable components.

In software development, the Adapter Pattern acts as a bridge between two incompatible components or libraries. This pattern involves a single class, the Adapter, which links the functionalities of independent or incompatible interfaces.

## Types of Adapters

There are two primary types of the Adapter Pattern:

1. **Class Adapter:** Uses inheritance to adapt one interface to another.
2. **Object Adapter:** Uses composition to combine the functionalities of the two interfaces.

### Class Adapter

In a class adapter, the adapter inherits from both objects. This means the adapter has access to both methods and can just override the methods. We will use a classic example of a XML to JSON adapter.

```csharp
using System;
using System.Xml.Linq;
using Newtonsoft.Json;

public class XMLDataProvider
{
    public string GetXMLData()
    {
        return @"<note>
                     <title>Bears</title>
                     <description>Bears are cool!</description>
                 </note>";
    }
}

public interface IJSONDataProvider
{
    string GetJSONData();
}

public class XMLToJSONAdapter : XMLDataProvider, IJSONDataProvider
{
    public string GetJSONData()
    {
        var document = XDocument.Parse(GetXMLData());
        return JsonConvert.SerializeXNode(document);
    }
}

public class Program
{
    static void Main()
    {
        IJSONDataProvider dataProvider = new XMLToJSONAdapter();
        Console.WriteLine(dataProvider.GetJSONData());
    }
}
```

### Object Adapter

Instead of inheriting, we use composition and wrap the service object.

```csharp
using System;
using System.Xml.Linq;
using Newtonsoft.Json;

public class XMLDataProvider
{
    public string GetXMLData()
    {
        return @"<note>
                     <title>Bears</title>
                     <description>Bears are cool!</description>
                 </note>";
    }
}

public interface IJSONDataProvider
{
    string GetJSONData();
}

public class XMLToJSONAdapter : IJSONDataProvider
{
    private XMLDataProvider _xmlDataProvider;

    public XMLToJSONAdapter(XMLDataProvider xmlDataProvider)
    {
        _xmlDataProvider = xmlDataProvider;
    }

    public string GetJSONData()
    {
        var document = XDocument.Parse(_xmlDataProvider.GetXMLData());
        return JsonConvert.SerializeXNode(document);
    }
}

public class Program
{
    static void Main()
    {
        XMLDataProvider xmlProvider = new XMLDataProvider();
        XMLToJSONAdapter dataProvider = new XMLToJSONAdapter(xmlProvider);
        Console.WriteLine(dataProvider.GetJSONData());
    }
}

```

I hope this was a helpful look at what adapters can do and how to use them! Happy Coding!
---
title: "Fast Inverse Square Root: A Look into Quake's Ingenious Algorithm"
date: 2022-24-03T19:53:33+05:30
draft: false
author: "Erik Zhou"
tags:
  - Quake
  - Inverse Square
  - Optimization
image: /images/quake_sqrt.jpg
description: "In this blog post, we will take a look into one of the coolest optimizations in the history of video game development: the fast inverse square root algorithm used in the Quake video game."
toc: 
---

In this blog post, we will take a look into one of the coolest optimizations in the history of video game development: the fast inverse square root algorithm used in the Quake video game. 

## Introduction

The fast inverse square root algorithm was first introduced in 1996 as part of the Quake video game engine, developed by id Software. It is an extremely efficient method for computing an approximation of the inverse square root of a 32-bit floating-point number, specifically designed for the limited processing power of the computers available at the time.

The algorithm is cool not only for its speed and accuracy but also for its combination of mathematical insight, clever bit manipulation, and a interesting constant that took advantage of the IEEE 754 floating-point representation used by computers.

## The Importance of Inverse Square Root

In computer graphics and 3D game development, the inverse square root is a fundamental operation used in various calculations, such as normalizing vectors, calculating lighting, and performing perspective projections. The performance of these calculations directly impacts the rendering speed and responsiveness of the game.

In the mid-1990s, when Quake was being developed, computers were not as powerful as they are today, and the cost of performing the inverse square root using traditional methods was extremely high. This made it essential for the developers to find a faster, more efficient method to perform this operation.

## The Traditional Approach: Newton-Raphson Iteration

One common method for computing the inverse square root of a number is to use the Newton-Raphson iterative method. The Newton-Raphson method is an iterative algorithm for finding the roots of a real-valued function. For the inverse square root, the function is defined as:

```scss
f(x) = 1 / x^2 - a
```

Where `a` is the input number for which we want to find the inverse square root. The Newton-Raphson iteration formula can be derived as follows:

```scss
x_{n+1} = x_n - f(x_n) / f'(x_n)
```

Applying this formula to the inverse square root function, we get:

```scss
x_{n+1} = x_n * (1.5 - 0.5 * a * x_n^2)
```

While the Newton-Raphson method converges quickly, it still requires many iterations and multiplications, which can be expensive in terms of processing power.

## Quake's Fast Inverse Square Root

Quake's fast inverse square root algorithm reduces the computational complexity of the inverse square root calculation by combining bit manipulation and mathematical insight. The algorithm can be broken down into three main steps:

- Bit manipulation and the initial guess
- A magic constant
- Refining the guess: one iteration of Newton-Raphson

## Bit Manipulation and the Initial Guess

The first step of the algorithm involves some bit manipulation to generate an initial guess for the inverse square root. The input floating-point number is treated as an integer, and the following operations are performed:

1. The exponent of the floating-point number is shifted right by one, effectively dividing it by two.
2. The magic constant is subtracted from the result.

The resulting integer is then treated as a floating-point number again, which serves as the initial approximation for the inverse square root.

Here's the C code for this part of the algorithm:

```c
float FastInvSqrt(float number) {
    long i;
    float x2, y;
    const float threehalfs = 1.5F;

    x2 = number * 0.5F;
    y = number;
    i = * (long *) &y; // Treat the float as an integer
    i = 0x5F3759DF - (i >> 1); // Shift exponent and subtract magic constant
    y = * (float *) &i; // Treat the integer as a float
    y = y * (threehalfs - (x2 * y * y)); // One iteration of Newton-Raphson

    return y;
}
```

### A Magical Constant

At the heart of the fast inverse square root algorithm lies a constant, which is often referred to as the "magic" constant:
```
0x5F3759DF
```
This constant is used in the algorithm to generate an initial approximation of the inverse square root. The choice of this constant is not arbitrary. It is a result of lots of testing, taking advantage of the properties of the IEEE 754 floating-point representation.

The exact role 0x5F3759DF plays in the algorithm is outside the scope of this post. But the constant 0x5F3759DF is subtracted from the result of the right shift. This constant serves as a correction factor to compensate for the approximation error that occurs when simply halving the exponent. This step generates the initial guess for the inverse square root. 

### Refining the Guess: One Iteration of Newton-Raphson

After obtaining the initial approximation, the algorithm performs one iteration of the Newton-Raphson method to refine the result. This single iteration significantly improves the accuracy of the approximation:

```c
y = y * (threehalfs - (x2 * y * y));
```

## Why Was It Genius?

The fast inverse square root algorithm was groundbreaking for several reasons:

### Speed and Efficiency

The algorithm dramatically reduced the computational cost of calculating the inverse square root compared to traditional methods. It relied on simple bit manipulation and a single iteration of the Newton-Raphson method, making it much faster than other techniques available at the time.

## Influence and Legacy

The impact of the fast inverse square root algorithm extended far beyond the Quake engine. It has been used in various other game engines, computer graphics libraries, and numerical computing applications. The algorithm also inspired further research and development of fast approximation techniques for other mathematical functions.

With modern hardware, dedicated hardware units and specialized instructions are available for fast computation of inverse square roots and other mathematical functions, making the algorithm less relevant today. However, it remains an excellent example of creative problem solving, optimization, and the importance of understanding the underlying hardware when developing high performant code.

### Conclusion

Quake's fast inverse square root algorithm is a testament to the ingenuity and creativity of the game's developers. It demonstrated the power of combining deep mathematical understanding with clever programming techniques and hardware-specific optimizations to achieve huge performance gains.
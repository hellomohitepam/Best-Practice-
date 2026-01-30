# Design Patterns

Design patterns are typical solutions to commonly occurring problems in software design.

## Overview

- There are officially **23 design patterns**.
- A design pattern is **not a specific piece of code**, but a **general concept** for solving a particular problem.
- Unlike an algorithm, which defines a clear set of steps to achieve a goal, a pattern provides a **high-level description or blueprint** for a solution.
- The core idea behind patterns is that **some classes change frequently while others remain stable**, so we separate them to manage change effectively.
- Pattern concepts are now used **beyond object-oriented programming** in many areas of software development.
- Design patterns give developers a **shared vocabulary**, making communication easier.
- Patterns can sometimes turn into **rigid rules (dogma)** instead of flexible guidelines.
- Some patterns solve **small, simple problems**, while others help shape **entire system architectures**.

## Categories of Design Patterns

There are three main categories of design patterns:

### 1. Creational Patterns
- Focus on **how objects are created**
- Aim to make code more **flexible and reusable**

### 2. Structural Patterns
- Show how to **combine classes and objects** into larger structures efficiently

### 3. Behavioral Patterns
- Focus on **how objects communicate**
- Deal with **sharing responsibilities** between objects






Strategy design pattern :- Defines a family of algorithms, put them into separate classes so that they can be changed at run time.

<img width="1013" height="646" alt="image" src="https://github.com/user-attachments/assets/fa4e5e9c-995e-4c28-9768-f8ab0f501446" />

we can call these strategies as algo

fever composition over inheritence

the one who use the algo are client class
strategies are like talking, walking

Conclusion

Encapsulate what varies & keep it separate from what remains same.

Solution to inheritance is not more inheritance.

Composition should be favoured over inheritance.

Code to interface & not to concrete.

Do NOT repeat yourself (DRY).
-------------------------------------------------------------------------------------------------------------------------

Object creation & business logic should be separate that's why we make factory class 

we alsways think how we can decouple the client from the application internals

factory are of three types
1.simple factory {DPrinciple}
2.factory method {DP}
3.abstract factory method {DP}


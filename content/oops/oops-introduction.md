+++
title = "Object Oriented Programming in Java: An introduction"
date = 2025-12-31
author = "Amit Verma"
draft = false
categories = ["oops"]
+++

### Introduction to OOPS

OOPS (Object-Oriented Programming System) is a programming paradigm that is based on the concept of **objects** rather than functions or procedures. Java is fundamentally an object-oriented language, and understanding OOPS is essential for writing clean, maintainable, and scalable Java applications.

At its core, OOPS models real-world entities using **classes** and **objects**, making programs easier to understand, maintain, and extend.

---

### What is a class?

A **class** is a blueprint or template that defines:
- **State** (data / fields)
- **Behavior** (methods)

In Java, a class describes what an object will look like and what it can do, but it does not represent a real entity by itself.

Example:
```java
class Car {
    String color;
    String model;

    void drive() {
        System.out.println("Car is driving");
    }
}
```

Here, `Car` defines the properties and behaviors that every car object will have.

---

### What is an object?

An **object** is a real instance of a class.  
It represents an actual entity created from the class blueprint.

Example:
```java
Car myCar = new Car();
myCar.color = "Red";
myCar.model = "Sedan";

myCar.drive();
```

In simple terms:
- **Class** → definition
- **Object** → real usage

Multiple objects can be created from the same class, each with its own state.

---

### Why object-oriented programming?

OOPS helps developers:
- represent real-world problems in a natural and structured way
- organize code into reusable and modular components
- manage complexity in large applications
- improve code maintainability and future scalability

This is one of the main reasons Java is widely used for **enterprise applications, backend systems, large-scale software development.**.

---

### Basic building blocks of OOPS in Java

While OOPS includes many ideas, Java’s object-oriented approach is based on a few essential building blocks:

- **Class** — defines the structure and behavior of objects  
- **Object** — a real instance of a class  
- **Methods** — define actions an object can perform  
- **Fields** — data that represents an object’s state  

Together, these building blocks form the foundation of Java programming.

---

### The four pillars of OOPS

Java’s object-oriented design is commonly explained using four core principles:

- **Abstraction** — exposing only what is necessary and hiding implementation details
- **Encapsulation** — bundling data and behavior together while controlling access  
- **Inheritance** — creating new classes based on existing ones  
- **Polymorphism** — allowing the same operation to behave differently in different contexts  

Each pillar plays a critical role in building flexible and maintainable Java applications.

---

### Key takeaways

- Object-Oriented Programming models software using real-world entities
- A class defines structure and behavior, while objects represent actual instances
- Java’s OOPS foundation is built on abstraction, encapsulation, inheritance, and polymorphism
- These concepts form the basis for writing clean and maintainable Java applications
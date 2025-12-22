+++
draft = false
tags = ["java", "core java", "java-basics"]
categories = ["Java Core"]
+++

## Java Core Concepts

This section covers **core Java language features** that every Java developer should understand properly.

The focus is on **clarity, correctness, and real-world usage**, not just definitions.

---

## What You’ll Find Here

- Object-Oriented Programming concepts
- Language fundamentals and behavior
- Common misconceptions and pitfalls
- Design-related Java concepts

---

## Start Here (Recommended)

If you’re new to these topics, start with the following articles:

- 👉 [OOPS concepts explained clearly](/java-core/oops-basics/)
- 👉 [`equals()` vs `==` in Java](/java-core/equals-vs-double-equals/)
- 👉 [Immutability in Java: why it matters](/java-core/immutability/)

---

## Common Java Core Topics

- Classes and objects
- Inheritance, composition, and polymorphism
- Method overloading vs overriding
- `final` keyword in Java
- Access modifiers
- Object equality and hashing

---

## How Java Code Is Executed

Let’s walk through the execution process:

1. You write Java source code (`.java`)
2. The compiler converts it into bytecode (`.class`)
3. The JVM loads and executes the bytecode

### Example:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Java");
    }
}

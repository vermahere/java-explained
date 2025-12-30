+++
title = "Immutability in Java: why it matters"
date = 2025-12-30
author = "Amit Verma"
draft = false
categories = ["Java Core"]
+++

### Introduction

Immutability is a core concept in Java that influences how we design classes, manage object's state, and reason about program behavior. While it may seem like an advanced or optional practice at first, immutability plays a crucial role in writing safer, more maintainable, and more concurrent-friendly Java code.

This article explains what immutability is, why it matters, and how to use it effectively in Java.

---

### What immutability means?

An object is immutable if its state cannot be changed after it is created. Once constructed, the values of its fields remain constant for the lifetime of the object.

In Java, common examples of immutable classes include:

- `String`
- Wrapper classes such as `Integer`, `Long`, and `Boolean`
- `LocalDate` and other classes from the `java.time` package

When you modify what appears to be an immutable object, Java actually creates a new instance instead of changing the existing one.

```java
String s1 = "hello";
String s2 = s1.toUpperCase();

System.out.println(s1); // hello
System.out.println(s2); // HELLO
```

The original string remains unchanged.

---

### Why immutability matters?

Immutability provides several important benefits that directly affect code quality and reliability. The following sections examine these benefits in more detail.

#### Thread safety

Immutable objects are inherently thread-safe. Since their state never changes, multiple threads can access them simultaneously without synchronization.

This eliminates entire classes of concurrency bugs such as race conditions and visibility issues.

```java
public final class Config {
    private final int timeout;

    public Config(int timeout) {
        this.timeout = timeout;
    }

    public int getTimeout() {
        return timeout;
    }
}
```

No locks or `synchronized` blocks are required.

#### Simple, reliable and predictable code

When objects do not change state, code becomes easier to understand. You can trust that a value remains the same throughout its usage.

This makes debugging simple because you do not need to track who modified an object and method calls have fewer side effects

#### Safer reuse and sharing

Immutable objects can be freely shared between components, methods, and libraries without defensive copying.

```java
public void process(String input) {
    // No risk of input being modified
}
```

#### Suitable for caching and collections

Immutable objects are well suited for caching and use in collections because their state never change. Once stored, their hash code and equality behavior remain consistent, preventing lookup failures and data corruption. This makes immutable objects reliable keys in hash-based collections and safe entries in caches without requiring defensive copying or additional synchronization.

---

### How to create immutable classes in Java?

The following rules outline a simple approach to creating immutable objects. 

- Declare the class as `final` to prevent subclassing
- Make all fields `private` and `final`
- Initialize all fields in the constructor
- Do not provide setter methods
- Do not expose references to mutable internal objects; return defensive copies when necessary
- Do not store references to mutable constructor arguments; store defensive copies instead

---

### Example of an immutable class

```java
public final class User {

    private final String name;
    private final List<String> roles;

    public User(String name, List<String> roles) {
        this.name = name;
        // Defensive copy of mutable constructor argument
        this.roles = new ArrayList<>(roles);
    }

    public String getName() {
        return name;
    }

    public List<String> getRoles() {
        // Defensive copy to prevent external modification
        return Collections.unmodifiableList(new ArrayList<>(roles));
    }
}

```

Once created, a `User` object cannot be modified.

```java
List<String> roles = new ArrayList<>();
roles.add("USER");

User user = new User("root", roles);

// External modification after object creation
roles.add("ADMIN");

System.out.println(roles);           // [USER, ADMIN]
System.out.println(user.getRoles()); // [USER]

// Attempt to modify internal state via getter
user.getRoles().add("GUEST"); // throws UnsupportedOperationException
```

This example shows that:

- the constructor creates a copy of the incoming `List`, so later changes to the original list do not affect the `User` object.
- the `getRoles()` method returns an unmodifiable view, preventing callers from mutating the internal state through the accessor.
- both measures are required to ensure true immutability when working with mutable fields.

---

### Performance considerations

Immutability may create more objects, but in most cases:
- The performance impact is negligible
- JVM optimizations handle short-lived objects efficiently
- The safety and clarity gains outweigh the cost

For performance-critical code paths, immutability should be evaluated using real measurements rather than assumptions.

---

### When immutability is not ideal?

Immutability is not always the best choice. It may be less suitable when:
- Objects represent large, frequently changing data structures
- Performance constraints demand in-place updates
- The domain model naturally requires mutability

In such cases, controlled mutability with clear ownership can be a valid alternative.

---

### Conclusion

Immutability in Java is more than a stylistic preference. It is a powerful design principle that improves thread safety, simplifies reasoning, and leads to more robust applications.

By favoring immutable objects where possible, you write code that is easier to understand, safer to use, and better prepared for concurrent and scalable systems.

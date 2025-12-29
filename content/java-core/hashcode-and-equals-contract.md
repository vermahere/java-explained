+++
title = "hashCode() and equals() Contract"
date = 2025-12-28
author = "Amit Verma"
draft = false
categories = ["Java Core"]
+++

## Introduction

In Java, `equals()` and `hashCode()` are two fundamental methods inherited from the `Object` class.  
They play a critical role in object comparison and in the correct functioning of hash-based collections such as `HashMap`, `HashSet`, and `Hashtable`.

A misunderstanding of their contract often leads to subtle and hard-to-diagnose bugs in real-world applications.

---

## What is `equals()`?

The `equals()` method is used to determine **logical equality** between two objects.

By default, the implementation in `Object` compares **object references**:

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

This means two distinct objects with identical data are considered unequal unless `equals()` is overridden.

---

## What is `hashCode()`?

The `hashCode()` method returns an integer value that represents the object in hash-based data structures.

Collections like `HashMap` and `HashSet` use this value to determine where an object should be stored internally.

---

## Why Does the Contract Exist?

Hash-based collections rely on **both methods working together**:

1. `hashCode()` determines the bucket
2. `equals()` determines equality within that bucket

If these methods are inconsistent, collections may behave incorrectly.

---

## The `equals()` Contract

A correct `equals()` implementation must satisfy the following properties:

### Reflexive
```java
x.equals(x) == true
```

### Symmetric
```java
x.equals(y) == y.equals(x)
```

### Transitive
```java
if x.equals(y) and y.equals(z), then x.equals(z)
```

### Consistent
Repeated calls return the same result if objects are unchanged.

### Non-null
```java
x.equals(null) == false
```

---

## The `hashCode()` Contract

The relationship between `equals()` and `hashCode()` is strict:

> If two objects are equal according to `equals()`, they **must** return the same `hashCode()`.

Important clarifications:
- Unequal objects may have the same hash code
- Equal objects must never have different hash codes

---

## Correct Implementation Example

```java
class User {
    private final int id;
    private final String email;

    User(int id, String email) {
        this.id = id;
        this.email = email;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User)) return false;
        User user = (User) o;
        return id == user.id && email.equals(user.email);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, email);
    }
}
```

This implementation:
- Uses the same fields in both methods
- Preserves the contract
- Works correctly in hash-based collections

---

## Broken Example (Common Mistake)

```java
class User {
    private int id;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User)) return false;
        User user = (User) o;
        return id == user.id;
    }
    // hashCode() NOT overridden
}
```

### What goes wrong?

```java
User u1 = new User(1);
User u2 = new User(1);

Set<User> users = new HashSet<>();
users.add(u1);
users.add(u2);
```

Even though `equals()` returns true, both objects are added because they produce different hash codes.

---

## Why This Breaks Hash-Based Collections

Hash-based collections follow this process:

1. Compute `hashCode()`
2. Find the bucket
3. Use `equals()` to compare entries

If equal objects have different hash codes, step 3 is never reached.

---

## Hash Collisions

Two unequal objects can legally share the same hash code:

```java
"Aa".hashCode() == "BB".hashCode()
```

Java handles collisions internally using:
- Linked lists (older versions)
- Balanced trees (Java 8+)

Collisions affect performance, not correctness.

---

## Best Practices

- Always override `hashCode()` when overriding `equals()`
- Use immutable fields where possible
- Avoid mutable fields in both methods
- Use `Objects.hash()` for clarity
- Keep implementations simple and consistent

---

## When You Should Not Override Them

- When object identity matters more than logical equality
- For singleton objects
- For objects managed strictly by reference

---

## Summary

- `equals()` defines logical equality
- `hashCode()` supports efficient hashing
- Both must be consistent
- Violating the contract breaks collections
- This is a core Java concept every developer must understand

---

## Related Articles

- [equals() vs == in Java](/java-core/equals-vs-double-equals/)
- How `HashMap` works internally
- Immutability and its impact on hashing

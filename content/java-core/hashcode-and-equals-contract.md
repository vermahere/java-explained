+++
title = "hashCode() and equals() Contract"
date = 2025-12-28
author = "Amit Verma"
draft = false
categories = ["Java Core"]
+++

### Introduction

In Java, `equals()` and `hashCode()` are two important methods inherited from the `Object` class. They play a key role in object comparison and in the correct functioning of hash-based collections such as `HashMap`, `HashSet`, and `Hashtable`.

A lack of understanding of their contract often results in subtle bugs that are difficult to diagnose in real-world applications.

---

### What is `equals()`?

The `equals()` method is used to determine **logical equality** between two objects.

By default, the implementation in `Object` compares **object references**:

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

This means that two distinct objects, even if they contain identical data, are considered unequal by default. To compare them based on their data, you must override the `equals()` method in your class.

---

### The `equals()` contract

A correct `equals()` implementation must satisfy the following properties:

#### Reflexive
```java
x.equals(x) == true
```

#### Symmetric
```java
x.equals(y) == y.equals(x)
```

#### Transitive
```java
if x.equals(y) and y.equals(z), then x.equals(z)
```

#### Consistent
Repeated calls return the same result, provided no information used in equals comparisons on the objects is modified.

#### Non-null
```java
x.equals(null) == false
```
---

### What is `hashCode()`?

The `hashCode()` method returns an integer value that represents the object in hash-based data structures.

Collections like `HashMap` and `HashSet` use this value to determine where an object should be stored internally.

---

### Why does the contract exist?

Hash-based collections rely on **both methods working together**:

1. `hashCode()` determines the bucket
2. `equals()` determines equality within that bucket

If these methods are inconsistent, collections may behave incorrectly.

---

### What is the contract?

The relationship between `equals()` and `hashCode()` is strict:

> If two objects are equal according to `equals()`, they **must** return the same `hashCode()`.

Important clarifications:
- Unequal objects may have the same hash code
- Equal objects must never have different hash codes

---

### Correct implementation example

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
- overrides both `equals()` and `hashCode()`
- uses the same fields (`id` and `email`) in both methods
- preserves the contract
- works correctly in hash-based collections

---

### Broken example (common mistake)

```java
class User {
    private int id;
    
    public User(int id) {
		this.id = id;
	}

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User)) return false;
        User user = (User) o;
        return id == user.id;
    }
    // hashCode() Not overridden
}
```

#### What goes wrong?

```java
User u1 = new User(1);
User u2 = new User(1);

Set<User> users = new HashSet<>();
users.add(u1);
users.add(u2);

System.out.println(u1.equals(u2)); //Returns true

System.out.println(users.size()); //Returns 2
```

Even though `equals()` returns `true`, both objects are added to the `HashSet` because `hashCode()` is not overridden consistently with `equals()`. As a result, they produce different hash codes, which violates the `equals()/hashCode()` contract.

---

### Why this breaks hash-based collections

Hash-based collections follow this process:

1. Compute `hashCode()`
2. Find the bucket
3. Use `equals()` to compare entries

If equal objects have different hash codes, step 3 is never reached because the objects end up in different buckets and are therefore treated as distinct entries.

---

### Hash collisions

Two unequal objects can legally share the same hash code:

```java
"Aa".hashCode() == "BB".hashCode()
```

Java handles collisions internally using:
- Linked lists (older versions)
- Balanced trees (Java 8+)

Collisions affect performance, not correctness.

---

### Key takeaways

- Always override `hashCode()` when overriding `equals()`
- Avoid mutable fields in both methods
- Use `Objects.hash()` for clarity
- Keep implementations simple and consistent
- Violating the contract breaks collections

---

### Related articles

- [equals() vs == in Java](/java-core/equals-vs-double-equals/)

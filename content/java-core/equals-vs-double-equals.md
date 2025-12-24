+++
title = "equals() vs == in Java"
draft = false
+++

### equals() vs == in Java

Understanding the difference between `equals()` and `==` is one of the most common — and important — topics in Core Java.  
Although they may appear similar, they serve very different purposes, and confusing them can lead to subtle bugs.

This article explains what each one does, why they exist, and when to use which.

---

### What does `==` do in Java?

The `==` operator compares whether two references point to the same object in memory.

For reference types, it checks identity, not content.

#### Example

```java
String a = new String("java");
String b = new String("java");

System.out.println(a == b); // false
```

Even though `a` and `b` contain the same text, they refer to different objects, so `==` returns `false`.

---

### What does `equals()` do?

The `equals()` method is used to compare logical equality — whether two objects are meaningfully equal.

By default, `equals()` behaves the same as `==` because it is inherited from `Object`:

```java
public boolean equals(Object obj) {
    return this == obj;
}
```

However, many Java classes override `equals()` to define what equality means for them.

---

### Example: String equality

```java
String a = new String("java");
String b = new String("java");

System.out.println(a.equals(b)); // true
```

`String` overrides `equals()` to compare character content, not object identity.

This is why `equals()` is almost always used when comparing strings.

---

### Why does Java have both `==` and `equals()`?

They solve different problems:

- `==` answers: Are these two references pointing to the same object?
- `equals()` answers: Do these two objects represent the same value?

Both are necessary.

Examples where `==` is useful:
- Checking if two references are literally the same instance
- Comparing enum values
- Performance-critical identity checks

Examples where `equals()` is required:
- Comparing strings
- Comparing value objects
- Business logic comparisons

---

### Common mistake: Using `==` for object comparison

```java
Integer x = 1000;
Integer y = 1000;

System.out.println(x == y);      // false
System.out.println(x.equals(y)); // true
```

Here:
- `==` compares references
- `equals()` compares values

Relying on `==` for wrapper classes can produce unexpected results, especially outside the integer cache range.

---

### What about primitives?

For primitive types, `==` compares actual values.

```java
int a = 10;
int b = 10;

System.out.println(a == b); // true
```

There is no `equals()` method for primitives.

---

### Overriding `equals()` in your own classes

If your class represents a value, you should override `equals()`.

```java
class Person {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Person person = (Person) o;
        return age == person.age && name.equals(person.name);
    }
}
```

Failing to override `equals()` correctly can break:
- Collections (`HashSet`, `HashMap`)
- Caching logic
- Business rules

---

### Relationship between `equals()` and `hashCode()`

Whenever you override `equals()`, you must also override `hashCode()`.

The contract is simple:

> If two objects are equal according to `equals()`, they must have the same `hashCode()`.

Violating this contract leads to unpredictable behavior in hash-based collections.

---

### When should you use each?

Use `==` when:
- Comparing primitives
- Comparing enums
- Checking object identity intentionally

Use `equals()` when:
- Comparing strings
- Comparing wrapper classes
- Comparing domain or value objects

---

### Key takeaway

- `==` checks reference equality
- `equals()` checks logical equality
- They are not interchangeable
- Using the wrong one can introduce subtle bugs

Understanding this distinction is fundamental to writing correct and predictable Java code.

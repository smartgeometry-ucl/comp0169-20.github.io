---
title: Casting
nav_order: 3
---

The compiler doesn't always have complete information about the actual type of a variable. As we saw, it's impossible to know whether the actual type of `p` is `iPhone` or `CDPlayer` until the program is actually executed.

```java
MusicPlayer p;
if (new Random().nextBoolean()) {
    p = new iPhone();
} else {
    p = new CDPlayer();
}
p.play();
p.phoneCall(); // MusicPlayer cannot call phoneCall
```

In the circumstances when this restriction might be too harsh, the programmer can use a **cast** to temporarily change the **promised type** of an expression. This is typically used to change the promised type from a more general type like `MusicPlayer` to a more specific type like `iPhone`.


```java
MusicPlayer p;
if (new Random().nextBoolean()) {
    p = new iPhone();
} else {
    p = new CDPlayer();
}
p.play();
((iPhone) p).phoneCall(); // iPhone can call phoneCall
```

The compiler performs a consistency check to confirm that the cast is possible. At runtime, the Java interpreter performs another consistency check to confirm that the actual type is truly compatible with the casted promise type.

Compile-time casting check
: Is it possible for the value of a variable `p` with promised type `MusicPlayer` to have actual type `iPhone` (or a subtype of `iPhone`)?

Runtime casting check
: Check to make sure that the actual type works with the promised type. Otherwise, throw a `ClassCastException`.

The runtime check is necessary since it's possible that the actual type of `p` is actually `CDPlayer`, in which case `CDPlayer` cannot call `phoneCall()`. The code will compile since it's possible that `p` has an actual type of `iPhone` based on the relationship between `MusicPlayer` and `iPhone`, but it will throw a `ClassCastException` at runtime if it turns out that `p` is actually a `CDPlayer`.

Note
: Casting does not change the actual type of an object. It only changes the **promised type** of the expression! It's not possible to change the actual type of an object using casting.

In summary, the process for determining the result of executing a program with an inheritance hierarchy is complicated.

Compile-time
: The compiler only considers the promised type of an expression.
  1. **Determine the promised type of the expression**. Validate any casting.
  1. **Determine the compile-time method signature**. Look in the promised type before searching its superclasses. If there are multiple methods in the class that could work with the promised parameter types, choose the method that most closely matches the promised types.

Runtime
: 1. Validate any casting by double checking that the actual type is a subclass of the cast type.
  1. If the method is a `static` method or if it is accessed through the `super` keyword, call the method determined at compile-time.
  1. Since the actual type of the object might override methods declared by the promised type, call the method closest to the actual type that **exactly matches the compile-time method signature**.

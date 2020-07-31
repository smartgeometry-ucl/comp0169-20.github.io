---
title: Compile-time
nav_order: 2
---

Before running a Java program, it needs to be compiled using the `javac` command. We've already run into many times where the Java compiler catches mistakes (like using values of the wrong type) or typos (like calling a method that isn't actually defined).

Java compiler `javac`
: A program that translates source code to "bytecode" that can be executed by the Java runtime.
: The goal of the Java compiler is to ensure that every statement can (at least in theory) run.

```java
List<Integer> list = new ArrayList<Integer>();
list.containsKey(5); // List cannot call containsKey
```

Let's study this closely. Consider the following inheritance hierarchy representing real-world music players.

- `Object`
  - `MusicPlayer`
    - `MP3Player`
      - `iPod`
        - `iPhone`
      - `Zune`
    - `TapeDeck`
    - `CDPlayer`

As with interface types, the left-hand side type (**promise type**) does not need to be the same as the right-hand side type (**actual type**). Likewise, we can declare a `MusicPlayer` as the promise type with its value as a reference to the actual type `iPhone`, for example.

```java
MusicPlayer p = new iPhone();
```

The compiler checks this line against the inheritance hierarchy.

> Is `iPhone` a `MusicPlayer`?

Looking at the inheritance hierarchy above, `iPhone` is indeed a `MusicPlayer` so we can `p.play()` music.

However, we cannot call `p.phoneCall()` even though the actual type is `iPhone`. By declaring the promise type as `MusicPlayer`, the compiler can only be guaranteed access to the methods and fields of `MusicPlayer` and its super-classes. The intuition behind this is that someone might write a method that returns a `MusicPlayer`, but randomly picks between an `iPhone` and a `CDPlayer` as the actual type, so there's no way for the compiler to know the actual type of `p` until running the code.

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

The compiler keeps track of the details of which methods are available to which types in what's called **virtual method table**. The compiler refers to this table to make sure that every method call can be resolved at runtime. In order to build the virtual method table, the compiler associates each class with its methods' signatures.

Method signature
: The unique identifier for the method given by the name of the method and its parameter types. Two methods cannot share the same signature in the same class.
: Overriding
  : A subclass defines a method with the same exact signature as a method in its superclass.
: Overloading
  : A class defines multiple methods with the same name but different parameter types.
  : `List.add` is overloaded: one method appends to the end while another adds at an index.

The reason why it's called a virtual method table (as opposed to just "method table") is because the compiler builds the table for method overriding. The virtual method table for a subclass like `iPhone` contains not only all methods explicitly defined in `iPhone`, but also all of the "virtual methods" defined in its superclasses. This way, `iPhone` can inherit methods like `play()` without having to define the method again.

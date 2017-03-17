---
layout: post
title:  Effective Java
author: Rahul Pandey
date:   2017-03-16
categories: java
---

*Effective Java* is a programming guide written by Josh Bloch, initially published in 2001 and updated in 2008. The former "Chief Java Architect" at Google, Bloch does an excellent job in distilling best practices in Java, and the reasoning behind it. The ideas presented can be transferred to other languages. Here's my attempt to summarize some insightful sections. 

## Classes and interfaces

- Make each class or member as inaccessible as possible: this is a key tenet of software engineering, to hide complexity and implementation details. 

- Use protected fields rarely: protected fields are accessible from subclasses of the class where it is declared, and from any class in the declared package. 

- Instance fields should never be public.
  * By making a field public, you give up the ability to limit the values that can be stored in the field.
  * Classes with public mutable fields are not thread-safe.
  * An exception can be made for degenerate classes which simply group instance fields, if the class is package-private or private nested. 

- Minimize mutability: in Java, this can be done by marking the class as final, or making all fields final. 
  * Immutable objects are simple since they must exist in the same state as when they were created. 
  * Immutable objects are inherently thread-safe; they require no synchronization.

- Favor composition over inheritance
  * Inheritance violates encapsulation, since a subclass depends on the implementation details of the superclass, which may change from release to release. 
  * With composition (also called the decorator pattern), you give your new class a private field that references an instance of the existing class. Instance methods in the new class invoke a corresponding method in the existing class, i.e. method forwarding. 
- Prefer interfaces to abstract classes
  * Abstract classes are permitted to contain implementations for some methods while interfaces are not. 
  * Existing classes can be easily retrofitted to implement a new interface, and interfaces are ideal for defining mixins. 
  * Abstract classes are good for type hierarchies, but often times things don't fall neatly into a rigid hierarchy. 
  * Once an interface is released/implemented by clients, it is almost impossible to change. It is much easier to evolve an abstract class. 

## Generics

- Generics: for maximum flexibility, use wildcard types on input parameters that represent producers or consumers.
  * For any 2 distinct types `Type1` and `Type2`, `List<Type1>` is neither a subtype not supertype of `List<Type2>`. It’s counterintuitive that `List<String>`is not a subtype of `List<Object>`, but you can only put in Strings in the former, but any object in the latter.
  * If you have a `List<String>` and want to use it in a method that takes in a list of object, we need the parameter to be a wildcard type: `List<? extends Object>`.
  * PECS: producer-extends, consumer-super. If a parameterized type represents a `T` producer, use `<? extends T>`; if it represents a `T` consumer, use `<? super T>`.


## General programming
- When instantiating an object, consider static factory methods instead of constructors.
  * Static factory methods have names, unlike constructors.
  * Static factory methods are not required to create a new object each time they’re invoked.
  * Static factory methods can return an object of any subtype of their return type.

- If an constructor has a large number of parameters, or many optional parameters, consider the Builder pattern, which uses a public static inner class to set all the parameters.

- Prefer interfaces to reflection. Reflection in Java offers programmatic access to information about loaded classes. There are several downsides to the power of reflection.
  * You lose all the benefits of compile-time type checking.
  * Code is verbose, and performance suffers.

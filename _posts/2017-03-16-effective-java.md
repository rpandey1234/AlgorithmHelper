---
layout: post
title:  Effective Java
author: Rahul Pandey
date:   2017-03-16
categories: java
---

Effective Java is a programming guide written by Josh Bloch, initially published in 2001 and updated in 2008. The former "Chief Java Architect" at Google, Bloch does an excellent job in distilling best practices in Java, and the reasoning behind it. The ideas presented can be transferred to other languages. Here's my attempt to summarize some insightful sections. 

## Classes and interfaces

- Make each class or member as inaccessible as possible: this is a key tenet of software engineering, to hide complexity and implementation details. 

- Use protected fields rarely: protected fields are accessible from subclasses of the class where it is declared, and from any class in the declared package. 
- Instance fields should never be public.
By making a field public, you give up the ability to limit the values that can be stored in the field.
Classes with public mutable fields are not thread-safe.
An exception can be made for degenerate classes which simply group instance fields, if the class is package-private or private nested. 

- Minimize mutability: in Java, this can be done by marking the class as final, or making all fields final. 
Immutable objects are simple since they must exist in the same state as when they were created. 
Immutable objects are inherently thread-safe; they require no synchronization.

- Favor composition over inheritance
Inheritance violates encapsulation, since a subclass depends on the implementation details of the superclass, which may change from release to release. 
With composition (also called the decorator pattern), you give your new class a private field that references an instance of the existing class. Instance methods in the new class invoke a corresponding method in the existing class, i.e. method forwarding. 

- Prefer interfaces to abstract classes
Abstract classes are permitted to contain implementations for some methods while interfaces are not. 
Existing classes can be easily retrofitted to implement a new interface, and interfaces are ideal for defining mixins. 
Abstract classes are good for type hierarchies, but often times things don't fall neatly into a rigid hierarchy. 
Once an interface is released/implemented by clients, it is almost impossible to change. It is much easier to evolve an abstract class. 


## Real World Application

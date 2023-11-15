---
layout: post
title: "Var in java"
description: "`var` was aimed at improving the developer experience in Java 10, this blog explores the arguments aganist it"
comments: true
keywords: "java, var  JEP 286"
---


The introduction of `var` was aimed at improving the developer experience in Java 10 through [JEP 286](https://openjdk.org/jeps/286). Java is well-known for its verbosity, and this JEP was designed to reduce the ceremony associated with writing Java code by allowing developers to elide the often-unnecessary manifest declaration of local variable types, while still maintaining Java's commitment to static type safety.

For example 

```java 
//before
PriorityQueue<Item> itemQueue = new PriorityQueue<Item>();

//after using var 
var   itemQueue = new PriorityQueue<Item>();

```

And there is a certain amount of controversy over this feature in the community. The blog explores arguments against the use of var are outlined in the following discussions.

 ### Loss of Explicitness  & Readability

>Using **var** can make the code less readable, especially when the inferred type is not immediately obvious. Explicitly declaring the type can improve code clarity and make it easier for other developers to understand.

This arugments mostly arised from the fact Developers spend much more time reading code than writing code. Which widely stated in the  *A Handbook of Agile Software Craftsmanship* Robert C. Martin claims “the ratio of time spent reading versus writing is well over 10 to 1”.



### Code Review and Collaboration

>In a team setting, different team members may have different preferences regarding the use of var.
>Code readability shouldn’t depend on IDEs.
>
#Conculsion
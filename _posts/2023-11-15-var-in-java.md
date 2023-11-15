---
layout: post
title: "Var in Java"
description: "`var` was aimed at improving the developer experience in Java 10, this blog explores the arguments aganist it"
comments: true
keywords: "java, var  JEP 286"
---


The introduction of `var` was aimed at improving the developer experience in Java 10 through [JEP 286](https://openjdk.org/jeps/286). Java is well-known for its verbosity, and this JEP was designed to reduce the ceremony associated with writing Java code by allowing developers to omit the often-unnecessary manifest declaration of local variable types, while still maintaining Java's commitment to static type safety.

For example 

```java 
//before
PriorityQueue<Item> itemQueue = new PriorityQueue<Item>();

//after using var 
var itemQueue = new PriorityQueue<Item>();
```

And there is a certain amount of controversy over this feature in the community. The blog explores arguments against the use of `var` and are outlined in the following discussions.

### Loss of Explicitness

>Explicitly declaring the type can improve code clarity and make it easier for other developers and future readers.

While the argument against using var suggests that explicit type declarations improve code clarity, there are scenarios where the judicious use of var can enhance readability and maintainability.Consider the following for example.

```java 
void removeMatches(Map<? extends String, ? extends Number> map, int max) {
    for (Iterator<? extends Map.Entry<? extends String, ? extends Number>> iterator =
             map.entrySet().iterator(); iterator.hasNext();) {
        Map.Entry<? extends String, ? extends Number> entry = iterator.next();
    }
}
```

Use of var here removes the noisy type maifest which improves the readabiltiy

```java
void removeMatches(Map<? extends String, ? extends Number> map, int max) {
    for (var iterator = map.entrySet().iterator(); iterator.hasNext();) {
        var entry = iterator.next();
    }
}
```
---
> Developers are not comfortable with type interferncing.

Java developers have already embraced intermediated intefered types in streams. In the following code 

```java 
int maxWeight = blocks.stream()
                      .filter(b -> b.getColor() == BLUE)
                      .mapToInt(Block::getWeight)
                      .max();
```
no one is bothered (or even notices) that the intermediate types Stream<Block> and IntStream, as well as the type of the lambda formal b, do not appear explicitly in the code.

Certainly, there are situations where using var can lead to unintended behavior, especially when dealing with primitives. Special attention should be given to these cases to ensure the expected behavior, as the automatic type inference of `var` might not always align with the intended types, particularly in scenarios involving primitive data types.

```java
// before
byte flags = 0;
short mask = 0x7fff;
long base = 17;

// all infer as int
var flags = 0;
var mask = 0x7fff;
var base = 17;
```


## Readability

> Using **var** can make the code less readable.

The arugment mostly arises from the fact developers spend much more time reading code than writing code. Robert C. Martin claims in *A Handbook of Agile Software Craftsmanship* that "the ratio of time spent reading versus writing is well over 10 to 1".

The focus should be on understandabiltity of the program rather than on the numbers of words which used to describe the program.Consider the following code 

```java 
Map<String,List<Students>> myMap = new HashMap<>(); 
```
In general, opting for descriptive variable names instead of generic ones like `myMap` aligns with good programming practices. The use of `var` similarly demands conscientious variable naming from the developer. It underscores the importance of choosing meaningful names that convey the business context and purpose of the variable.

```java
var studentsByCategory = new HashMap<String, List<Students>>(); 
```

This inturn makes the code more concise and readable, especially when the type information is obvious from the right-hand side of the assignment. This can reduce boilerplate code and make the intent of the code more apparent.

---
>Code readability shotuldn’t depend on IDEs.

Consider the following code 

```java
//before
try (Stream<Customer> result = dbconn.executeQuery(query)) {
    return result.findAny();
}
```

The argument contends that if the variable result were declared using var, it could be challenging to comprehend, especially without the aid of an Integrated Development Environment (IDE). This could impede the readability and effectiveness of code review.

Code should be self-revealing. It should be inherently understandable without relying heavily on tools. As discussed earlier, employing better variable naming practices serves as a solution to the aforementioned problem.

```java
// after 
try (var customers = dbconn.executeQuery(query)) {
    return customers.findAny();
}
```

Limiting the scope of local variables is good practice in general.

### Code Review and Collaboration

>In a team setting, different team members may have different preferences regarding the use of var.

While the argument against using `var`` in a team setting is valid, it's essential to note that coding conventions and team agreements can help mitigate these challenges. 

There is a great documentation by open jdk on how to use [Local Variable Type Inference](https://openjdk.org/projects/amber/guides/lvti-style-guide) aka `var`


# Conculsion

Embracing a balance between explicit type declarations and concise code, developers can enhance the overall readability and maintainability of their Java code.

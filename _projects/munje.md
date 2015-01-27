---
title: Munje
layout: page
description: A temporal logic programming language designed for humans.
links:
  - title: Spec
    url: /projects/munje.html
  - title: Github
    url: https://github.com/project-nsmg/munje
---

**Munje** is a programming language for advanced human-computer communication. It was designed to use **relation logic** to represent both data and code. Munje is time-static, which means there is totally no changes inside the language. Instead, time is represented as an addition to the relation logic. The design philosophy of Munje is highly influenced by [Prolog](https://en.wikipedia.org/wiki/Prolog Wikipedia: Prolog) and [Cosmos](https://github.com/mcsoto/cosmos Cosmos: A New Logical Language). It also has a syntax that is similar to [Python](https://en.wikipedia.org/wiki/Python_%28programming_language%29 Wikipedia: Python) and [Lisp](https://en.wikipedia.org/wiki/Lisp_%28programming_language%29 Wikipedia: Lisp). This article introduces the design principles of the Munje programming language and its time-static feature.

**Keywords:** programming language, logical, time-static, munje, prolog, lisp

### News

A developping implementation can be found at [Github](http://github.com/sorpaas/munje).

### Introduction

Programming languages are created to make computers understand human beings. Munje is a logical time-static programming language created by [sorpa'as plat](http://sorpaas.com) sorpa'as plat]. It has a simple yet powerful syntax. Munje approaches side effects in a different way called time-static compared with [I/O Monad](https://en.wikipedia.org/wiki/Monad_%28functional_programming%29#The_I.2FO_monad) in functional programming languages such as Haskell.

Some features of Munje include:

* Data and code are represented in the same structure called **relation**.
* Easy to learn. The syntax is simple, with less than 5 keywords.
* Logical programming. You define *what you want to do*, but not *how to do it*.
* Time-static. Unlike in functional programming language where although side effects is explicit, it is unmanaged, in Munje, side effects is processed in a managed way.

#### The name "Munje"

The name "Munje" is a [Lojban](http://lojban.org) word meaning "the universe".

### Philosophy

Munje is designed to be simple and powerful. It believes that we should tell machines what to do, but not how to do it. The are only 4 core concepts in this language, namely **relations**, **terms**, **arguments**, and **paragraphs**. The program as follows is a simple program that calculates the factorial:

    define factorial
      factorial 0 1
      factorial n y
      (>) n 0
      (-) n 1 n'
      factorial n' y'
      (*) y' n y

### Concepts and Syntaxes

In this section, we will introduce some concepts of Munje.

#### Relations, Arguments and Terms

**Relations** are declarations claiming something is held. A relation consists of one **term**, defining the type of the relation, and zero or more **arguments**, defining the content of the relation.

Terms are represented by symbols or partial relations. Each term has a certain number of arguments. For example, "<code>(+)</code>" is a terms that require 3 arguments "<code>a</code>", "<code>b</code>" and "<code>c</code>". It defines a relation between "<code>a</code>", "<code>b</code>" and "<code>c</code>" which in mathematics, known as "*c = a + b*". At the same time, a partial relation also represent a term. For example, "<code>(+) 1</code>" defines a relation between "<code>b</code>" and "<code>c</code>" where "*c = 1 + b*", "<code>(+) 1 1</code>" defines a relation with "<code>c</code>" where "*c = 1 + 1*", that is "*c = 2*", and "<code>(+) 1 1 2</code>" defines a relation with zero arguments that represents "*2 = 1 + 1 exists in the world*". Note that the last term "<code>(+) 1 1 2</code>" is also a relation.

Arguments are terms that put starting from the second place of relations. In this way, term becomes the first-class citizen in the Munje world that can be easily passed into different relations. For example, "<code>if</code>" is a term that requires 3 arguments, where the first is a relation that may be held or not, the second is a relation that will be held if the first relation is true, and the third is a relation that will be held if the first relation is false. In the next section, we will see a implementation of "<code>if</code>" term in Munje.

Munje is made up of declarations of relations. Relations are "declared" in the top level of the program. Those declared relations will be marked as "held" for further computation. Note that if a relation is not in the top level, it is not necessarily held, because there may be "<code>and</code>", "<code>or</code>", or "<code>not</code>" relations around them. Terms are implicitly defined by a declaration of a term.

For example, the program as follows defines a "<code>null</code>" value that we may be able to use in other parts of the program:

     null null

The declaration creates a relation "<code>null null</code>" that is marked as held. It implicitly creates the term "<code>null</code>" that represents there is no value exists. Here we make the term "<code>null</code>" an one-argument term because it becomes convenient for us to check whether something is an equivalent of "<code>null</code>" or not, like the example as follows, where different relations are marked held based on whether "<code>some_term</code>" is null or not:

     or (and (null some_term) [relation when some_term is null])
        (and (not (null some_term)) [relation when some_term is not null])

##### Pattern Matching of Terms

Determining whether two terms are equal plays an important role in the language Munje. Munje uses a pattern matching way similar to other functional programming languages. Two terms are equal if and only if it represents the same thing. That is, all parts of the terms should be equal.

#### Paragraphs

**Paragraphs** are used to separate different functions of a program. It creates an environment that has special properties outside the paragraph. Paragraphs are defined using the "<code>define</code>" keyword. Note that indentation is used to identify the end of a paragraph. The factorial program is used again as an example:

     define factorial
       factorial 0 1
       factorial n y
       (>) n 0
       (-) n 1 n'
       factorial n' y'
       (*) y' n y

##### Exposing

In the first line of the program, "<code>define factorial</code>" creates a paragraph. At the same time, it **expose** the term "<code>factorial</code>" and make it accessible to the outside world. All other terms inside the paragraph is not accessible to the outside world. This makes sure that a program can be properly arranged and different parts of the program don't influence each other improperly.

##### Uncertainty

Uncertainty means when a term is not referred, its relations with others will not be determined. This allows different '''instances of a paragraph''', which is created each time a exposed value is called, to be implemented. For example, consider the program as follows based on the factorial program.

     factorial 5 a
     factorial 4 b

Because factorial is defined and exposed. Those two lines refer to the same term "<code>factorial</code>". However, because terms "<code>n</code>", "<code>n'</code>", "<code>y</code>", and "<code>y'</code>" are defined inside the paragraph, its value will not be determined until the compiler find "<code>factorial 5 a</code>" and "<code>factorial 4 b</code>". Therefore, "<code>n</code>", "<code>n'</code>", "<code>y</code>", and "<code>y'</code>" have different values for "<code>factorial 5 a</code>" and "<code>factorial 4 b</code>". At the same time, inside the paragraph, since "<code>factorial</code>" is exposed, each time when a "<code>factorial</code>" relation is created, a new instance of paragraph will be created. This creates a recursion.

##### Immutability

After one term is exposed, its value cannot be changed and its relation cannot be further limited.

### Data Structure

In Munje, data structures are created totally by relations. Below there will be example codes for some ways to create those data structures.

#### Core Terms

In this section, we define some core terms to make creating data structure easier.

##### equals
"<code>equals</code>" is used to determine whether two arguments are equal:

    define equals
      equals x x

##### similar

"<code>similar</code>" is used to determine whether two arguments have the same "type". It is held if and only if two arguments are equal, or two arguments are applied with same relations.

    define similar
      similar a b
      or (equals a b) (and (rel a) (rel b))

##### if

"<code>if</code>" is used to determine which relation should be true based on a statement relation:

    define if
      if statement if_true if_false
      or (and statement if_true) (and (not statement) if_false)

#### Types

Types are implemented as relations inside Munje. The "<code>similar</code>" function is useful to determine whether two terms have the same type.

##### Cons

**Cons** are used to represent a sequence of things, i.e. lists.

Cons requires 2 arguments. The first argument is an item inside the list, and the second one is also a Cons relation. One implementation of Cons in Munje can be represented as follows:

    define cons
      cons head tail
      equals tail (cons head' tail')
      similar head head'
      similar tail tail'

"<code>head</code>" and "<code>tail</code>" can be used to retrieve the item of a Cons list.

    define head
      head list value
      equals list (cons head _)

    define tail
      tail list value
      equals list (cons _ tail)

##### Integer and String

**Integer** and **String** are implemented by compiler. Integer are zero-argument terms. String is Cons of Characters.

##### Booleans

**Booleans** needs to be explicitly defined inside Munje, for example:

    boolean true
    boolean false

After boolean type is defined, we can easily convert any relations into boolean:

    define to_boolean
      to_boolean rel val
      or (and rel (equals val true)) (and (not rel) (equals val false))

##### Association List

**Association List** is a list of keys and values in which a value can be retrieved based on a key. In Munje, association list is based on the **pair** type:

    define pair
      pair a b

An example implementation of association list can be created as follows:

    define association_list
      equals (association_list item _) (cons item _)
      pair item

**get** relation for association list can be created based on the definition.

#### Immutability

Because terms are immutable, the data structure based on terms are immutable as well. However, side effects is still possible based on the time-tense API.

### Time-static

The concept **time** in Munje is used to represent different states and cycles a program may go through while running. **Time-static** means in Munje, time is represented as normal terms. There are no time functionalities in the language specification. Instead, time-tense API is implemented in compiler and standard library to provide this functionality.

The idea of time-static is based on the **Event-token reification** in [Temporal Logic](http://plato.stanford.edu/entries/logic-temporal). In each state, instead of claiming a term or variable is held, a relation with that variable together with an **timepoint** is held. This allows "time travel" in some extent. You can refer to a variable in any state anywhere in your code, because in Munje, time is just another dimension like anything else. The compiler will figure out at which time, which relation should be held.

#### Time-tense API

The time-tense API has the relations as follows.

* **start_timepoint**: the timepoint when the program starts running.
* **end_timepoint**: the timepoint when the program ends.
* **timepoint**: declare a new timepoint.
* **after**: declare a statement is held after a certain timepoint.
* **before**: declare a statement is held before a certain timepoint.
* **at**: declare a statement is held at a certain timepoint.
* **during**: declare a statement is held during a certain timepoint. **during** is the combination of **before** and **after**.

#### Example

Below is an example using time-tense API to read a line from the console:

    define readline
      readline result after_timepoint before_timepoint
      at after_timepoint console content_start
      at sometime console content_end
      at sometime' console content_end_nextline
      during after_timepoint before_timepoint sometime
      during after_timepoint before_timepoint sometime'
      before sometime sometime'
      (++) content_end "\n" content_end_nextline
      (--) content_end content_start result

This **readline** term takes 3 arguments. The first argument is the placeholder for the result. The second and third is usually provided by a **do** relation. It put the value of **content_start** at the beginning time of the **readline**. And it tries to find a certain time (called **sometime**) when after a period of that certain time (called **sometime**'), a new line character is written. It calls this value **content_end**. The difference of **content_end** and **content_start** is the result of **readline**.

### Lojban Terminology

In this section, we provide a reference for the equivalent terminology in Lojban.

* **Relation**: bridi
* **Term**: selbri
* **Argument**: sumti
* **Paragraph**: girzu

### Implementation of Munje's Time-static Feature

Due to man-power reason, it would be interesting to first implement the time-static feature in other programming languages or a toy programming language, and then back to implement other parts of it.

#### Required Data Structure

* Atom
* List

#### Methodology

Time-static could be treated as a generalized situation of "dependency resolving". Based on the logic provided by the source code, the compiler will try to resolve all involving parts related, and generate a ''List'' as above data structure. The list represent a sequence that should be true during the runtime.

### Related Programming Languages

Munje's concept and language design is similar to other programming languages in some ways:

* Prolog
* Idris

### Conclusion

As a logical time-static programming language, Munje allows people to write programs in a simple and powerful way. It can be used both for querying and executing, which allows easy debugging and high stability. For advanced human-computer communication, Munje can be used by voice to make commands and queries.

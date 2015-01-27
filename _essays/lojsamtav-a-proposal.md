---
title: "Lojsamtav: a Proposal"
layout: page
year: 2014
---

<p class="message">This is an archived proposal. Currently the language is called <a href="/projects/munje.html">Munje</a></p>

This proposal describes the draft of a Lojban time-tensed programming
Language called Lojban.

Background
----------

### Desgin Indicators

-   Imperative
-   Object-oriented
-   Functional
-   Procedural
-   Generic
-   Reflective
-   Event-driven

### What Haskell Did Wrong

About Monad and pure functions.

Principle
---------

-   Logical Language
-   Time tense (before and after)

Philosophy
----------

Simple and easy to understand.

Used Temporal Logic
-------------------

-   Tense Logic
-   Event-token reification

Design
------

All is “relations”, or “bridi”. A relation has one “logical predicate”,
or “selbri”. A relation has many “arguments”, or “sumti”.

**sumti** itself is another **bridi**.

The text of a program is made of declarations of **bridi**s. The root of
the declarations are considered “truth”. Whether the inner parts are
truth depends on the logic and “and” and “or” connections.

Pattern Matching.

### Referring Things

#### Constant

The most common thing in lojysamtav is **constant**. A constant only
represent one single thing, and it is implied through all the logic in
the program.

#### Def Variables

The program has a special syntax like this:

    def (=) $x $x:
      pass

    def cons $x $y:
      (=) y (cons a b)
      (=) (r1 x) (r1 a)
      (=) (r2 y) (r2 b)

In **def**, each time the situation is fitted, a new r1, r2, a, b will
be created for the situation to give logic to \$x and \$y.

### Time Tense API

    at $statement $time_indicator

Only at the time \$time\_indication is \$statement true.

    before $statement $time_indicator

Before \$time\_indicator to an unknown time, the \$statement is true. If
there are no other restrictions, default to the beginning of the world.

    after $statement $time_indicator

After \$time\_indicator to an unknown time, the \$statement is true. If
there are no other restrictions, default to the end of the world.

Concepts
--------

-   Relations from Logical Language Cosmos
-   Functors from Logical Language Cosmos
-   Time points
-   Constant and variable
-   Term and relation

### Data Structure

-   Tuple/Functors/Relations (They are the same thing)
-   String
-   Real
-   Integer

### Boolean: Always True

### Side Effects

There's totally no side effects in this world. Everything is just logic
with time tense. And data is managed all by the program.

Unmanaged Data
-----------------

### Managed Data

### Unmanaged Data

Might change unexpectly. Write a program that can deal with this
situation.

Use `True or False` so that it is still always true and not break
anything. Also
`(True and (situation and code for true)) or (False and (situation and code for false))`.

Compiler
--------

Nothing to Hide
---------------

Immutability
------------

If Statement
------------

Lojban
------

Time Travel
-----------

Examples
--------

### Factorial Function

    def (=) $x $x:
      pass

    def if $statement $if_true $if_false:
      (statement and if_true) or (statement xor if_false)

    def f $x $y:
      if ((=) x 0) ((=) y 1) (f ((-) x 1) ((/) y x))

It can be called:

    f x y
    x = 3
    #=> y = 6

Version 2:

    define factorial:
      factorial 0 1
      factorial *n *y
      (>) *n 0
      (-) *n 1 *n'
      factorial *n' *y'
      (*) *y' *n *y

TODO: This might be hard to write a parser. Might human just give
“instructions” for solving these problems?

### Ask for User Input

    def (=) $x $x

    def cons $head $tail:
      tail = cons a b
      r1 head
      r1 a
      r2 tail
      r2 b

    def null $x:
      x = null

    questions = cons "Who are you" (cons "Where do you come from?" (cons "Where are you going?" null))
    questions = ["Who are you?", "Where do you come from?", "Where are you going?"]

    def head $list $h:
      list = cons h _

    def tail $list $t:
      list = cons _ t

    def more $list $has_more:
      tail list t
      null t has_more

    def each $list $statement:
      more list has_more
      if (more ) ((rel (head x)) and (each (tail x) rel)) (rel (head x))

    def if $statement $if_true $if_false:
      (statement and if_true) or (statement xor if_false)

    def not $statement $if_not:
      statement xor if_not

    lines x y iff
      a = head x
      head x a
      if (a = "\n")
        lines (tail x) y
        lines (tail x) (y - 1)
      if (null y) (if (a = "\n") (tail x 1) (tail x 0))

    console = "" when init_console
    console = (console before x) ++ a after x where x = print a

    read x from_state iff
      end_state = ((console after x) = (console before x) + "\n" after from_state) # There may be several occations (?)
      start = console when from_state
      end = null before end_state
      end = console when end_state
      x = null before end_state
      x = end -- start after end_state

    println x = print (x ++ "\n")
    println "Hi, we are going to ask you some questions" after init_console before init_asking
    each questions ask after init_asking

    ask x answer from_state end_state iff
      print x after from_state when print_state
      read answer print_state before end_state

Version 2:

    define equals:
      equals x x

    define alias:
      #TODO write this ...

    define similar:
      similar a b
      or (equals a b) (and (rel a) (rel b))

    define cons:
      cons head tail
      equals tail (cons head' tail')
      similar head head'
      similar tail tail'

    equals questions (cons "Who are you?" (cons "Where do you come from?" (cons "Where are you going?" null)))
    equals questions (cons ["Who are you?" "Where do you come from?" "Where are you going?"])

**define** works like in quantum. When it is exposed and there's
something referring it, the value is there and it must be single. For
example, when I say **define similar**, similar is exposed, so it refers
to a single thing. So does **or**, **equals** and **and** inside
**define similar**. However, **a**, **b**, and **rel** is not referred
so they can be everything.

When I say **similar x y**, **a** and **b** is suddenly exposed. So at
that relation **a** refer to **x** and **b** refer to **y**, but that is
just within that single relation.

TODO: Inconsistence: Use Time Travel (Treat Time as the Forth
Demination)

Artificial Intelligence and NP Hard Problems
--------------------------------------------

Temporal Logic
-------------------

### Tense Logic

-   P “It has at some time been the case that …”
-   F “It will at some time be the case that …”
-   H “It has always been the case that …”
-   G “It will always be the case that …”

#### Extensions

<strong>The binary temporal operators S and U (“since” and
“until”)</Strong>

-   Spq “q has been true since a time when p was true”
-   Upq “q will be true until a time when p is true”

<strong>Metric tense logic</strong>

-   Fnp “It will be the case the interval n hence that p”

<strong>The “next time” operator O</strong>

-   Discrete sequence of atomic times

### Temporal Arguments

### Hybrid approaches

A statement of the form “True(p, t)”, saying that proposition p is true
at instant t, can then be paraphrased as “□ (t→ p)”, i.e., the
instant-proposition t necessarily implies p.

### State and event-type reification

    Holds(Asleep(Mary), (1pm, 6pm))
    Occurs(Walk-to(John, Station), (1pm, 1.15pm))

### Event-token reification

∃e(See(John, Mary, e) & Place(e, London) & Time(e, Tuesday)), Therefore,
∃e(See(John, Mary, e) & Time(e, Tuesday)).

  [Stanford Encyclopedia of Philosophy: Temporal Logic]: http://plato.stanford.edu/entries/logic-temporal
  [Wikipedia: Comparison of Programming Languages]: https://en.wikipedia.org/wiki/Comparison_of_programming_languages
  [Cosmos: A New Logical Language]: https://github.com/mcsoto/cosmos

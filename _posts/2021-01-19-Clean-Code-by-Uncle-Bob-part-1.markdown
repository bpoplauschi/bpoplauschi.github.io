---
layout: post
title:  "Clean Code - by Uncle Bob - part 1"
date:   2021-01-19 20:00:00 +0300
comments_id: 11
categories: []

---

One of my best sources for learning how to write clean code is the content from Robert C. Martin aka "Uncle Bob". While studying his free "Clean Code - Uncle Bob / Lessons" on YouTube, I took notes and decided to share those notes, so others can benefit from them.

To be clear, all the content belongs to Robert C. Martin, I merely summarised the content and updated the code examples for Swift.

## About Robert C Martin

> **Robert Cecil Martin**, colloquially called "Uncle Bob" is an American [software engineer](https://en.wikipedia.org/wiki/Software_engineer), instructor, and best-selling author. He is most recognised for developing many software design principles and for being a founder of the influential [Agile Manifesto](https://en.wikipedia.org/wiki/Agile_Manifesto).
>
> Five of Martin's principles have become known collectively as the SOLID principles. Though he invented most of the principles he promotes, the Liskov substitution principle was invented by Barbara Liskov, while the open–closed principle was invented by Bertrand Meyer.
>
> Martin is a proponent of software craftsmanship, agile software development, and test-driven development.

He has authored a series of books on Clean Code and Architecture:
- 2002 *Agile Software Development, Principles, Patterns, and Practices*
- 2009 *Clean Code: A Handbook of Agile Software Craftsmanship*. Prentice Hall
- 2011 *The Clean Coder: A Code Of Conduct For Professional Programmers*. Prentice Hall
- 2017 *Clean Architecture: A Craftsman's Guide to Software Structure and Design*. Prentice Hall
- 2019 *Clean Agile: Back to Basics*

## Quotes

To better explain what the concept of Clean Code is, Uncle Bob starts with a few quotes.

> Clean code is simple and direct. Clean code reads like well-written prose. Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control. - Grady Booch author of Object Oriented Analysis and Design with Applications

> Clean code always looks like it was written by someone who cares - Michael Feathers, author of Working Effectively with Legacy Code

> You know you are working on clean code when each routine you read turns out to be pretty much what you expected ... - Ward Cunningham, coauthor of the Manifesto For Agile Software Development

> Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write. - Robert C. Martin

## Ideal size of functions

#### Q: How small should a function be?
The old rule was: _is should fit on your screen_.
A better answer is: __It should do one thing__.

> A function does one thing if you can't meaningfully extract another function from it.

Uncle Bob recommends we extract and extract using public functions that call private functions that call private functions and so on, leading to functions that do one thing, are very small and very easy to read.
Ideally, those should have no indentation, but it's ok to have one or two levels of indentation (by indentation he means statements that require code indentation like `if else`,  `while`, `do`, `try / catch`, ...).

#### Q: How many arguments should a function have?
- 0 is ideal
- 1 argument - not too hard to understand
- 2 arguments - still ok
- a function with 3 arguments is a little hard to read, since there are 6 ways to order the 3 args
- 4 - it's getting hard (24 ways)
- 5...

#### Bool arguments for 2 behaviours
Don't pass booleans as arguments to have the function do one thing if that argument is `true` and another if it's `false`. Instead, separate into 2 functions.

## Avoid switch statements

Uncle Bob says that if the compiler doesn't warn about handling all cases, adding new cases can lead to undefined states (this is true for some languages, not Swift).

Also, they end up having many dependencies (aka dependency magnet) because all the cases are handled into a single place, so if each case is handled by a different component, the place containing the `switch` will be heavy coupled with all those components.

In the case of Swift's `switch` statement on enums, the compiler will help us by returning an error when one or more cases are not handled and even generate the code for handling those, so we are in a better case. Of course, this requires not using the `default` case and adding explicitly each case to be handled, otherwise `default` will match any new cases added later.
As for the dependency magnet, we can still have this issue in Swift.

## Functions with side effects vs functions with a return type
__Side effects__ are changes the state of the system.
Functions with side effects usually come in pairs: `open` / `close`, `malloc` / `free`, `init` / `dealloc`, ...

#### Convention by Uncle Bob
A command aka a function that returns void has side effects, otherwise it would do nothing.
A function that returns a value should have no side effects

## Prefer exceptions to error codes
We prefer exceptions to error codes because they are more explicit.
When dealing with `try` / `catch`, we should not add more logic in a function than the `try` / `catch` block, so that function does one thing: handle errors.
Recommendation: don't use nested `try` / `catch`.

## DRY = Don't Repeat Yourself
Avoid code duplication, in some cases using lamdas (clojures in Swift) where we can't do it otherwise.

## Proving algorithms are correct

Dijkstra worked on mathematically proving software is correct. He was able to do it for sequencing, selection (if-else) and iteration.
Any algorithm can be expressed using those 3 building blocks.
He could not prove algorithms are correct if they use goto-s.

## Code reviews

Q: How long should it take to review code that took 5 hours to write?
R: Probably a function of 5 hours (maybe less, but with a similar order of magnitude) - because you need to walk through the same reasoning.

> if you review a module that took 5 hours to write in 15 minutes, all you did was look for semicolons :)

So if you're gonna spend that time code reviewing, why not pair program?
That way you get involved, you share knowledge and contribute to the team.

## GoF book

Uncle Bob strongly recommends we read [Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, John Vlissides, Richard Helm, Ralph Johnson](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612).
> probably the most important book written in the last 30 years

## Sources

- [Clean Code - Uncle Bob / Lesson 1](https://www.youtube.com/watch?v=7EmboKQH8lM)

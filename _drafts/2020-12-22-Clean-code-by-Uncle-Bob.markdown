---
layout: post
title:  "Clean Code - by Uncle Bob"
date:   2020-12-22 16:00:00 +0300
categories: []


---

> Clean code is simple and direct. Clean code reads like well-written prose. Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control. - Grady Booch author of Object Oriented Analysis and Design with Applications

> Clean code always looks like it was written by someone who cares - Michael Feathers, author of Working Effectively with Legacy Code

> You know you are working on clean code when each routine you read turns out to be pretty much what you expected ... - Ward Cunningham

> Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write. - Robert C Martin



### Ideal size of functions

How small should a function be?

Old rule: fit on your screen

It should do one thing

> A function does one thing if you can't meaningfully extract another function from it.

Tiny functions, ideally with no indentation, allowed one or two levels of indentation



Arguments: 0 - ideal, 1 - not too hard to understand, 2 - it's ok, 3 - a little hard (6  ways to order the 3 args), 4 - it's getting hard (24 ways), so max 3

Don't pass booleans as args, instead separate into 2 functions

### Avoid switch statements

if the compiler doesn't warn about handling all cases, adding new cases can lead to undefined states

Also, they end up having many dependencies (aka dependency magnet) because all the cases are handled into a single place



### Functions with side effects vs functions with a return type

Side effects: changes the state of the system.

Side effect functions come in pairs: open / close, alloc / free, ...

Convention: 

A command aka a function that returns void has side effects, otherwise it would do nothing. 

A function that returns a value should have no side effects



Ideally able to independently deploy modules (especially the ones that change for "dumb reasons" like the UI)



### Prefer exceptions to error codes

separate the try / catch in a dedicated function, so the function does one thing: handle errors

don't use nested try / catch

### DRY

avoid code duplication, in some cases using lamdas where we can't do it otherwise



Dijkstra worked on mathematically proving software is correct. (attribution, selection if-else, loops). Can't prove algorithms are correct if they use gotos

Any algorithm can be expressed using attribution, selection and loop



https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29
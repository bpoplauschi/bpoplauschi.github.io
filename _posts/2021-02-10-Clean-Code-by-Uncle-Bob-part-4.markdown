---
layout: post
title:  "Clean Code - by Uncle Bob - part 4"
date:   2021-02-10 16:00:00 +0300
comments_id: 15
categories: []
tags: [CleanCode]
---

One of my best sources for learning how to write clean code is the content from Robert C. Martin aka "Uncle Bob". While studying his free "Clean Code - Uncle Bob / Lessons" on YouTube, I took notes and decided to share those notes, so others can benefit from them.

To be clear, all the content belongs to Robert C. Martin, I merely summarized the content and updated the code examples for Swift.

## Uncle Bob, your new CTO

In this lesson, Robert Martin guides us through an imaginative exercise where he is the new CTO at a tech company.

He focuses on raising awareness, given the need to increase the level of criteria in code production. Pointing to the lack of preparation in most programmers, as one of the main reasons for the inefficiency in software development today. 

Robert Martin proposes a series of Expectations, through which he hopes to instill in the programmers, the knowledge and desire to prosper towards a way of programming based on ethics and responsibility.

### 1. We will not ship shit

People now regularly download and use betas. :) Beta software means code that probably doesn't work well / as expected. It definitely has bugs. Betas should be used only in limited cases.

Robert Martin's expectation is that we only ship code __we know it works__. It should have the highest quality level you can deliver. It should be clean, tested, well organized. Everybody expects that. 

Example: when you buy a car, you expect it to work, to have been properly tested and you are going to complain bitterly if even the smallest thing doesn't work.

### 2. We will always be ready

Agile = system is in a deployable / shippable state after every iteration / sprint.
The system is ready, even if it doesn't have enough features to deploy (that is a business decision).
Ready to deploy = all the testing is done, all the documentation is done, the set of features is done.
If you are not deployable at the end of the sprint, you are not doing Agile.

Iteration length: ideally 1 week, acceptable 2 weeks.

### 3. Stable Productivity

The longer the project goes, the slower you __don't__ get. You are able to produce features at the same rate even after a long time. Each added feature does not make the system harder and harder to maintain / change. 
The reason we slow down is because we make a mess. As the mess builds, we get slower.

Example from Robert Martin's career: worked for a company where every estimate was 6+ months. The dev team's cost of missing estimates was so high, they would not commit to anything under 6 months. Also, the system was highly coupled and you couldn't even change a menu item's name without affecting other areas.

### 4. Inexpensive Adaptability

The software must be changeable. It should be cheap and easy to make changes to the system.

Put otherwise, the size of the change should be proportional to the scope of the change.

He expects us not to respond to a small client change with: "Oh God, that destroys our entire architecture" :)

> "Software" is a compound word = "soft" (changeable) + "ware" (product). 

Software = changeable product. It was invented so our machines can be easily changeable.
If it's hard to change, you have just reinvented Hardware.

How do you get changeable software?
- you keep it clean
- you better have a really good suite of tests

### 5. Continuous Improvement

The code should be getting better with time (even if most of the time, the code rots.)
The architecture of the system should be getting better with time.

### 6. Fearless Competence

If your reaction to bad code is "if I change it, I will have to own it", then that code will end up rotting.
You will change it from time to time, but with minimal changes to reduce the risk. You will not improve the system. No one will ever clean it.

How do you get fearless competence? With tests (you can run quickly and believe their outcome).
You could make small improvements, run the tests and make sure the state is still green, then check it in, making the system a little better than before. It will be trivial (if the tests can be trusted).

Conquer the fear with tests: First write unit tests for the code you have written, then for code other people has wrote.

Fragile test problem is when the tests are so coupled with the production code than changing anything in the production makes a bunch of tests fail. This usually happen to beginners with unit testing. Tests must be designed, just like other components.
Even outside tests, if a change in one component leads to many things breaking in others, that is just a bad design, if it's tests or not.

Code coverage - there is no target number other than 100% that makes sense. Otherwise, you are ok with x% of the code just not working. Also don't let managers see that number :) The team understands that this is just the number of lines of code being tested. If management adds this metric, developers start using less asserts and just increase coverage in a fake way.

### 7. We will not dump on QA

QA is not the tool we use to find bugs in our system. QA should find nothing.
The responsibility of making sure the system works belongs to developers.

Manual QA is a less than ideal process, also are under a lot of pressure.
QA belongs at the beginning of the process, not at the end.

### 8. Automation!

If a computer can do it, a computer should do it.

When under pressure (time), QA will not be able to run all the tests suite, so they will just execute some. That is really problematic.

### 9. We cover for each other

How do you make sure someone can cover for you when you are not available?

We can do this by pairing. Otherwise we end up with explicit responsibilities that only a person knows how to do.

Recommendation: a few times a week pair for an hour / half an hour.

Pairing is a better way to do code reviews.

### 10. Introduction to Honest Estimates

[PERT estimate system](https://en.wikipedia.org/wiki/Program_evaluation_and_review_technique) - uses 3 numbers instead of one (best case, nominal - usual case, worst case)

## Sources

- [Clean Code - Uncle Bob / Lesson 3](https://www.youtube.com/watch?v=Qjywrq2gM8o)
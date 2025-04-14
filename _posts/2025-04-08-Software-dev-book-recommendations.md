---
layout: post
title:  "Software Development Book Recommendations"
date:   2025-04-08 08:00:00 +0300
categories: []
comments_id: 34
tags: [books]

---

I love reading and learning, so I've compiled a list of books that helped me a lot, some of them were simply transformational.
I preffer to have a clear and proven learning track, so I mostly focus on timeless classics instead of following trends and the most recent publications.

__Disclaimer:__ I recommend __studying__ these books, and __learning__ from them, __not just reading__. This means __reading more than once__, __taking notes__, __trying out__ the concepts in real life, trying to __link the new knowledge to the one you already have__ (to make learning effective).

_Format: I tried summarizing the book in one paragraph, define the book's impact on me and listing my main takeaways._

# [Dependency Injection in .NET by Mark Seeman](https://www.amazon.com/Dependency-Injection-NET-Mark-Seemann/dp/1935182501)
This book offers a thorough explanation of how to __decouple__ software components by __injecting their dependencies at runtime__, enabling more flexible, testable, and maintainable code in applications.

For me, it has transformed the way I design and code apps, deeply understanding layering and dependencies, modularizing (based on the requirements), but also having a proven mechanism (Composition Root) to compose modules into different apps or flavors of the same app.

Takeaways:
- __Manual DI__: Constructor, Property or Method Injection. 
- __Constructor Injection__ is the safest option in compiler-based languages, as the compiler won't allow constructing an object unless its dependencies are provided.
- __Service Locator anti-pattern__ - widely used pattern which leads to high-coupling and runtime errors
- Choosing between __Manual DI__ vs __DI containers__ (the later should not be used in all app layers, but rather in the Composition Root only)
- __Modularity__ and how to __compose an app using modules__ and __a Composition Root__.
- __Composition Root pattern__ = the lowest level and most concrete component, the app entry point
- How to implement __cross-cutting concerns__ with the __Decorator pattern__.

# [Working Effectively with Legacy Code by Michael Feathers](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)
If you’re a developer, you work with __legacy code__. This book highlights the __value of existing, functioning code__ and shows how to __make small adjustments to make it testable and unlock refactoring__.

This book transformed the way I look at code, especially legacy code. From running away from legacy code and being intimidated by such challanges, it flipped the narative and allowed me to see all the opportunities to improve that already functioning code, following these battle-proven techniques and patterns. These skills just make me stand out as an engineer.

Takeaways:
- __Legacy code__ = __code without tests or with an unclear structure, regardeless of age__
- Use the __recipes__ in the book to make __small safe changes that enable testability__
- __Seam__ = place where you can alter behavior without modifying existing code directly - essential for introducing tests and making controlled changes
- Write __Characterization Tests__ for legacy code which enable safe refactorings
- __Sprout Method / Class__ - instead of modifying an existing method or class, you “sprout” a new one that safely holds new functionality
- __Wrap Method / Class__: Wrapper around existing entity to intercept or adjust behavior without changing the original one
- __Breaking Dependencies__ - see techniques for dealing with problematic dependencies (like _singletons_, _static methods_, or _global variables_) so that you can isolate and test the units in question. Often involves introducing interfaces or employing DI.
- __Extract and Override Call__: Move direct dependency call (e.g., to a difficult API) into separate method that you can override in a subclass, avoiding the real dependency in tests.
- __Extract Interface__: Split an existing class into an interface and an implementation; the interface can then be mocked or substituted as needed.
- __Parameterize Constructor or Method__: Let the constructor / method accept external dependencies (instead of hard-coding them)
- __Pull Up / Push Down Feature__: Move a field or method from derived classes into a base class when it is shared logic; move the other way to isolate specialized behavior
- __Extract Method__: Pull out piece of logic from a larger method into a separate one

# [Domain-Driven Design: Tackling Complexity in the Heart of Software by Eric Evans](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software-ebook/dp/B00794TAUG)
This book emphasizes structuring and implementing software around a deep understanding of the business domain, using a Ubiquitous Language and well-defined boundaries to create models that reflect real-world complexity effectively.

Takeaways:
- __Focus on the Core Domain__: Invest time and resources in the core domain (business  strategic advantage) and model it with precision and clarity.
- __Iterative Model Refinement__: Continuously refine the model as new insights emerge, using ongoing collaboration and domain exploration to keep it aligned with real-world needs.
- __Align Design and Implementation__: Ensure that code structure reflects the domain model and that the model is kept in sync with the code - refactor the code to keep up.
- __Ubiquitous Language__
  - A shared language between domain experts and developers.
  - Ensures everyone speaks consistently about the model, preventing misunderstandings.
- __Bounded Context__
  - A boundary within which a particular domain model applies.
  - Helps avoid confusion due to overlapping or duplicated concepts across different parts of the system.
- __Entitie__, __Value Objects__, __Aggregates__
- __Repositories__, __Factories__, __Domain Events__
- __Anti-Corruption Layer__ 

# [Refactoring: Improving the Design of Existing Code by Martin Fowler](https://www.amazon.com/Refactoring-Improving-Existing-Addison-Wesley-Signature/dp/0134757599)
Refactoring is not rewriting; it’s a methodical process of cleaning up code with recipe-like steps. Think of this as the classic “recipe book” for refactoring.

I use it regularly, especially given the IDE I mostly use (Xcode) has very limited automated refactoring capabilities.
The most transformative concept for me was deeply understand what refactoring is and trying to refactor as much as possible ever since, of course, by preserving the code behavior.
As Kent Beck explained it, a developer can wear multiple hats, only one at a time. Changing functionality is done under a different hat than refactoring. 
Making this separation I believe to be making your output so much clearer.

Takeaways:
- __Incremental Improvements__: effective refactoring happens in __small__, __safe__ steps:  
  - make one change at a time
  - run tests
  - and proceed only if the system remains stable.
- __Code Smells__ indicate where to refactor - e.g.  Long Method, Large Class, Primitive Obsession, Switch Statements, Divergent Change, Shotgun Surgery.
- __Automated Testing Is Essential__: Having robust unit and integration tests enables you to refactor with confidence—verifying that changes preserve existing functionality.
- __Refactoring Preserves Behavior__: The goal is to change the structure, not the functionality; if system behavior changes, that’s not a pure refactoring.
- __Refactoring Improves Maintainability__: Well-structured code is easier to understand, modify, and extend, ultimately saving time and effort in the long run.
- __Composition Over Inheritance__: Wherever appropriate, prefer using composition to extend functionality rather than complex class hierarchies; this often reduces coupling.
- __Encapsulate What Varies__:  Identify aspects of the code that might change or expand over time and encapsulate them (e.g., through design patterns like __Strategy__ or __Decorator__).
- __Replace Conditionals with Polymorphism__:  Frequent if/else or switch statements can often be replaced with polymorphic classes or methods to simplify control flow and make code more extensible.
- __Naming Matters__: Consistent and meaningful names for variables, methods, and classes help communicate intent and maintain clarity.
- __Continuous Refactoring Culture__: Treat refactoring as an ongoing, integral part of development rather than a one-time event—this keeps code healthy over its entire lifespan.

# [Clean Code: A Handbook of Agile Software Craftsmanship by Robert Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
Clean Code emphasizes writing readable, maintainable, and elegant code by following core principles, best practices, and disciplined craftsmanship that leads to higher-quality software.

For me, reading Clean Code and Clean Coder was the point in time where I stopped being a "hacker" (in the sense of hacking things together so they work) and moved towards being a professional developer, that follows standards, metrics, rules, that takes pride of their work. This was an identity tranformation.

Takeaways:
- __SOLID principles__: applying all the 5 principles is fundamental to writing maintainable, readable, testable code
- __Readability Is Key__: Code should be optimized for human comprehension first, ensuring that anyone can easily understand and modify it in the future.
- __Small, Focused Functions and Classes__: Methods and classes should do exactly one thing and do it well, which keeps the codebase modular, testable, and easier to maintain.
- __Meaningful Names__: Clear, descriptive naming for variables, functions, and classes significantly reduces confusion and improves overall readability.
- __Continuous Refactoring__: Regularly improving existing code keeps it clean and up to date, preventing “code rot” and complexity from creeping in over time.
- __Emphasis on Testing__: Testing (often through Test-Driven Development) helps catch issues early and ensures the code’s design stays simple, robust, and reliable.
- __Error Handling__: Handle errors gracefully and clearly. Use exceptions for exceptional conditions and provide context in error messages.
- __Code comments__: Favor self-explanatory code over excessive comments. Use comments sparingly to clarify complex logic rather than to explain poorly written code.
- __Formatting and Consistency__: Follow consistent formatting rules to help others (and your future self) navigate the code easily.
- __Boy Scout Rule: Leave the camp cleaner than you found it.__

# [The Clean Coder: A Code of Conduct for Professional Programmers by Robert C. Martin](https://www.amazon.com/Clean-Coder-Conduct-Professional-Programmers/dp/0137081073)

The Clean Coder emphasizes the ethics, mindset, and practices that distinguish professional programmers, focusing on responsibility, discipline, and clear communication to produce high-quality software.

Takeaways:
- __Professionalism as a programmer, Standards and Accountability__
- __Clear communication and Boundaries__
- __Discipline in Coding Practices__: TDD, Refactoring, CI
- __Handling timelines, pressure, estimations__
- __Continuous Learning and Improvement__

# [Clean Architecture: A Craftsman's Guide to Software Structure and Design by Robert C. Martin](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164)

Clean Architecture describes principles and design practices for creating software systems that are resilient to changing requirements, easily testable, and maintainable over their entire lifecycle.

Takeaways:
- __Separation of concerns__
- __SOLID Principles__ in detail
- __Entities__
- __Business rules__
- __Usecases__
- __Frameworks and drivers__
- __Layers and Boundaries__
- __Plug-in Architecture__

# [iOS Unit Testing by Example: XCTest Tips and Techniques Using Swift by Jon Reid](https://www.amazon.com/iOS-Unit-Testing-Example-Techniques/dp/1680506811)
This is a practical guide to writing maintainable and robust iOS applications through effective unit testing and test-driven development (TDD) principles, focusing on both Swift and Objective-C codebases.

Automated testing is such a complex domain and it requires so many other skills to be put to practice. More so TDD. It's one thing to write some unit tests and another to choose between numerous types of tests, write clean concise tests, that exercise the behavior of the code and not its structure so that one can later refactor that code without breaking the tests.
With the help of this book and the iOS Lead Essentials program by Caio and Mike, I was able to take my testing skills to the next level. I became profficient with testing, capable of thinking strategically about testing strategies and their implementation using various types of tests.

Takeaways:
- __TDD encourages Clean Code__
- __Clear test structure__ improves readability: use patterns like __Arrange-Act-Assert (AAA)__ and give tests __descriptive names__
- Using test doubles are essential skills: mocks, stubs, fakes
- Continuous testing boosts confidence: run these locally, while you develop, after every change. Also run them as part of the CI pipelines
- __Well-written, high-coverage__ __tests__ serve as a __safety net for refactoring__, encouraging developers to improve design __without fear__ of breaking existing functionality.

# [The Pragmatic Programmer: Your Journey To Mastery by Andrew Hunt and David Thomas](https://www.amazon.com/Pragmatic-Programmer-journey-mastery-Anniversary/dp/0135957052)
The Pragmatic Programmer provides practical guidance and a mindset for software developers to continually refine their craft, create robust solutions, and take ownership of all aspects of their code.

This book may seem trivial in some ways, but it has a subtle intelligence to it. It highlights an art of being balanced, pragmatic, of choosing wisely between quick and dirty and clean ellaborate solutions. It highlights the need to be focused so you can choose from all the tools at your disposal. And it does a great job at showcasing many of these tools that were here many years ago when it was first published.

Takeaways:
- Embrace __continuous learning__
- Adopt a __pragmatic mindset__ over dogmatic rules
- Focus on __clean design__ and __DRY__: minimize duplication, promote reusable components, and favor simplicity to reduce complexity
- Take __ownership__ and __responsibility__: Take pride in your work, own the quality of your code, and be proactive in identifying and fixing potential problems or misunderstandings.
- Refactor early and often
- __Orthogonality__ and __decoupling__: Strive for independent components whose functionality does not overlap, making your system more adaptable, maintainable, and testable
- __Tracer Bullets__: Implement thin, end-to-end solutions to test architecture and feasibility early, guiding further development through continuous feedback
- __Prototyping__: Build quick, disposable implementations to explore ideas, verify assumptions, or confirm feasibility without committing to production-quality code right away
- __Design by Contract__: Define clear, enforceable contracts for functions, modules, or services (preconditions, postconditions, and invariants) to ensure correct behavior.
- __Automation__: Invest in tools that automate repetitive or error-prone tasks (testing, builds, deployment) so you can focus on high-value work.
- __Coding Conventions and Standards__: Maintain consistent naming, formatting, and architectural decisions to improve team collaboration and readability.
- __Collaboration and Communication__: Write clear documentation, engage in effective code reviews, and communicate openly with team members to address issues early and improve collective knowledge.

# [Extreme Programming Explained: Embrace Change by Kent Beck](https://www.amazon.com/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

Consider this the field manual on Extreme Programming (XP) and Test Driven Development (TDD).
XP is a lightweight, people-centric methodology that emphasizes rapid feedback, short development cycles, close collaboration, and continuous improvement to deliver high-quality software in the face of changing requirements.

Takeaways:
- __Core XP Values: Communication, Simplicity, Feedback, Courage, Respect__
- __TDD__: Writing automated tests before coding to guide design and validate functionality. Uses a Red - Green - Refactor cycle.
- __Pair Programming__: Two developers share one workstation, continuously reviewing and improving each other’s work. Produces quality design, less defects, reduces need to use the classical code review workflow.
- __Continuous Integration__: Frequent merging of code changes to identify integration problems early.
- __Refactoring__: Systematic improvement of code structure without altering functionality.
- __Simple Design__: Always aim for the simplest system that works, deferring complexity until necessary.
- __On-Site Customer__: Having a real customer or representative available for immediate feedback and clarification.
- __Collective Code Ownership__: Any team member can modify any part of the code at any time.
- __Whole Team__: All roles (developers, testers, domain experts) collaborate continuously.
- __Sustainable Pace__: Avoid burnout by maintaining a healthy, consistent work pace (e.g., 40-hour work week).

# Feedback

What are the books that have changed the way you work, the way you think?

What book from this list are you planning to read next? Why?

Let me know in the comments section. I'd be happy do discuss and learn.
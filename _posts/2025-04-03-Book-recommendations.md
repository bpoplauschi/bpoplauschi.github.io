---
layout: post
title:  "Book Recommendations"
date:   2025-04-03 08:00:00 +0300
categories: []
comments_id: 34
tags: [books]

---

I love reading and learning, so I've compiled a list of books that I really enjoyed and helped me grow.

__Disclaimer:__ I recommend __studying__ these books, and __learning__ from them, __not just reading__. This means __reading more than once__, __taking notes__, __trying out__ the concepts in real life, trying to __link the new knowledge to the one you already have__ (to make learning effective).

# Software Development and iOS Development

### [Dependency Injection in .NET by Mark Seeman](https://www.amazon.com/Dependency-Injection-NET-Mark-Seemann/dp/1935182501)
Mark Seemann’s Dependency Injection offers a thorough explanation of how to __decouple__ software components by __injecting their dependencies at runtime__, enabling more flexible, testable, and maintainable code in applications.

Takeaways:
- __Manual DI__: Constructor, Property or Method Injection. __Constructor Injection__ is the safest option in compiler-based languages, as the compiler won't allow constructing an object unless its dependencies are provided.
- __Service Locator anti-pattern__ - this is a widely used pattern which leads to high-coupling and runtime errors
- Choosing between __Manual DI__ vs __DI containers__ (the later should not be used in all app layers, but rather in the Composition Root only)
- __Modularity__ and how to __compose an app using modules__ and __a Composition Root__.
- __Composition Root pattern__ = the lowest level and most concrete component, the app entry point
- How to implement __cross-cutting concerns__ with the __Decorator pattern__.

### [Working Effectively with Legacy Code by Michael Feathers](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)
If you’re a developer, you work with __legacy code__. This book highlights the __value of existing, functioning code__ and shows how to __make small adjustments to make it testable and unlock refactoring__.

Takeaways:
- __Legacy code__ = __code without tests or with an unclear structure, regardeless of age__
- Use the __recipes__ in the book to make __small safe changes that enable testability__
- __Seams__ = places where you can alter behavior without modifying existing code directly - essential for introducing tests and making controlled changes
- Write __Characterization Tests__ for legacy code which enable safe refactorings
- __Breaking Dependencies__ - see techniques for dealing with problematic dependencies (like _singletons_, _static methods_, or _global variables_) so that you can isolate and test the units in question. Often involves introducing interfaces or employing DI.
- __Sprout Method / Class__ - instead of modifying an existing method or class heavily, you “sprout” a new one that safely holds new functionality

### [Refactoring: Improving the Design of Existing Code by Martin Fowler](https://www.amazon.com/Refactoring-Improving-Existing-Addison-Wesley-Signature/dp/0134757599)
Refactoring is not rewriting; it’s a methodical process of cleaning up code with recipe-like steps. Think of this as the classic “recipe book” for refactoring.

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

### [Clean Code: A Handbook of Agile Software Craftsmanship by Robert Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
Clean Code by Uncle Bob Martin emphasizes writing readable, maintainable, and elegant code by following core principles, best practices, and disciplined craftsmanship that leads to higher-quality software.

Takeaways:
- __Readability Is Key__: Code should be optimized for human comprehension first, ensuring that anyone can easily understand and modify it in the future.
- __Small, Focused Functions and Classes__: Methods and classes should do exactly one thing and do it well, which keeps the codebase modular, testable, and easier to maintain.
- __Meaningful Names__: Clear, descriptive naming for variables, functions, and classes significantly reduces confusion and improves overall readability.
- __Continuous Refactoring__: Regularly improving existing code keeps it clean and up to date, preventing “code rot” and complexity from creeping in over time.
- __Emphasis on Testing__: Testing (often through Test-Driven Development) helps catch issues early and ensures the code’s design stays simple, robust, and reliable.
- __SOLID principles__
- __Error Handling__: Handle errors gracefully and clearly. Use exceptions for exceptional conditions and provide context in error messages.
- __Code comments__: Favor self-explanatory code over excessive comments. Use comments sparingly to clarify complex logic rather than to explain poorly written code.
- __Formatting and Consistency__: Follow consistent formatting rules to help others (and your future self) navigate the code easily.
- __Boy Scout Rule: Leave the camp cleaner than you found it.__

### [iOS Unit Testing by Example: XCTest Tips and Techniques Using Swift by Jon Reid](https://www.amazon.com/iOS-Unit-Testing-Example-Techniques/dp/1680506811)
This is a practical guide to writing maintainable and robust iOS applications through effective unit testing and test-driven development (TDD) principles, focusing on both Swift and Objective-C codebases.

Takeaways:
- __TDD encourages Clean Code__
- __Clear test structure__ improves readability: use patterns like __Arrange-Act-Assert (AAA)__ and give tests __descriptive names__
- Using test doubles are essential skills: mocks, stubs, fakes
- Continuous testing boosts confidence: run these locally, while you develop, after every change. Also run them as part of the CI pipelines
- __Well-written, high-coverage__ __tests__ serve as a __safety net for refactoring__, encouraging developers to improve design __without fear__ of breaking existing functionality.

### [The Pragmatic Programmer: Your Journey To Mastery by Andrew Hunt and David Thomas](https://www.amazon.com/Pragmatic-Programmer-journey-mastery-Anniversary/dp/0135957052)
The Pragmatic Programmer provides practical guidance and a mindset for software developers to continually refine their craft, create robust solutions, and take ownership of all aspects of their code.

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

# Leadership

### [Extreme Ownership by Jocko Willink]()
Demonstrates why taking total responsibility is the cornerstone of effective leadership (and personal growth). A fun read if you enjoy military stories.

### [The Dichotomy of Leadership by Jocko Willink]()
A follow-up to Extreme Ownership, in the same engaging style. The key message: find balance in all things.

### [Hal Moore on Leadership by Harold Moore]()
Translates battlefield lessons into practical insights on inspiring and guiding others. My main takeaway: Colonel Hal Moore used to go on a long run before making a tough decision to gain clarity.

### [Leaders Eat Last by Simon Sinek]()
Explores how creating a culture of trust and support leads to stronger, more cohesive teams.

# Personal Growth

### [Atomic Habits by James Clear]()
Offers practical steps for developing small, consistent habits that grow into significant results. A cornerstone read for high performance.

### [Indistractable by Nir Eyal]()
In a world filled with distractions, this is a practical guide for regaining focus and clarity.
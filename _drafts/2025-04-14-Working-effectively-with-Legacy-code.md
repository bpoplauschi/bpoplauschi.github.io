---
layout: post
title:  "Leadership and Personal Growth Book Recommendations"
date:   2025-04-08 08:00:00 +0300
categories: []
comments_id: 35
tags: [books]

---

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

Dependency breaking techniques:
- __Adapt Parameter__: Change or wrap a parameter’s type (e.g., from a proprietary object to a simpler interface or vice versa).
- __Break out Method Object__: When a method becomes unwieldy, transform its functionality into a separate class (a “method object”) with clear responsibilities.
- __Definition Completion__: In languages like C++, fully define a class or function that was only forward-declared or partially declared.
- __Encapsulate Global Reference__: Hide direct uses of global variables (or singletons) behind a function or class, enabling replacement or injection during testing.
- __Expose Static Method__: Convert or expose a private or hidden static function as public (or move it out into a test-accessible area), letting you call or override it in tests.
- __Extract and Override Call__: Move direct dependency call (e.g., to a difficult API) into separate method that you can override in a subclass, avoiding the real dependency in tests.
- __Extract and Override Factory Method__: If an object is creating its own dependencies (newing objects internally), pull that creation logic into a separate factory method—then override the factory for testing.
- __Extract and Override Getter__: Take a direct data access (especially to a global or static) and move it into a getter method that can be overridden or mocked in a subclass during tests.
- __Extract Implementer__: Move key implementation details out of a bloated class into a separate “implementer” class; you can then swap or test that implementer in isolation.
- __Extract Interface__: Split an existing class into an interface and an implementation; the interface can then be mocked or substituted as needed.
- __Introduce Instance Delegator__: Replace static/global calls with an instance that delegates functionality
- __Introduce Static Setter__: Provide a setter for static references (or singletons)
- __Link Substitution__: Use your language or linker options to choose an alternative implementation or library at compile/link time without changing the original code.
- __Parameterize Constructor__: Let the constructor accept external dependencies (instead of hard-coding them)
- __Parameterize Method__: Pass in external dependencies or context as parameters on a method-by-method basis
- __Primitivize Parameter__: Replace an object parameter with a simpler or more fundamental type (like int, string, or enum) to reduce coupling and make the code easier to test with basic inputs.
- __Pull Up Feature__: Move a field or method from derived classes into a base class when it is shared logic - remove duplication across subclasses, simpify testing
- __Push Down Dependency__: Push a field or method from a base class into a subclass to isolate specialized behavior - makes each subclass more focused and testable.
- __Replace Function with Function Pointer__: In languages that allow function pointers (ex C++), switch a direct function call to a function pointer call so that the pointer can be reassigned in test code.
- __Replace Global Reference with Getter__: Use a getter method to retrieve a global or static object
- __Subclass and Override Method__: Create a test-specific subclass that overrides problematic methods (network calls, file I/O, etc.) to avoid the real side effects in test runs
- __Supersede Instance Variable__: Introduce a new field (or override an existing one in a subclass) that “takes precedence” over the old instance variable, allowing you to inject test-friendly values without completely removing the original variable.
- __Template Redefinition__: In templated C++ code, use specialized or test-specific template definitions to swap out unwanted dependencies or behaviors when running tests.
- __Text Redefinition__: In languages that rely on text-based macros (C/C++ preprocessor directives, for example), redefine or conditionally replace the macro in test code, enabling a seam for controlling behavior at compile time.

Refactorings:
- __Extract Method__: Pull out piece of logic from a larger method into a separate one
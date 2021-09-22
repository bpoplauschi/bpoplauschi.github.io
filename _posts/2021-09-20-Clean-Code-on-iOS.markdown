---
layout: post
title:  "Clean Code on iOS"
date:   2021-09-20 08:00:00 +0300
categories: []
comments_id: 20
tags: [Clean Code]
---

## Why

I have been doing iOS development for many years and I worked on many projects (including some open source). I have seen many codebases become hard to manage and just unpleasant to work with.

A while back, I became passionate about the concepts of Clean Code and Clean Architecture, so I tried to apply them to my projects.

I learned a lot from Robert Martin's books and from the [iOS Lead Essentials program](https://iosacademy.essentialdeveloper.com/p/ios-lead-essentials/).

Why Code Code and Clean Architecture?

- IMO, it’s a fundamental part of building great software
- It’s agnostic of platform, language, technology
- Has a great impact on the lifetime and maintainability of systems

Here's what I learned so far.

## 1. Hard to change codebases

How many times did you see codebases that with time are getting harder and harder to maintain / change?

They come with legacy code (code without tests), many flavours of code, many patterns, multiple languages (Swift, ObjC and more), they have huge files, unused code and resources, and most of the time, have some parts that nobody touches (because the guy that worked on that has left the team and nobody known how to work with that).

It's definitely not profitable for a company to own such a codebase, so the ones that need to be maintained over years usually get to a point where they need to be rebuilt from scratch. 

The problem with that (besides the obvious) is, without changing our principles, the new codebase will soon have the same issues as the old one.

![only-god-knows](/assets/onlyGodknows.jpeg)

## 2. What is Software

> “Software” is a compound word = “soft” (changeable) + “ware” (product).
> The software must be changeable. It should be cheap and easy to make changes to the system.

## 3. Changeable software
How do you get changeable software?
According to Robert Martin:

- you keep it clean
- you better have a really good suite of tests

## 4. Who is Robert C Martin

![Robert-Martin](/assets/RobertMartin.jpg)

**Robert Cecil Martin** colloquially called "Uncle Bob" is an American software engineer, instructor, and best-selling author. He is most recognized for developing many software design principles and for being a founder of the influential [Agile Manifesto](https://en.wikipedia.org/wiki/Agile_Manifesto).

He's had a substantial role in stating the SOLID principles.

He's authored several books, among which The Clean Coder, Clean Code, Clean Architecture.

Robert Martin had a continuous quest for what Clean code is and to establish industry standards for Software Development.

Many of Robert Martin's teachings are applied for iOS in detail in the [iOS Lead Essentials program](https://iosacademy.essentialdeveloper.com/p/ios-lead-essentials/).

## 5. What is clean code

> **Clean code is simple and direct. Clean code reads like well-written prose.** Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control.
> -- Grady Booch author of Object Oriented Analysis and Design with Applications

> **Clean code always looks like it was written by someone who cares.**
> -- Michael Feathers, author of Working Effectively with Legacy Code

> You know you are working on clean code when **each routine you read turns out to be pretty much what you expected** … 
> -- Ward Cunningham, coauthor of the Manifesto For Agile Software Development

> Indeed, **the ratio of time spent reading versus writing is well over 10 to 1**. **We are constantly reading old code as part of the effort to write new code. …[Therefore,] making it easy to read makes it easier to write.** 
> -- Robert C. Martin

![clean-code-image](/assets/clean-code-image.png)

## 6. Principles of clean code

### 6a. The Boy Scouts rule

Leave the campground cleaner than you found it.

With every change (commit), we make the codebase just a bit better: fix a warning, remove a todo or an obsolete comment, add a test, improve a name, split a component that is getting too big, ...

### 6b. [**KISS**](https://en.wikipedia.org/wiki/KISS_principle): Keep It Simple Stupid

A design principle originating from the U.S. Navy that goes back to 1960 already. 

It states that most systems should be kept as simple as possible.

In my opinion, the art of designing code lies in simplicity - the simpler and easier to read / change code is, the better it is designed.

### 6c. [**DRY**](https://en.wikipedia.org/wiki/Don't_repeat_yourself): Don't Repeat Yourself
It states that every piece of code must have a single, unambiguous, authoritative representation within a system.

There's a catch: duplication is usually the enemy, but similar (even identical code) doesn't always mean duplication. We need to also look at the reasons to change.

Example: the representation of the API response (that changes when the API schema changes) and the business model that your core uses (that changes when the business requirements change). Even though they might start as duplicates, it's good to have them separate so we can protect our business rules from API schema changes (otherwise on any change like that, we'll have many changes to apply to other layers of the code).

```swift
// model object representing a feed item
struct FeedItem {
    let title: String
    let createdAt: Date
}

// network response representation of the feed item
struct RemoteFeedItem: Decodable {
    let title: String
    let created_at: String
}
```

### 6d. Keep things small
By things I mean all kinds of entities: functions, classes, interfaces (protocols), enums, structs, clojures, etc.
Small pieces of code are easier to write, maintain and, most importantly, read and understand.
There are guidelines used to increase readability about what those numbers should be: including max number of lines per function, per file, max characters per line and more.

Recommendations compiled by Robert Martin:
- 50 or less lines of code per file (on average)
  - keeping most files under 100 lines
- 30-40 or less characters per line
  - "It's rude to make your readers scroll to the right"
- how small should a function be?
  - It should do one thing
  - like a few lines of code with 1 or a max of 2 levels of indentation
- how many arguments should a function have?
  - Ideally 0
  - 1 and 2 are good
  - from 3 arguments the function becomes a bit harder to understand and it might be doing too much

### 6e. Command-Query Separation

Side effects are changes the state of the system. 

Functions with side effects (commands) usually come in pairs: `open` / `close`, `malloc` / `free`, `init` / `dealloc`, …

A command aka a function that returns void has side effects, otherwise it would do nothing. 
A query aka a function that returns a value should have no side effects.

Don't create functions that return values and have side effects, they are very confusing.

### 6f. Prefer exceptions to error codes or optional returns
We prefer exceptions to error codes because they are more explicit.

When dealing with `do` / `catch`, we should not add more logic in a function than the `do` / `catch` block, so that function does one thing: handle errors. 

Recommendation: don’t use nested `do` / `catch`.

See in the following example where the `init` throws instead of returning an optional `CoreDataFeedStore` (which would say nothing about why the API failed).

```swift
public final class CoreDataFeedStore {
    private static let modelName = "..."
    private static let model: NSManagedObjectModel? = ...
    private let container: NSPersistentContainer
    private let context: NSManagedObjectContext

    enum StoreError: Error {
        case modelNotFound
        case failedToLoadPersistentContainer(Error)
    }

    public init(storeURL: URL) throws {
        guard let model = CoreDataFeedStore.model else {
            throw StoreError.modelNotFound
        }
        
        do {
            container = try NSPersistentContainer.load(name: CoreDataFeedStore.modelName, model: model, url: storeURL)
            context = container.newBackgroundContext()
        } catch {
            throw StoreError.failedToLoadPersistentContainer(error)
        }
    }
}
```

### 6g. Proper use of access control modifiers

Many projects avoid using access control modifiers and just go with the default one (which currently in Swift is `internal`), but properly using access control has its advantages:

- first of all, they help us better express in code. When you see a type having `private`, `internal` and `public` properties / methods, it is very clear which ones were created to be used by outer layers and types and which are implementation details.
- testing our types through their public interface only (no use of `@testable import`) keeps the tests decoupled from the production code, allowing us to refactor the internal and private implementations without breaking the public contract
- it's easier to spot entities that have too many responsibilities by looking at their `public` interface

## 7. Choosing good names
Names are everywhere (files, directories, programs, classes, variables, arguments, …).
Because we do so much of it … we’d better do it well!

It is said that choosing good names is one of the most difficult tasks when writing code. 
Remember this is an iterative process and the names in your projects will evolve with time. 
Remember the Boy Scouts rule: leave the compound a cleaner then you found it.

The rules for naming recommended by Uncle Bob are based on Tim Ottinger’s Rules for Variable and Class Naming:

### 7a. Reveal your intent
- a name that requires a comment does not reveal it's intent: `var d: Int // elapsed time in days`
- the name of a variable should tell us the significance of what that variable contains `var elapsedTimeInDays: Int`

### 7b. Use consistency when naming entities

- it's confusing to see functions with get, retrieve, load, fetch, ...

### 7c. Rule for variable names
"A variable name should be proportional to the size of the scope that contains it."

### 7d. Short scope
- if the scope is very small, like 1 line, a single letter name is fine
```swift
switch (lhs, rhs) {
    case (.failure(l), .failure(r)): return l == r // one line scope, using single letter names
}
```

- this is because the scope is limited, so you don't need the name to remind you of anything, the (name of the) function that generated the value should be enough
- inside an if statement, so maybe a couple of lines of code, the variable names inside should be very short
```swift
if let data = cacheData {
    let cache = try decoder.decode(Cache.self, from: data) // scope limited to the if, using one word names
    completion(cache)
}
```

- inside a while loop, the variables should be very short
- if you have a function (maybe 4 lines long), the variables inside should be pretty short, maybe a bit longer than the previous ones, maybe a word would be good for an argument
- instance variables that live inside a class have a longer scope (the scope of the class) so it should be long-ish: 2 words maybe
```swift
struct FeedItem {
    let name: String
    let imageURL: URL
}
```

- the arguments to a member function probably one word
```swift
func save(_ feed: [FeedItem], completion: @escaping (SaveResult) -> Void)
```

### 7e. Large scope
- long scopes need long names
- global variables have a huge scope, so they should probably be very long
```swift
// I don't think using global variables is a good practice, but as an exercise ...
public let testsDefaultTimeoutInSeconds: Int = 1
```

### 7f. Rule for function names
"The name of a function is inversely proportional to the size of the scope that contains it."
- as the scope gets larger, we want to shrink the name because it will be called a lot, from all over the place. 
- also, if the function has a larger scope, it's probably dealing with a high-level abstraction
- we would not want to call the `open()` function if the name was `openFileAndThrowExceptionIfNotFound()`
- as the scope decreases, the names start to get longer
- the instance methods of a class will probably have slightly bigger names
- private functions called by public functions will have even longer names
- private functions called by private functions will have even longer names
- this rule can apply recursively, as you go deeper the names will get longer
- as you keep extracting functions, those get smaller and more precise, that you need more words to specify

```swift
public func purgeAllData() { ... }

public final class DataManager: DataManagerInterface {
    public func loadObject<T: Codable>(from storageType: DataStorageType, with key: String, completion: @escaping (Result<T, Error>) -> Void) {
        switch storageType {
        case .secure:
            loadObjectFromKeychain(for: key, completion: completion)
        case let .disk(directory):
            loadObjectFromDisk(in: directory, with: key, completion: completion)
        case .memory:
            loadObjectFromMemory(with: key, completion: completion)
        case .userDefaults:
            loadObjectFromUserDefaults(with: key, completion: completion)
        }
    }
  
    private func loadObjectFromKeychain<T: Codable>(for key: String, completion: @escaping (Result<T, Error>) -> Void) {
        guard verifyKeychainStoreIsValid() else { return }
        keychainStorage.retrieve(forKey: key) { result in
            ...
        }
    }
  
    private func verifyKeychainStoreIsValid() -> Bool { ... }
}
```

### 7g. Rule for class names
"The name of a class is inversely proportional to the size of the scope that contains it."
- classes at the global scope have one word names
- derived classes have multiple word names
- inner classes have multiple word names
- as the scope shrinks, the name grows
```swift
protocol Store {}
protocol FeedStore: Store {}
final class CodableFeedStore: FeedStore {}
final class InMemoryFeedStore: FeedStore {}
```

### 7h. Code Example
```java
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int []>();
    for (int[] x : theList)
        if (x[0] == 4)
            list1.add(x);
    return list1;
}
```
- this code isn't complicated
- but it's not explicit. It reveals no intent
- the names do not explicitly reveal the context of the problem being solved

```java
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell : gameBoard)
        if (cell[STATUS_VALUE] == FLAGGED)
            flaggedCells.add(cell);
    return flaggedCells;
}
```
- only the names have changed and the meaning is far clearer
- now we know the list represents the game board and the function gets all the cells from a list that have the status flagged
- a good naming system will tell you more than the context of the function

### 7i. Disambiguate
- What's the difference between the following?
```
XYZControllerForEfficientHandlingOfStrings
XYZControllerForEfficientStorageOfStrings
```
- Would you pick the right one from a code completion list?
- And watch out for symbols that look alike
```java
int a = 1;
if (O == l)
    a = Ol;
else
    l = 01;
```

### 7j. Avoid convenient mispellings
- `klass` vs `aClass` or `theClass`
- avoid situations where the code will break if a spelling error is fixed

### 7k. Number series
- the opposite of intentional naming
- provide no clue into the author's intent
```java
public static void copyChars(char a1[], char a2[]) {
    for (int i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
    }
}
```

```java
public static void copyChars(char source[], char destination[]) {
    for (int i = 0; i < source.length; i++) {
        destination[i] = source[i];
    }
}
```

### 7l. Noise words
- suffixes
```swift
var product: Product
var pd: ProductData
var pi: ProductInfo
```

- data is completly redundant
- what is the difference between `Product`, `ProductData` and `ProductInfo`? the names don't tell the difference
- prefixes
```swift
var aProduct: Product
var theProduct: Product
```

### 7m. Distinguish names meaningfully
- how to know which function to call
```swift
func getActiveAccount() -> Account
func getActiveAccounts() -> [Account]
func getActiveAccountInfo() -> [Account]
```

### 7n. Make sure names are pronounceable
- Imagine you have the variable `genymdhms` (Generation date, year, month, day, hour, minute and second) and you need talk about this variable calling it "gen why emm dee aich emm ess".
- that should be renamed to `generationTimestamp`

## 8. Comments

The purpose of comments is to explain code that cannot explain itself.

Nothing can be quite as helpful as a good comment. 
Nothing can be quite as obscure as a bad comment. 
Comments are not “pure good”.

The proper use of comments is: **to compensate for our failure to express ourselves in code**. 
Every use of a comment represents a failure. So don’t comment *first*. Try everything else, then comment as a *last resort*.

```swift
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) > 0) && (employee.age > 65) {
    // ...
}
```

VS

```swift
if employee.isEligibleForFullBenefits() {
    // ...
}
```

Acceptable comments: copyrights, informative comments, warning of consequences, documentation comments in public APIs

### 8a. TODOs

`TODO`s are a nice IDE feature, but it’s recommended not to check them in, so fix them before, otherwise, they become `DONTDO`s

### 8b. Use explanatory code instead of comments

```java
// does the module from the global list <mod>
// depend on the subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem())) {
    // ...
}
```

extract variables to explain things

```java
ArrayList<Module> moduleDependees = smodule.getDependSubsystems();
Module ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem)) {
    // ...
}
```

### 8c. Commented out code

- few practices are as odious.
- don’t do this!

## 9. References
- [iOS Lead Essentials program](https://iosacademy.essentialdeveloper.com/p/ios-lead-essentials/) - Online program meticulously thought out for iOS developers who want to become world-class senior developers and be part of the highest-paid iOS devs in the world. Focuses on key concepts like Swift, TDD, BDD, DDD, Clean Architecture, Design Patterns, Git, Automation, CI/CD, and Modular Design.
- [The Clean Coder by Robert C. Martin](https://www.goodreads.com/book/show/10284614-the-clean-coder)
- [Clean Code by Robert C. Martin](https://www.goodreads.com/book/show/3735293-clean-code)
- [Test Driven Development: By Example by Kent Beck](https://www.goodreads.com/book/show/387190.Test_Driven_Development)
- [Working Effectively with Legacy Code by Michael C. Feathers](https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code)
- [Design Patterns by Gamma, Johnson, Vlissides, Helm](https://www.goodreads.com/book/show/85009.Design_Patterns)
- [Pro Git by Scott Chacon](https://www.goodreads.com/book/show/6518085-pro-git)

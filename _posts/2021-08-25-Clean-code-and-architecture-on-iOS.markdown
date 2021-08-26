---
layout: post
title:  "Clean Code and Architecture on iOS"
date:   2021-08-25 15:00:00 +0300
categories: []
tags: [Clean Code, SOLID, Clean Architecture]
---

## What is Software

> “Software” is a compound word = “soft” (changeable) + “ware” (product).
> The software must be changeable. It should be cheap and easy to make changes to the system.

## Hard to change codebases
How many times did you see codebases that with time are getting harder and harder to maintain / change?
It's definitely not profitable for a company to own such a codebase, so the ones that need to be maintained over years usually get to a point where they need to be rebuilt from scratch. The problem with that (besides the obvious) is, without changing our principles, the new codebase will soon have the same issues as the old one.

## Changeable software
How do you get changeable software?
- you keep it clean
- you better have a really good suite of tests

## Who is Robert C Martin

**Robert Cecil Martin** colloquially called "Uncle Bob" is an American software engineer, instructor, and best-selling author. He is most recognized for developing many software design principles and for being a founder of the influential [Agile Manifesto](https://en.wikipedia.org/wiki/Agile_Manifesto).

He's had a substantial role in stating the SOLID principles.

He's authored several books, among which The Clean Coder, Clean Code, Clean Architecture.

Robert Martin had a continuous quest for what Clean code is and to establish industry standards for Software Development.

Many of Robert Martin's teachings are applied for iOS in detail in the [iOS Lead Essentials program](https://iosacademy.essentialdeveloper.com/p/ios-lead-essentials/).

## What is clean code

> Clean code is simple and direct. Clean code reads like well-written prose. Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control.
> -- Grady Booch author of Object Oriented Analysis and Design with Applications

> Clean code always looks like it was written by someone who cares.
> -- Michael Feathers, author of Working Effectively with Legacy Code

> You know you are working on clean code when each routine you read turns out to be pretty much what you expected … 
> -- Ward Cunningham, coauthor of the Manifesto For Agile Software Development

> Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. …[Therefore,] making it easy to read makes it easier to write. 
> -- Robert C. Martin

![clean-code-image](/assets/clean-code-image.png)

## Principles of clean code

### The Boy Scouts rule

Leave the campground cleaner than you found it.

### [**KISS**](https://en.wikipedia.org/wiki/KISS_principle): Keep It Simple Stupid

A design principle originating from the U.S. Navy that goes back to 1960 already. It states that most systems should be kept as simple as possible.

In my opinion, the art of designing code lies in simplicity - the simpler and easier to read / change code is, the better it is designed.

### [**DRY**](https://en.wikipedia.org/wiki/Don't_repeat_yourself): Don't Repeat Yourself
It states that every piece of code must have a single, unambiguous, authoritative representation within a system.

There's a catch: duplication is usually the enemy, but similar (even identical code) doesn't always mean duplication. We need to also look at the reasons to change.

Example: the representation of the API response (that changes when the API schema changes) and the business model that your core uses (that changes when the business requirements change). Even though they might start as duplicates, it's good to have them separate so we can protect our business rules from API schema changes (otherwise on any change like that, we'll have many changes to apply to other layers of the code).

```swift
struct FeedItem {
    let title: String
    let createdAt: Date
}

struct RemoteFeedItem: Decodable {
    let title: String
    let created_at: String
}
```

### [**Composition over inheritance**](https://en.wikipedia.org/wiki/Composition_over_inheritance)
Inheritance is the highest form of coupling, so to achieve low coupling, prefer composition. 

This doesn not mean avoid polymorphism (which is a strong programming tool), but rather separate responsibilities into small entities that are composed, also keeping them clean of inherited behavior / interface.

- inheritance is simply a "is a" relationship, while composition can be expressed as a "has a" relationship
- using inheritance results in a slower compilation time (since the compiler needs to generate a virtual table for each class that is not final) and also slower at runtime (as it is more costly to identify which function to call)
- inheritance breaks encapsulation by allowing subclasses to access internal state (details)
- the biggest advantage of composition is it can be resolved at runtime (you can compose your types diferently depending on platform / environment / ...). Inheritance is resolved at compile time.

```swift
// inheritance
class Device {
    let name: String
    let operatingSystem: String
}

class Smartphone: Device {}
class Computer: Device {}

// composition
protocol SystemProtocol {
    var operatingSystem: String { get }
}
protocol Nameable {
    var name: String { get }
}

struct Smartphone: Nameable, SystemProtocol {
    var name: String
    var operatingSystem: String
}
struct Camera: Nameable {
    var name: String
}
```

### Keep things small
By things I mean all kinds of entities: functions, classes, interfaces (protocols), enums, structs, clojures, etc.
Small pieces of code are easier to write, maintain and, most importantly, read and understand.
There are guidelines used to increase readability about what those numbers should be: including max number of lines per function, per file, max characters per line and more.

Recommendations compiled by Robert Martin:
- 50 or less lines of code per file (on average) -> keeping most files under 100 lines
- 30-40 or less characters per line -> "It's rude to make your readers scroll to the right"
- how small should a function be? It should do one thing (like a few lines of code with 1 or a max of 2 levels of indentation)
- how many arguments should a function have? Ideally 0, 1 and 2 are good, from 3 arguments the function becomes a bit harder to understand and it might be doing too much

### Choose good names
Names are everywhere (files, directories, programs, classes, variables, arguments, …).
Because we do so much of it … we’d better do it well!

It is said that choosing good names is one of the most difficult tasks when writing code. Remember this is an iterative process and the names in your projects will evolve with time. Remember the Boyscout rule: leave the compound a cleaner then you found it.

The rules for naming recommended by Uncle Bob are based on Tim Ottinger’s Rules for Variable and Class Naming:

#### Reveal your intent
- a name that requires a comment does not reveal it's intent: `var d: Int // elapsed time in days`
- the name of a variable should tell us the significance of what that variable contains `var elapsedTimeInDays: Int`

#### Use consistency when naming entities

- it's confusing to see functions with get, retrieve, load, fetch, ...

#### Rule for variable names
"A variable name should be proportional to the size of the scope that contains it."

#### Short scope
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

#### Large scope
- long scopes need long names
- global variables have a huge scope, so they should probably be very long
```swift
// I don't think using global variables is a good practice, but as an exercise ...
public let testsDefaultTimeoutInSeconds: Int = 1
```

#### Rule for function names
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

#### Rule for class names
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

#### Code Example
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

#### Disambiguate
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

#### Avoid convenient mispellings
- `klass` vs `aClass` or `theClass`
- avoid situations where the code will break if a spelling error is fixed

#### Number series
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

#### Noise words
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

#### Distinguish names meaningfully
- how to know which function to call
```swift
func getActiveAccount() -> Account
func getActiveAccounts() -> [Account]
func getActiveAccountInfo() -> [Account]
```

#### Make sure names are pronounceable
- Imagine you have the variable `genymdhms` (Generation date, year, month, day, hour, minute and second) and imagine a conversation where you need talk about this variable calling it "gen why emm dee aich emm ess".
- that should be renamed to `generationTimestamp`

### Comments

The purpose of comments is to explain code that cannot explain itself.

Nothing can be quite as helpful as a good comment. Nothing can be quite as obscure as a bad comment. Comments are not “pure good”.

The proper use of comments is: **to compensate for our failure to express ourselves in code**. Every use of a comment represents a failure. So don’t comment *first*. Try everything else, then comment as a *last resort*.

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

#### TODOs

`TODO`s are a nice IDE feature, but it’s recommended not to check them in, so fix them before, otherwise, they become `DONTDO`s

#### Use explanatory code instead of comments

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

#### Commented out code

- few practices are as odious.
- don’t do this!

### Command Query Separation

Side effects are changes the state of the system. Functions with side effects (commands) usually come in pairs: `open` / `close`, `malloc` / `free`, `init` / `dealloc`, …

A command aka a function that returns void has side effects, otherwise it would do nothing. 
A query aka a function that returns a value should have no side effects.

### Prefer exceptions to error codes or optional returns
We prefer exceptions to error codes because they are more explicit.

When dealing with `do` / `catch`, we should not add more logic in a function than the `do` / `catch` block, so that function does one thing: handle errors. Recommendation: don’t use nested `do` / `catch`.

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

### Proper use of access control modifiers

Many projects avoid using access control modifiers and just go with the default one (which currently in Swift is `internal`), but properly using access control has its advantages:

- first of all, they help us better express in code. When you see a type having `private`, `internal` and `public` properties / methods, it's very clear which ones were created to be used by outer layers and types and which are implementation details.
- testing our types through their public interface only (no use of `@testable import`) keeps the tests decoupled from the production code, allowing us to refactor the internal and private implementations without breaking the public contract
- it's easier to spot entities that have too many responsibilities by looking at their `public` interface

## What is Clean architecture

There are different variations of architectures that are considered clean, but they all have the following characteristics:

- Independent of frameworks
  
  > The architecture does not depend on the existence of some library of feature-laden software. This allows you to use such frameworks as toosl, rather than forcing you to cram your system into their limited constraints.
  
- Testable
  
  > The business rules can be tested without the UI, database, web server, or any other external element.
  
- Independent of UI

  > The UI can change easily, without changing the rest of the system. A web UI could be replaced with a console UI, for example, without changing the business rules.

- Independent of the database

  > You can swap out Oracle or SQL Server for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.

- Independent of any external agency

  > In fact, your business rules don't know anything at all about the interfaces to the outside world.

![clean-architecture](/assets/clean-architecture.jpeg)

## SOLID Principles

### Single Responsibility Principle
> A module should have one, and only one, reason to change.

can be rephrased as

> A module should be responsible to one, and only one, user or stakeholder.

The simplest definition of a module is a source file.

SRP is different than the lower level principle of refactoring large functions into smaller ones (aka a function should do one thing).

The best way to understand this principle is by lookign at the symptoms of violating it.

#### Symptom 1 - Accidental Duplication

Let's imagine a `Employee` class with 3 methods: `calculatePay`, `reportHours` and `save`.
This class violates the SRP because those 3 methods are responsible for 3 very different actors:

- `calculatePay` is specified by the accounting department, which reports to the CFO
- `reportHours` is specified and used by the HR department, which reports to the COO
- `save` is specified by the DBAs, who report to the CTO

By putting the source code for these three methods into a single `Employee` class, the developers have coupled each of these actores to the others. This coupling can cause the actions of the CFO's team to affect something that the COO's team depends on.
For example, suppose the `calculatePay` function and the `reportHours` function share a common algorithm for calculating the non-overtime hours. The developers will naturally extract that common algorithm into a function `regularHours` called by both `calculatePay` and `reportHours`. When one team (for example the CFO's team) needs to change the `regularHours` algorithm, they will affect (and possibly break) the reports the COO's team is generating.

#### Symptom 2 - Merges

Merges usually happen in source files that contain many different methods, especially likely if they are responsible for different actors.
For example, the CTO's team of DBAs wanting to change the schema of the `Employee` table and the COO's team of HR clerks want to change the hours report formatting. This means at least 2 developers will make changes to the same file at the same time, probably resulting in a conflict that requires a merge.

Merges are risky affairs, even if we now have tools that handle some cases, we will still end-up with having to resolve conflicts. Unless we really understand what the other team changes are, there's a good chance we won't resolve the conflicts properly and introduce issues.

#### Solutions

There are many different solutions to this problem. Each moves the functions into different classes.

The simplest and most obvious one is to separate the data from the functions. So we create the `Employee Data`, `PayCalculator`, `HourReporter` and `EmployeeSaver` classes, now the last 3 classes that have encapsuled different pieces of logic are separated from each other.

The downside is that now developers need to instantiate and track all 3 classes. A common solution for this dilemma is to use the `Facade` pattern by creating an `EmployeeFacade` that will act as a proxy to each of the dedicated classes.

### Open / Closed Principle
> A software artifact should be open for extension but closed for modification.

OCP is interconnected with SRP and DIP. When we make sure each class has a single responsibility (SRP) and we make it depend on abstractions instead of concrete types (DIP), OCP quickly follows. Now it's easy to create new implementations of the abstractions and inject them without having to change the initial class.

Ex: enums require changing the client code every time we add / remove a case, so they break OCP.

Example: `FeedViewController` depends on an abstraction `FeedLoader`. Initially during development we can just create a `FakeFeedLoader` that directly returns some data, later on we can add `RemoteFeedLoader`, `LocalFeedLoader`, ... or even combinations of them. But the behavior of the `FeedViewController` class remains the same, but is open for extension.

### Liskov Substitution Principle
> Derived classes must be substitutable for their base classes. 

The program should function without issues when using any of the `FeedLoader` implementations above.

### Interface Segregation Principle
> Make fine grained interfaces that are client specific.

A common violation of ISP is creating interfaces with many functions, some optional or unneeded by their clients.

One reason for wanting segregated interfaces is to avoid source code dependencies that are not needed (thus requiring more time to compile and the need to redeploy a bunch of modules on a very small change to just one of them).

Example splitting a 3 methods `GestureProtocol` into 3 individual ones, thus allowing clients to conform to 1, 2 or all those protocols without having to add empty method implementations. Also, a class like `MyButton` can have different properties for each action type (i.e. a `tapHandler` property of type `TapProtocol` and a `doubleTapHandler` property of type `DoubleTapProtocol`). We can implement those protocols in multiple classes or just one, but `MyButton` is agnostic of those details.

```swift
protocol GestureProtocol {
    func didTap()
    func didDoubleTap()
    func didLongPress()
}

// ----->

protocol TapProtocol {
    func didTap()
}
protocol DoubleTapProtocol {
    func didDoubleTap()
}
protocol LongPressProtocol {
    func didLongPress()
}
```

Apple's frameworks often break ISP by having protocols like `UITableViewDataSource` or `UITableViewDelegate` with many methods not needed by most of their clients.

### Dependency Inversion Principle
>  Depend on abstractions, not on concretions.

High-level modules should not depend on low-level modules both should depend on Abstractions. (Abstractions should not depend upon details. Details should depend upon abstractions).

We apply DIP to most of our dependencies, especially volatile ones.
One very useful application of DIP is to protect our business code from (3rd party) frameworks. Instead of making our code depend on a framework, we create an abstraction and depend upon it. Then the framework will conform to that abstraction, thus inverting the dependency (Inversion of Control).

```swift
protocol FeedLoader {
    typealias Items = [FeedItem]
    typealias Result = Swift.Result<Items, Error>
    func loadFeed(@escaping (Result) -> Void)
}

class FeedViewController: UIViewController {
    private let loader: FeedLoader
    
    init(loader: FeedLoader) {
        self.loader = loader
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        ...
        let items = loader.loadFeed()
        ...
    }
}

// URLSession based loader
class RemoteFeedLoader: FeedLoader {
    // ...
}

// or, even an Alamofire based loader
import Alamofire
extension SessionManager: FeedLoader {
    // ...
}
```

## The Main component aka Composition Root

In every system, there is at least one component that creates, coordinates, and oversees the others. We call this the Main Component or, it's also known as the Composition Root pattern.

The Main component is the ultimate detail - the lowest-level policy. It's the initial entry point of the sytem. Nothing, other than the operating system, depends on it.
It's job is to create all the Factories, Strategies, and other global facilities, and then hand control over to the high-level abstract portions of the system.

You can have multiple Main components: one for each configuration, platform or target.

## Frameworks are just details

As the Clean Architecture "onion layers" diagram shows, frameworks live at the outer boundary of the system because they are just details.
Clean systems should not depend on one or more frameworks becase:

- coupling the business code with framework details makes it harder to write, read and test (just think about frameworks that ask you to subclass their classes)
- the frameworks often change with a different agenda than our project's
- if the system is coupled with the frameworks, this change is very hard to do
- they should allow us to switch between one framework to another

Examples:

- you should be able to easily change your data storage from in memory, local file storage, `UserDefaults`, `Keychain`, `CoreData`, `SQLite`, `Realm`, ... or a combination of them
- changing the UI of the app should be easy and require just changes to the UI layer: from `UIKit` to `SwiftUI`or even a simple Command-Line tool without UI
- functional / reactive frameworks (`Combine`, `RxSwift`, ... ) should be interchangeable and decoupled from the business layers - used only in the `CompositionRoot` because of the way they offer out-of-the-box patterns for Adapters / Factories / ...
- same for networking: `URLSession`, `Alamofire`, `AFNetworking`, `Moya`, ...

Postpone the decision about which framework / library to use for as much as possible - so you have as much info as possible. You can use test doubles (mocks or stubs) and you have a low coupling with the actual framework.

## UI layer patterns - a classical iOS debate

**MVC**, **MVVM**, **MVP**, **VIPER** are just design patterns for the UI layer. 
They don't solve the problem of who should handle many other responsibilities like: data access and storage, notifications, navigation and deeplinking, ...
Evem though our community is constantly debating which pattern is best and new ones keep coming out, there's hardly any evidence for it.
Indeed, certain patterns are better matches for specific constraints:

- for example, MVC is better suited for `UIKit` components as they are already built around the `UIViewController` subtypes
-  MVVM seems to fit better when working with `SwiftUI`

### The issue of massive components

But none of those patterns will protect us from the problem of Massive components. Massive View Controller can quickly become Massive View Model or Massive View Presenter. Clean architecture principles help us better separate our code into components and avoid this issue.

## Handling many 3rd party dependencies
It's become very normal that iOS projects have a lot of dependencies, especially 3rd parties. 

- We need a networking component, but not sure what we need from that component? No problem, just add a 3rd party.

- We want to do "analytics"? Sure, add a few more 3rd parties.

- Here's a cool utility belt library - add it!
- Need a draggable table view cell? Of course there's a library for that.

Before you know it, your project has a dozen of those dependencies, most likely through different dependency management systems (manual, git submodules, SPM, CocoaPods, Carthage), it takes a large amount of time to clone the project and build it.

A few problems with this approach:

- you're adding a lot of uncontrollable risks: each library has it's own roadmap, hidden issues, ...
- what do you do when a 3rd party project is abandoned or doesn't fix the most outstanding issues you have?
- having so many dependencies + a lot of them are not designed for a modular and testable integration results in very hard to test code

I recommend you:

- limit the number as much as possible
- postpone the decision
- protect from them using inversion of control, otherwise we end up with high coupling (prod + tests) and it's very hard to maintain over time

## References
- [iOS Lead Essentials program](https://iosacademy.essentialdeveloper.com/p/ios-lead-essentials/) - Online program meticulously thought out for iOS developers who want to become world-class senior developers and be part of the highest-paid iOS devs in the world. Focuses on key concepts like Swift, TDD, BDD, DDD, Clean Architecture, Design Patterns, Git, Automation, CI/CD, and Modular Design.
- [The Clean Coder by Robert C. Martin](https://www.goodreads.com/book/show/10284614-the-clean-coder)
- [Clean Code by Robert C. Martin](https://www.goodreads.com/book/show/3735293-clean-code)
- [Dependency Injection: Principles, Practices, and Patterns by Mark Seemann and Steven van Deursen](https://www.goodreads.com/en/book/show/44416307-dependency-injection-principles-practices-and-patterns)
- [Test Driven Development: By Example by Kent Beck](https://www.goodreads.com/book/show/387190.Test_Driven_Development)
- [Working Effectively with Legacy Code by Michael C. Feathers](https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code)
- [Design Patterns by Gamma, Johnson, Vlissides, Helm](https://www.goodreads.com/book/show/85009.Design_Patterns)
- [Clean Architecture by Robert C. Martin](https://www.goodreads.com/book/show/18043011-clean-architecture)
- [Pro Git by Scott Chacon](https://www.goodreads.com/book/show/6518085-pro-git)

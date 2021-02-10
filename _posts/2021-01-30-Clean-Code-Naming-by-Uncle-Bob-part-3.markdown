---
layout: post
title:  "Clean Code - Naming - by Uncle Bob - part 3"
date:   2021-01-30 14:00:00 +0300
comments_id: 14
categories: []
tags: [CleanCode]
---

One of my best sources for learning how to write clean code is the content from Robert C. Martin aka "Uncle Bob". While studying his free "Clean Code - Uncle Bob / Lessons" on YouTube, I took notes and decided to share those notes, so others can benefit from them.

To be clear, all the content belongs to Robert C. Martin, I merely summarised the content and updated the code examples for Swift.

## Code sizing

Here are a few interesting insights from Robert Martin about the size of the code we write.

### How many lines should there be in a source file

File size is not a function of project size, file size is a style you impose.

Aim for 50 or less lines of code per file (average), keeping most files under 100 lines of code.

### Line lengths

Aim for lines up to 30-40 characters / line.

"It's rude to make your readers scroll to the right".

## Names

Names are everywhere (files, directories, programs, classes, variables, arguments, ...).

Because we do so much of it ... we'd better do it well!

The rules for naming recommended by Uncle Bob are based on [Tim Ottinger's Rules for Variable and Class Naming](https://www.maultech.com/chrislott/resources/cstyle/ottinger-naming.html).

### Reveal your intent

`var d: Int // elapsed time in days`

- a name that requires a comment does not reveal it's intent
- the name of a variable should tell us the significance of what that variable contains

`var elapsedTimeInDays: Int`

### Rule for variable names

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

### Rule for function names

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

### Rule for class names

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

### Code Example

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

### Disambiguate

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

### Avoid convenient mispellings

- `klass` vs `aClass` or `theClass`
- avoid situations where the code will break if a spelling error is fixed

### Number series

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

### Noise words

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

### Distinguish names meaningfully

- how to know which function to call

```swift
func getActiveAccount() -> Account
func getActiveAccounts() -> [Account]
func getActiveAccountInfo() -> [Account]
```

### Make sure names are pronounceable

- Imagine you have the variable `genymdhms` (Generation date, year, month, day, hour, minute and second) and imagine a conversation where you need talk about this variable calling it "gen why emm dee aich emm ess".
- that should be renamed to `generationTimestamp`

## Sources

- [Clean Code - Uncle Bob / Lesson 2](https://www.youtube.com/watch?v=2a_ytyt9sf8)

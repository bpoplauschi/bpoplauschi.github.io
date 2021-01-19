---
layout: post
title:  "Clean Code - by Uncle Bob - part 2"
date:   2021-01-19 20:00:00 +0300
categories: []

---

One of my best sources for learning how to write clean code is the content from Robert C. Martin aka "Uncle Bob". While studying his free "Clean Code - Uncle Bob / Lessons" on YouTube, I took notes and decided to share those notes, so others can benefit from them.

To be clear, all the content belongs to Robert C. Martin, I merely summarised the content and updated the code examples for Swift.

## Code sizing

### How many lines should there be in a source file

File size is not a function of project size, file size is a style you impose.

Preference for 50 lines of code in average per file, most files are less than 100 lines of code.

### Line lengths

Preference for 30-40 characters lines.

"It's rude to make your readers scroll to the right"

## Names

Names are everywhere (files, directories, programs, classes, variables, arguments, ...).

Because we do so much of it ... we'd better do it well!

The rules for naming recommended by Uncle Bob are based of: [Tim Ottinger's Rules for Variable and Class Naming](https://www.maultech.com/chrislott/resources/cstyle/ottinger-naming.html)

### Reveal your intent

`int d; // elapsed time in days`

- a name that requires a comment does not reveal it's intent
- the name of a variable should tell us the significance of what that variable contains

`int elapsedTimeInDays;`

### Rule for variable names

"A variable name should be proportional to the size of the scope that contains it."

#### Short scope

- if the scope is very small, like 1 line, a single letter name is fine
- this is because the scope is limited, so you don't need the name to remind you of anything, the (name of the) function that generated the value should be enough
- inside an if statement, so maybe a couple of lines of code, the variable names inside should be very short
- inside a while loop, the variables should be very short
- if you have a function (maybe 4 lines long), the variables inside should be pretty short, maybe a bit longer than the previous ones, maybe a word would be good for an argument
- instance variables that live inside a class have a longer scope (the scope of the class) so it should be long-ish: 2 words maybe
- the arguments to a member function probably one word

#### Large scope

- long scopes need long names
- global variables have a huge scope, so they should probably be very long

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

### Rule for class names

"The name of a class is inversely proportional to the size of the scope that contains it."

- classes at the global scope have one word names
- derived classes have multiple word names
- inner classes have multiple word names
- as the scope shrinks, the name grows

### Code Example

```
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

```
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell : gameBoard)
        if (cell[STATUS_VALUE] == FLAGGED)
            flaggedCells.add(cell);
    return flaggedCells;
}
```

- only the names have changed and the meaning if far clearer
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

```
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
- provide no clue as the author's intent

```
public static void copyChars(char a1[], char a2[]) {
    for (int i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
    }
}
```

```
public static void copyChars(char source[], char destination[]) {
    for (int i = 0; i < source.length; i++) {
        destination[i] = source[i];
    }
}
```

### Noise words

- suffixes

```
Product product;
ProductData pd;
ProductInfo pi;
```

- data is completly redundant
- what is the difference between `Product`, `ProductData` and `ProductInfo`? the names don't tell the difference
- prefixes

```
Product aProduct;
Product theProduct;
```

### Distinguish names meaningfully

- how to know which function to call

```
Account getActiveAccount();
List<Account> getActiveAccounts();
List<Account> getActiveAccountInfo();
```

### Make sure names are pronounceable

- Imagine you have the variable `genymdhms` (Generation date, year, month, day, hour, minute and second) and imagine a conversation where you need talk about this variable calling it "gen why emm dee aich emm ess".
- that should be renamed to `generationTimestamp`

## Sources

- [Clean Code - Uncle Bob / Lesson 2](https://www.youtube.com/watch?v=2a_ytyt9sf8)

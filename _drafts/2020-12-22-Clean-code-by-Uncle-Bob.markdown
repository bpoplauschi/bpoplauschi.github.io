---
layout: post
title:  "Clean Code - by Uncle Bob"
date:   2020-12-22 16:00:00 +0300
categories: []


---

## TODOS

- [ ] split into small one-topic articles
- [ ] convert examples to Swift
- [ ] add mention on Swift `switch`

## Quotes

> Clean code is simple and direct. Clean code reads like well-written prose. Clean code never obscures the designer’s intent but rather is full of crisp abstractions and straightforward lines of control. - Grady Booch author of Object Oriented Analysis and Design with Applications

> Clean code always looks like it was written by someone who cares - Michael Feathers, author of Working Effectively with Legacy Code

> You know you are working on clean code when each routine you read turns out to be pretty much what you expected ... - Ward Cunningham

> Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write. - Robert C Martin



## Ideal size of functions

How small should a function be?

Old rule: fit on your screen

It should do one thing

> A function does one thing if you can't meaningfully extract another function from it.

Tiny functions, ideally with no indentation, allowed one or two levels of indentation



Arguments: 0 - ideal, 1 - not too hard to understand, 2 - it's ok, 3 - a little hard (6  ways to order the 3 args), 4 - it's getting hard (24 ways), so max 3

Don't pass booleans as args, instead separate into 2 functions

## Avoid switch statements

if the compiler doesn't warn about handling all cases, adding new cases can lead to undefined states

Also, they end up having many dependencies (aka dependency magnet) because all the cases are handled into a single place



## Functions with side effects vs functions with a return type

Side effects: changes the state of the system.

Side effect functions come in pairs: open / close, alloc / free, ...

Convention: 

A command aka a function that returns void has side effects, otherwise it would do nothing. 

A function that returns a value should have no side effects



Ideally able to independently deploy modules (especially the ones that change for "dumb reasons" like the UI)



## Prefer exceptions to error codes

separate the try / catch in a dedicated function, so the function does one thing: handle errors

don't use nested try / catch

## DRY

avoid code duplication, in some cases using lamdas where we can't do it otherwise

## Proving algorithms are correct

Dijkstra worked on mathematically proving software is correct. (attribution, selection if-else, loops). Can't prove algorithms are correct if they use gotos

Any algorithm can be expressed using attribution, selection and loop

https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29

## Comments

The purpose of comments is to explain code that cannot explain itself.

### Schindler's list

Nothing can be quite so helpful as a good comment.
Nothing can be quite so obscure as a bad comment.
Comments are not "pure good".

### Comments are a last resort

The proper use of comments is:
- to compensate for our failure to express ourselves in code.

Every use of a comment represents a failure.
- So don't comment _first_. Try everything else, then comment as a _last resort_.

### Comments lie.

- Not always.
- Not intentionally.
- But they silently rot.
- They migrate.

IDE's usually make them almost hidden.
They get desynchronized from the code and can lead to more confusion.
Delete comments when they are no longer necesarry (code is clear enough).

### Explain yourself in code
```
// Check to see if the employee is eligible for full benefits
if (((employee.flags & HOURLY_FLAG) > 0) && (employee.age > 65)) {
    // ...
}
```
VS
```
if (employee.isEligibleForFullBenefits()) {
    // ...
}
```
Preffer using the name of variables and functions to explain the code.

### Acceptable comments
#### Copyrights

```
//  Created by John McClane on 01/01/2020.
//  Copyright © 2020 Company. All rights reserved.
```

#### Informative comments

- this is useful, but it would be better if the function was renamed to `responderBeingTested`
```
// Returns an instance of the Responder being tested
protected abstract Responder responderInstance();
```
- acceptable because it's using the Singleton design pattern
- a better example of an informative comment
```
// format matched hh:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = pattern.compile("\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*");
```

#### Explanation of intent

```
public int compareTo(Object o) {
    if (o instanceOf WikiPagePath) {
        // ...
    }
    return 1; // we are greater because we are the right type.
}
```
```
// This is our best attempt to get a race condition
// by creating large number of threads.
for (int i = 0; i < 1000; i++) {
    Thread thread = new Thread(widgetBuilderThread);
    try {
        thread.start();
    } catch (OutOfMemoryError e) {
        break;
    }
}
```
#### Warning of consequences

```
// Don't run unless you have some time to kill.
public void _testWithReallyBigFile() throws Exception {
    writeLinesToFile(10000000);
    response.setBody(testFile);
    response.readyToSend(this);
    String responseString = output.toString();
    assertSubString("Content-Length: 1000000000", responseString);
    assertTrue(bytesSent > 1000000000);
}
```
```
public static SimpleDateFormat makeLogFormat() {
    // SimpleDateFormat is not thread safe, so we need to create each instance independently
    return new SimpleDateFormat("dd/MMM/yyyy:HH:mm:ss Z");
}
```
#### Todo comments

- todos are nice, but it's recommended not to check them in, so fix them before

```
private SimpleResponse makeResponseWithXML(Document doc) throws Exception {
    // TODO MdM Should probably use a StreamedResponse
    ByteArrayOutputStream output = new ByteArrayOutputStream();
    // ...
}
```
#### Amplification

```
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting
// spaces that could cause the item to be recognized
// as another list.
```
#### Javadocs in publis APIs

### Bad comments
#### Mumbling

- What does this mean?
- Who loads the defaults?
- Were they previously loaded or is that yet to come?
- Was the author trying to remind himself to do it later?

```
public void loadProperties(String propertiesLocation) {
    try {
        String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
        FileInputStream propertiesStream = new FileInputStream(propertiesPath);
        loadedProperties.load(propertiesStream);
    } catch(IOException e) {
        // No properties files means all defaults are loaded
    }
}
```

#### Redundant comments

- it takes longer to read and understand the comment than it takes to read and understand the code

```
// Utility method that returns when this.closed is true. Throws an exception
// if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
    while (!closed && timeoutMillis > 0) {
        wait(100);
        timeoutMillis -= 100;
    }
    if(!closed) {
        throw new Exception("MockResponseSender could not be closed");
    }
}
```

- redundant comments (from Tomcat)
```
/** The child Containers belonging to this Container, keyed by name. */
protected HashMap children = new HashMap();

/** The processor delay for this component. */
protected int backgroundProcessorDelay = -1;

/** The lifecycle event support for this component. */
protected LifecycleSupport lifecycle = new LifecycleSupport(this);

...
```

#### Mandated comments

- it is just plain silly to have a rule that says that every function must have a javadoc,
- or every variable must have a comment
- Comments like these clutter the code and propagate lies, confusion, and disorganization.

```
/**
 * @param title The title of the CD
 * @param author The author of the CD
 * @param tracks The number of tracks on the CD
 * @param durationInMinutes The duration of the CD in minutes
 */
public void addCD(String title,
                  String author,
                  int tracks,
                  int durationInMinutes) {
    // ...
}
```

#### Journal comments

```
* Changes (from 11-Oct-2001)
* --------------------------
* 11-Oct-2001 : Re-organised the class and moved it to new package com.jrefinery.date (DG);
* 05-Nov-2001 : Added a getDescription() method, and eliminated NotableDate class (DG);
* 12-Nov-2001 : IBD requires setDescription() method, now that NotableDate class is gone (DG); Changed getPreviousDayOfWeek(),
getFollowingDayOfWeek() and getNearestDayOfWeek() to correct bugs (DG);
* 05-Dec-2001 : Fixed bug in SpreadsheetDate class (DG);
```
- this is why we have source code control systems

#### Noise comments

```
/**
 * Default constructor.
 */
protected AnnualDateRule() {
}
```

- These comments aren't just redundant, they are noisy. So we learn to ignore them
- After a while our eyes don't even see them.

#### Scary noise

```
/** The name. */
private String name;

/** The version. */
private String version;

/** The licenseName. */
private String licenseName;

/** The version. */
private String info;
```

- This was taken from a popular open-source framework.
- Clearly noisily redundant.
- Now read it again more carefully.

#### Use explanatory code, not comments

```
// does the module from the global list <mod>
// depend on the subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem())) {
    // ...
}
```

- Extract variables to explain things

```
ArrayList<Module> moduleDependees = smodule.getDependSubsystems();
Module ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem)) {
    // ...
}
```

#### Position markers

```
// ------------------------------ Instance Variables
```

- These are usually pure clutter.
- Banners are startling and obvious _only if_ you don'y see them very often.

#### Closing Brace Comments

```
if ((result < 1) || (result > 12)) {
    for (int i = 0; i < monthNames.length; i++) {
        if (s.equals(shortMonthNames[i])) {
            result = i + 1;
            break;
        } // if
        if (s.equals(monthNames[i])) {
            result = i + 1;
            break;
        } // if
    } // for
} // if
```

- This is a noisy bad habit.
- Shorten your functions instead.

#### Attributions and bylines

```
/* Added by Rick */
```

- Again, _blame_ is what source code controls systems are for.

#### Commented out code

- Few practices are as odious.
- Don't do this!

```
this.bytesPos = writeBytes(pngIdBytes, 0);
//hdrPos = bytePos;
writeHeader();
writeResolution();
//dataPos = bytePos;
...
```

#### HTML in comments

```
/**
* Task to run fit tests.
* This task runs fitnesse tests and publishes the results.
* <p/>
* <pre>
* Usage:
* &lt;taskdef name=&quot;execute-fitnesse-tests&quot;
* classname=&quot;fitnesse.ant.ExecuteFitnesseTestsTask&quot;
* classpathref=&quot;classpath&quot; /&gt;
* OR
* &lt;taskdef classpathref=&quot;classpath&quot;
* resource=&quot;tasks.properties&quot; /&gt;
* <p/>
* &lt;execute-fitnesse-tests
* suitepage=&quot;FitNesse.SuiteAcceptanceTests&quot;
* fitnesseport=&quot;8082&quot;
* resultsdir=&quot;${results.dir}&quot;
* resultshtmlpage=&quot;fit-results.html&quot;
* classpathref=&quot;classpath&quot; /&gt;
* </pre>
*/
```

#### Non-local information

```
/**
 * Port on which fitnesse would run. Defaults to <b>8082</b>.
 */
public void setFitnessePort(int fitnessePort) {
    this.fitnessePort = fitnessePort;
}
```

- If you must write a comment, then make sure it describes the code it appears near.
- Where is the `8082` code?

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

## Code reviews

- how long should it take to review code that took 5 hours to write?
- probably a function of 5 hours (maybe less, but at a similar order of magnitude)
- because you need to walk through the reasoning
> if you review a module that took 5 hours to write in 15 minutes, all you did was look for semicolons :)

- so if you're gonna spend that time code reviewing, why not pair program
- that way you get involved, you share knowledge and contribute to the team

## GoF book

Strong recommendation to read [Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, John Vlissides, Richard Helm, Ralph Johnson](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612) 

> probably the most important book written in the last 30 years

## Sources

- [x] [Clean Code - Uncle Bob / Lesson 1](https://www.youtube.com/watch?v=7EmboKQH8lM)
- [x] [Clean Code - Uncle Bob / Lesson 2](https://www.youtube.com/watch?v=2a_ytyt9sf8)
- [ ] [Clean Code - Uncle Bob / Lesson 3](https://www.youtube.com/watch?v=Qjywrq2gM8o)
- [ ] [Clean Code - Uncle Bob / Lesson 4](https://www.youtube.com/watch?v=58jGpV2Cg50)
- [ ] https://mozekry.github.io/CleanCode/chapter_4.html
- [ ] https://github.com/JuanCrg90/Clean-Code-Notes/blob/master/README.md


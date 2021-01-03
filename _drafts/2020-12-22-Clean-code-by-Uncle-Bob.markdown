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

### GoF book
Strong recommendation to read [Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, John Vlissides, Richard Helm, Ralph Johnson](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612) 

> probably the most important book written in the last 30 years

# Sources

- [x] [Clean Code - Uncle Bob / Lesson 1](https://www.youtube.com/watch?v=7EmboKQH8lM)
- [ ] [Clean Code - Uncle Bob / Lesson 2](https://www.youtube.com/watch?v=2a_ytyt9sf8)
- [ ] [Clean Code - Uncle Bob / Lesson 3](https://www.youtube.com/watch?v=Qjywrq2gM8o)
- [ ] [Clean Code - Uncle Bob / Lesson 4](https://www.youtube.com/watch?v=58jGpV2Cg50)
- [ ] https://mozekry.github.io/CleanCode/chapter_4.html
- [ ] https://github.com/JuanCrg90/Clean-Code-Notes/blob/master/README.md

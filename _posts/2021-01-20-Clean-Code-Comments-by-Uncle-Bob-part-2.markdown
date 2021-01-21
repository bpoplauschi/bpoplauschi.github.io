---
layout: post
title:  "Clean Code - Comments - by Uncle Bob - part 2"
date:   2021-01-20 20:00:00 +0300
comments_id: 12
categories: []

---

One of my best sources for learning how to write clean code is the content from Robert C. Martin aka "Uncle Bob". While studying his free "Clean Code - Uncle Bob / Lessons" on YouTube, I took notes and decided to share those notes, so others can benefit from them.

To be clear, all the content belongs to Robert C. Martin, I merely summarised the content and updated the code examples for Swift.

## Comments

The purpose of comments is to explain code that cannot explain itself.

### Schindler's list

Nothing can be quite as helpful as a good comment.
Nothing can be quite as obscure as a bad comment.
Comments are not "pure good".

### Comments are a last resort

The proper use of comments is: __to compensate for our failure to express ourselves in code__.
Every use of a comment represents a failure.
So don't comment _first_. Try everything else, then comment as a _last resort_.

### Comments lie

- Not always
- Not intentionally
- But they silently rot
- They migrate

IDE's usually display comments in a faded non-evident color, so they are easy to ignore.
They get desynchronized from the code and can lead to more confusion.
Delete comments when they are no longer necessary (code is clear enough).

### Explain yourself in code

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

Prefer using the name of variables and functions to explain the code.

------

### Acceptable comments

#### Copyrights

```swift
//  Created by John McClane on 01/01/2020.
//  Copyright © 2020 Company. All rights reserved.
```

#### Informative comments

```swift
// Returns an instance of the Responder being tested
static func responderInstance() -> Responder
```

- this is useful, but it would be better if the function was renamed to `responderBeingTested`
- acceptable because it's using the Singleton design pattern
- a better example of an informative comment is a comment explaining what a regular expression does (Java)

```java
// format matched hh:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = pattern.compile("\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*");
```

- note: this comment actually lies, since the expression doesn't match only the format in the comment

#### Explanation of intent

```java
public int compareTo(Object o) {
    if (o instanceOf WikiPagePath) {
        // ...
    }
    return 1; // we are greater because we are the right type.
}
```

```java
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

```java
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

```java
public static SimpleDateFormat makeLogFormat() {
    // SimpleDateFormat is not thread safe, so we need to create each instance independently
    return new SimpleDateFormat("dd/MMM/yyyy:HH:mm:ss Z");
}
```

#### TODO comments

- `TODO`s are a nice IDE feature, but it's recommended not to check them in, so fix them before
- otherwise, they become `DONTDO`s

```java
private SimpleResponse makeResponseWithXML(Document doc) throws Exception {
    // TODO MdM Should probably use a StreamedResponse
    ByteArrayOutputStream output = new ByteArrayOutputStream();
    // ...
}
```

#### Amplification

```java
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting
// spaces that could cause the item to be recognized
// as another list.
```

#### Javadocs in publis APIs

Javadocs is just an example, this applies to any documentation format that comes with public APIs (of a library / framework) and is used to generate a documentation set (like HTML).

------

### Bad comments

#### Mumbling

```java
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

- what does this mean?
- who loads the defaults?
- were they previously loaded or is that yet to come?
- was the author trying to remind himself to do it later?

#### Redundant comments

- it takes longer to read and understand the comment than it takes to read and understand the code

```java
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

```java
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
- comments like these clutter the code and propagate lies, confusion, and disorganization.

```java
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

```java
/**
 * Changes (from 11-Oct-2001)
 * --------------------------
 * 11-Oct-2001 : Re-organised the class and moved it to new package com.jrefinery.date (DG);
 * 05-Nov-2001 : Added a getDescription() method, and eliminated NotableDate class (DG);
 * 12-Nov-2001 : IBD requires setDescription() method, now that NotableDate class is gone (DG); Changed getPreviousDayOfWeek(), getFollowingDayOfWeek() and getNearestDayOfWeek() to correct bugs (DG);
 * 05-Dec-2001 : Fixed bug in SpreadsheetDate class (DG);
 */
```

- this is why we have source code control systems

#### Noise comments

```java
/**
 * Default constructor.
 */
protected AnnualDateRule() {
}
```

- these comments aren't just redundant, they are noisy. So we learn to ignore them
- after a while our eyes don't even see them.

#### Scary noise

```java
/** The name. */
private String name;

/** The version. */
private String version;

/** The licenseName. */
private String licenseName;

/** The version. */
private String info;
```

- this was taken from a popular open-source framework.
- clearly noisily redundant.
- now read it again more carefully.

#### Use explanatory code, not comments

```java
// does the module from the global list <mod>
// depend on the subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem())) {
    // ...
}
```

- extract variables to explain things

```java
ArrayList<Module> moduleDependees = smodule.getDependSubsystems();
Module ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem)) {
    // ...
}
```

#### Position markers

```java
// ------------------------------ Instance Variables
```

- these are usually pure clutter.
- banners are startling and obvious _only if_ you don'y see them very often.

#### Closing Brace Comments

```java
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

- this is a noisy bad habit.
- shorten your functions instead.

#### Attributions and bylines

```java
/* Added by Rick */
```

- Again, _blame_ is what source code controls systems are for.

#### Commented out code

- few practices are as odious.
- don't do this!

```java
this.bytesPos = writeBytes(pngIdBytes, 0);
//hdrPos = bytePos;
writeHeader();
writeResolution();
//dataPos = bytePos;
...
```

#### HTML in comments

```java
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

```java
/**
 * Port on which fitnesse would run. Defaults to <b>8082</b>.
 */
public void setFitnessePort(int fitnessePort) {
    this.fitnessePort = fitnessePort;
}
```

- if you must write a comment, then make sure it describes the code it appears near.
- where is the `8082` code?

## Sources

- [Clean Code - Uncle Bob / Lesson 2](https://www.youtube.com/watch?v=2a_ytyt9sf8)

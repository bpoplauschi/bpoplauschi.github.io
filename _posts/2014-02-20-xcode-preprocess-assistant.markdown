---
layout: post
title:  "Xcode preprocess assistant"
date:   2014-02-20 12:20:46.000000000+02:00
categories: 
tags: [assistant, dev, developer, preprocess tool]


---

<img style="float: left; margin: 0px 10px 0px 0px" width="50%" src="/assets/xcode_preprocess_assistant.jpg">

A very cool Xcode feature I can across is **the preprocess assistant**. It basically shows the version of the file you\'re browsing after the preprocessor has expanded all macros.

I know it\'s a feature from Xcode 4, but since I didn\'t see it before, perhaps some of you haven\'t either.

Now, why is this useful?

\- **didactic**: several times I tried explaining to junior developers how the compiler works and how the preprocessor replaces all macros with their actual value, often being represented by some other macros. Best example is `#import` and `#include` directives. I think this tool makes it perfectly clear how does this process work and especially shows the output that without this, you can just picture.

\- **track compiler issues**: if you have a duplicate symbol compiler problem, your macros aren\'t working the way you expected them, using the preprocess assistant takes you closer to the actual issue.

\- **optimise build time and generated code size**: if you want to see how your files actually look like and reduce them (which also reduces the build time), this assistant can be of great help. My 1st impression after using it was: I need to re-think and clean my pch files :)

*Alternative: `Product | Perform Action | Preprocess "YourClassName.m"`*

My old days of CodeWarrior development make me think about what great job has Apple done with their dev tools.

![xcode\_preprocess\_assistant](/assets/xcode_preprocess_assistant.png?)


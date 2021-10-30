---
layout: post
title:  "Understanding Crashlogs and Symbolication"
date:   2021-11-01 08:00:00 +0300
categories: []
tags: []
---

Yeap, investigating crashes like this one are tough, but the satisfaction of understanding and fixing is high ![:slightly_smiling_face:](https://a.slack-edge.com/production-standard-emoji-assets/13.0/apple-medium/1f642.png)
Looking at the crashlog, I see at line 4 what I assume is your app's name or the name of the 3rd party lib you mentioned `linphone` but that line is not symbolicated. Can you try symbolicating the crash? (Let me know if you don't know how to do that, I can give you some extra info).



A couple of things first:

- when building any binary, Xcode has the option to produce a `dSYM` (debug symbols). It's controlled by the Xcode setting **Debug Information Format** (`DEBUG_INFORMATION_FORMAT`). The default value is ``dwarf-with-dsym`` which produces a `dSYM` (but there is a `dwarf` option too).
- the `dSYM` is used when symbolicating a crash log (depending on the setup, an app can have one or more `dSYM` s) to translate addresses into lines of code
- so when building an app (if `dwarf-with-dsym` option is used), you'll see a `MyApp.app.dSYM` folder next to the `MyApp.app` bundle in the `DerivedData/.../Build/Products/...` folder
- if you use any dynamic libraries or frameworks, they will also need separate `dSYM`s. You'll see them in the same folder I mentioned above
- if you use static libraries or frameworks, their symbols are included in your app binary, so the app `dSYM` will contain the symbols for those and you don't need to do anything special
- from what I know, working with Xcode archive and then uploading builds to TestFlight through Xcode deals with uploading the `dSYM`s to Apple.
- you can check the TestFlight build info page / "Build Metadata" section. You should see a `Includes Symbols Yes | Download dSYM` group
- the last important piece of the puzzle is if you use Bitcode or not. This is another Build Setting called **Use Bitcode** (`ENABLE_BITCODE`) and is used to optimize your binaries so that your users can download just a slice of your entire binary which is needed to run the app on their phone.
- Why this is important is because when Bitcode is enabled, Apple has to process your uploaded `dSYM`s and produces new ones (which you can download from the Build info page I mentioned above)
- I haven't worked with TestFlight in a while, so I can't really say how they symbolicate in each situation, but when using 3rd parties (like Crashlytics), I have to download those `dSYM`s after the build is processed and upload them to Crashlytics

Now a few questions for you:

- are you using just TestFlight or do you have any other crash reporting 3rd party?
- how are you linking to `linphone`? I think you said Cocoapods. How is it linked (static vs dynamic)? To answer this, if the Podfile contains just `use_frameworks!` , all dependencies are built as dynamic frameworks. `use_frameworks! :linkage => :static` builds them as static frameworks. If the `use_frameworks!` setting is not there, then all dependencies will be static libraries
- does your app target generate more than 1 `dSYM` in the Build/Products folder?
- do you have Bitcode Enabled for you app? What about the `linphone` library (check the Build Settings for that target)?
- what does the TestFlight build info page say about dSYM?

Nice, you got all the details and fast :)
I was expecting TestFlight to symbolicate automatically, but no prob, you can do it locally. Just look at https://developer.apple.com/documentation/xcode/adding-identifiable-symbol-names-to-a-crash-reportUse the dSYMs you can download from TestFlight - you need the exact one that corresponds to that build that crashed (each build generates unique dSYM) (edited) 



See https://iosleadessentials.slack.com/archives/CT94M0EPP/p1635409924087200
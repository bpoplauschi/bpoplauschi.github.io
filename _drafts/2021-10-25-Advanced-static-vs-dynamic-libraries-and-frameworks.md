---
layout: post
title:  "Advanced Static vs Dynamic libraries and frameworks on iOS (and macOS)"
date:   2021-10-24 08:00:00 +0300
categories: []
comments_id: 0
tags: [static, dynamic, framework, library, linker, iOS, macOS, CocoaPods, Carthage, SPM]

---

When building apps on any platform (applies to Apple platforms too), we have to deal with system frameworks, packaging our own code, using 3rd party code and many more. I've seen quite a few developers that work with static and / or dynamic frameworks / libraries, but don't fully understand them, and thus, can't get the best out of them. So I decided to share my knowledge into this article.

Let's take a quick look at how I structured this info:

- Definitions: what is a library, what is a framework, what is linking, what does static linking and dynamic linking mean (Part 1)
- iOS and macOS differences (Part 1)
- Implications of choosing static vs dynamic on app binary size and launch time
- The most common ways to integrate 3rd party code and how static vs dynamic linking applies to them
- Summary

Before jumping in to the more advanced topics of dynamic vs static linking, make sure you know what's the difference between a library and a framework, between a static framework and a dynamic one, what does the linker do, ...
Read [Part 1 - Introduction to static and dynamic, libraries and frameworks on iOS (and macOS)](../../../2021/10/24/Intro-to-static-and-dynamic-libraries-frameworks.html) before continuing.

## Summary of static vs dynamic linking

| Type of linking and platform / Impact | App Size                                                     | App Start Time                                               | Safety                                                       | Independent deployment                                       |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Static linking - iOS and macOS        | Smaller app size than using dynamic embedded modules (per compiler optimisation).<br />Of course, in the case of dynamically linked modules shared between apps (like on macOS), the app binary size will be the smallest, so compared to that, static linking results in a bigger app binary. | Fastest - loading non-system dynamic frameworks is pretty expensive (system frameworks are optimized). When using static linking, all the symbols are within the same module, so the app start is fast. | All the code linked statically   is checked and copied at build time, so we have the guarantee it works. The client code can't get out of sync with the library APIs. | Everything is delivered in one app binary, so there's no way to deploy just some dependencies / modules. |
| Dynamic linking - iOS                 | Larger binary than when using statically linked modules, since the compiler can't do the optimizations of excluding not used symbols). | This is a known issue: loading dynamic modules is expensive, especially at start time. Apple recommended in WWDC to use a max of 6 dynamic frameworks. See details in ....... | See the line below, keep in mind the dynamic libs / frameworks on iOS are embedded, so they can't really be missing or be set on a different version. | iOS doesn't allow independent deployment of libs / frameworks. |
| Dynamic linking - macOS               | If choosing to share those dynamic modules (via installer) or other way and not embed them in the app binary, the app size will be smaller, otherwise it's the same as above (for iOS). | I didn't find any reference of a limit of dynamic frameworks for macOS apps, probably because Macs have better hardware and this is no longer an issue. But even here, static linking will probably result in a quicker app start. | Since we relly on resolving dependencies at runtime, we add some risks. The dependencies might not exist or, exist, but have a different version than we expect. Both situations could result in a crash. | If modules are installed at a shared location (like the system ones), there's the posibility to deploy and update them independently of the apps using them. |

## When to use dynamic linking

So if statically linked modules result in a smaller app size and are faster at loading, why would we want to use dynamically loaded modules?

Here are a few situations.

### Sharing libraries / frameworks on macOS

When you want to share the same binary (library / framework) between multiple apps, you can install it in a shared location and use it as any other system dynamic module.

### Multiple static modules depending on the same module

Let's say you have a module (your own or 3rd party) named `Common` that is used by other modules in your app.
`FeatureA` -> `Common` and `FeatureB` -> `Common`.
If `FeatureA`, `FeatureB` and `Common` are static libraries / frameworks, you'll see a warning at app runtime in the console `duplicate symbol MY_COMMON_SYMBOL in ... FeatureA and ... FeatureB`. This happens because when linking statically to `Common`, both `FeatureA` and `FeatureB` binaries will contain the symbols from `Common`. So at runtime, the loader will not know which one is should use.

In this case, unless you have other things to consider, making `Common` a dynamic module will solve the problem, as both `FeatureA` and `FeatureB` will just reference `Common`, expecting at runtime to find an implementations for its symbols.

## iOS increased app launch time start when using many dynamic libs / frameworks

I've mentioned using many 3rd party dynamic libraries / frameworks probably leads to an increased app launch time on iOS.

This was detailed in WWDC 2016 Session 406 - Optimizing App Startup Time (I can only find the transcript of that session on [asciiwwdc](https://asciiwwdc.com/2016/sessions/406). Apple explained how each dynamic module adds to the total app launch time and we should keep the number of dynamic modules to a max of 6.
See [Apple's Reducing Your App’s Launch Time article](https://developer.apple.com/documentation/xcode/reducing-your-app-s-launch-time/) that mentiones all of that, except for an exact number of dynamic frameworks (perhaps over time the 6 limit became harder to keep and, since hardwares evolved, the limit might have increased).

The matter is simple: just test it out. Older Xcode versions required adding `DYLD_PRINT_STATISTICS` to the ENV variables to print stats regarding the app launch time and how much each step took. See [Apple's Logging Dynamic Loader Events](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/LoggingDynamicLoaderEvents.html).
Looks like this:

```
Total pre-main time:  95.07 milliseconds (100.0%)
         dylib loading time:  25.00 milliseconds (26.3%)
        rebase/binding time:  19.75 milliseconds (20.7%)
            ObjC setup time:   6.85 milliseconds (7.2%)
           initializer time:  43.45 milliseconds (45.7%)
           slowest intializers :
             libSystem.B.dylib :   8.43 milliseconds (8.8%)
   libBacktraceRecording.dylib :   9.00 milliseconds (9.4%)
    libMainThreadChecker.dylib :  22.05 milliseconds (23.1%)
```

Or you can use Instruments, as explained in [Apple's Reducing Your App’s Launch Time article](https://developer.apple.com/documentation/xcode/reducing-your-app-s-launch-time/).

One thing to consider is something called Cold Start vs Warm Start. A cold start is when you app starts for the 1st time (after a phone reboot) and is usually the slowest start. After that, even if you close the app, the system will cache some of the memory footprint (in case you closed the app by mistake). The next time you start the app will be a faster, warm start.
So to measure reliably, measure your app launch after a phone reboot (cold start).

If you find your app is taking a long time starting (Apple recommended 500 miliseconds for a seamless user experience), take a look at what is holding your app from launching. It can be many things, as executing some code on the `AppDelegate` or `SceneDelegate` methods, having a huge Launch storyboard, ... If the problem lies with `dylib loading time`, you can look into changing some dynamic libraries / frameworks to static ones.

Should you want to dig deeper into this issue, there's a very good thread on a GitHub open source app called Eigen by Artsy: https://github.com/artsy/eigen/issues/586

## Static / dynamic with different integrations techniques

Since nowadays many projects have a lot of 3rd party dependencies, let's look at how these dependency managers work and how we can control their linking related behavior.

### Own targets or projects linked directly

If the targets are in your repo or in other repos, but are linked to your project, you can go in and change the way they are built and linked through Build Settings.

You just need to change the value of the `MACH_O_TYPE` Build Setting between Static Library (`staticlib`) and Dynamic Library (`mh_dylib`).

To change between a library and a framework, you need to change the type of Product the target outputs (not sure how to do that from settings).

<img src="../assets/Mach-O Type.png" style="zoom:80%;" align="left"/>

### CocoaPods

By default, CocoaPods (no mention of `use_frameworks!`) will build and link all the dependencies as static libraries.

If we add `use_frameworks!` to our Podfile, CP will instead build and link the dependencies as dynamic frameworks.

We can use `use_frameworks! :linkage => :static` to make CP build and link dependencies as static frameworks.

See https://guides.cocoapods.org/syntax/podfile.html#use_frameworks_bang.

NOTE: of course, all these apply to dependencies that CocoaPods builds from sources. If you are referencing a pod that is precompiled, there is no way for CocoaPods to change how that pod is packaged. Most packages distributed as precompiled binaries will be static libraries or static frameworks.

### Swift Package Manager

SPM does not allow any control over how the dependencies are build and linked - they are all static libraries.
You can, however, specify how to build your own packages via `Package.swift` where you can specify `type: .dynamic`, but of course, this works only if you are the maintainer of that package.

### Carthage

By default, [Carthage uses dynamic frameworks to build your dependencies](https://github.com/Carthage/Carthage#supporting-carthage-for-your-framework), but there is an [option to change them to be built and linked statically](https://github.com/Carthage/Carthage#build-static-frameworks-to-speed-up-your-apps-launch-times).

## Conclusion

As you might have deduced already, there's no silver bullet and you have to choose what applies best to each situation. What is important is that you understand the impact your decision has.

Make sure to check out [Differences in Dynamic & Static Frameworks/Libraries by EssentialDeveloper](https://www.youtube.com/watch?v=IqsKGyklmL0), that does a great job going through some real examples and explaining what each setting does / changes.
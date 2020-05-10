---
layout: post
draft: true
title:  "Worthy iOS libraries (v2)"
date:   2020-05-09 18:00:00.000000000+02:00
categories: 
tags: [draft]

---

A few years ago I tried writing (a few articles) about iOS development. My first article was [Worthy iOS libraries](2013/11/06/ios-libraries.html).



I recently migrated my blog articles to github.io and I'm mentioning this article because this was back in 2013 and boy, how much did our iOS world change since then. 



Just take a look if you're curious which libraries were popular then.



As a small recap, back in 2013:

#### No Swift

There was no Swift yet (at least not released to the public), announced to the public and released only in 2014

#### iOS 7 and OS X 10.8 Mountain Lion

The latest iOS version was iOS 7, containing the popular flat / material redesign by Jony Ive

> The new iOS 7 is radically simplified, incredibly flat, colorful, and multi-layered. It is, according to Apple CEO Tim Cook, “the biggest change to iOS since iPhone.” And it may be one of the best things yet designed by Jony Ive

#### Xcode 5

This was an slightly different major update of Xcode, after Xcode 4 was a complete rewrite of our most popular IDE for iOS / Mac OS programming. It doesn't really stand out with some feature (compared to Xcode 4 being a full rewrite and Xcode 6 adding Swift support).

CLANG was introduced with Xcode 4, but GCC was still used by most projects.

#### Popular libraries

- almost everyone remembers using AFNetworking in every project
- Crashlytics for our crash reporting
- UI7Kit - this makes me laugh, cause most apps wanted to port the look & feel of iOS 7 to iOS 6 and earlier, UI7Kit was a nice helper for that. Anyone remembers this?



Anyway, I do think it's interesting to do an update to that article, so here it is.



Still true (I wrote this in the original article and I think it's still the way I think about reusing code):

> Whether we like it or not, iOS (and MacOS X) development has significantly evolved during the past years. A big part of that is how we write code, reuse it and rely on open source to build great products.
> So I want to recommend some libraries and frameworks that make developers\' lives easier \...

> The days when a developer reinvents the wheel over and over again should be gone (at least for us iOS developers). One should always know what\'s available on the internet before starting to do anything (and I\'m referring to non-trivial tasks).
> Why? You can find a good open source project that does exactly what you need. Even if you don\'t, people who tried to do similar things definitely encountered **issues** you might encounter. Some of them found good **solutions**, other just listed **bottlenecks**, **concerns**. Adding this info to the one you started from determines some boundaries of the potential solution. This global experience is priceless.

# Dependency management

Back in 2013, CocoaPods was the only option for an organized dependency management system, there was no Carthage and, of course, no Swift Package Manager.

#### CocoaPods

Interestingly, [CocoaPods](http://cocoapods.org/) is still a solid option, offering a good amount of features and keeping up with the new features released by Apple. Back in 2013, CocoaPods hasn't been officially released (still around version 0.34.x) and had a limited set of features, but you could use it in the same way you do now: write up a Podfile, run `pod install` or `pod update` and "the magic" happens - you end up with all the dependencies resolved, downloaded, added to a `Pods.xcodeproj` and linked to your targets. The way CocoaPods works today is (almost) the same.



I could write a detailed description of what we can or cannot do with CocoaPods (as it is my go-to tool for managing dependencies, one I know in some depth and feel most comfortable with), but perhaps that is subject to a standalone article.



The important things I want to underline here is this: even though Swift Package Manager is the shinny new tool that's catching up ground, I do believe CocoaPods has much strength in the way it can be customized with hooks or even plugins.



In an ideal world where all dependencies are ready out-of-the-box, it won't make much difference with mgm tool we use, but when things are less than perfect (like having to use a dependency that has no explicit Swift version or wanting the ability to choose between static or dynamic linking of those dependencies), I do want CocoaPods over the alternatives.

#### Swift Package Manager

The official package manager from Apple, this tool is getting more and more powerfull with each year. Apple is investing time and resources, as well as the open comunity around it, so no surprise it's catching up ground. Can already be used on Linux, has direct integration with Xcode, ...



I don't have a lot of experience with SPM, but from my knowledge, it does not offer many ways for configuration, so we must rely on the people who manage each project to set it up properly so we can use it from our own projects.

#### Carthage

Carthage is still there, I think there are people who preffer it over CocoaPods, because of the old "CocoaPods is easy, Carthage is simple" reasons. It has its strenghts, but for me it's never been a valid competitor to CocoaPods just because some dependencies do not support it. During all my projects, I encountered at least one or two dependencies we had to integrate for some reason (might be business reasons) which did not have Carthage support. And since you don't want to have all the dependency managers in a single project, ... we always dropped Carthage.



As a note: if an open source project doesn't support any of CocoaPods, Carthage, SPM, it will be easiest to add CocoaPods support. You can create a `podspec` file for that project and store it in your repo or in a private Specs repo. I had to do that sometimes, even if nowadays that doesn't happen that often.



> Libraries are like steaks. We like them well done

I still enjoy steaks, but I've migrated to more "rare" options :)

# The libraries and frameworks (finally)



1.  Development tools
    1.  [awesome-swift](https://github.com/matteocrippa/awesome-swift) - "A collaborative list of awesome Swift libraries and resources."
    2.  Functional Reactive Programming
        1.  [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) - "Reactive extensions to Cocoa frameworks, built on top of ReactiveSwift"
            - ReactiveCocoa is a project with a long history, being originally written in Obj-C, was rewritten in Swift a few years ago. It's been one of the first open source libraries for Reactive programming, it's been well maintained, has a strong community around it and one valid option to choose.
        2.  [RxSwift](https://github.com/ReactiveX/RxSwift) - "Reactive Programming in Swift"
            - I see it as the main competitor for ReactiveCocoa, also a well maintained + strong community project. If you're not an expert user, I think both are suitable options for a project
        3.  A good read about those two (explaining similarities and differences) is [Reactive Cocoa vs RxSwift @ raywenderlich.com](https://www.raywenderlich.com/1190-reactivecocoa-vs-rxswift#toc-anchor-012) - I linked the "What should you pick" section
    3.  Logging
        1.  **[CocoaLumberjack](https://github.com/robbiehanson/CocoaLumberjack)** - easy to use, super extensible and great performance logging component
            - even if the core of the library is in Obj-C, there is a layer of Swift API available on top of that
            - the library is still well maintained by a good community, has constant releases
            - I might be buyest, cause I am a maintainer for this project, but for me it's still the go-to option for logging
            - and, by the stats available to us maintainers, a lot of people are still using it
    4.  Unit Testing
        1. [OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs) - I still use this project for all my network mocking, it's been around since ever
        2. [KIF](https://github.com/kif-framework/KIF/) - Keep It Functional - An iOS Functional Testing Framework
           - I have been using this in a project with a lot of users and with years of history and it's been a reliable tool for writing and running UI tests. 
           - The project had a very strong automated-tests approach, where each developer added both UI and unit tests for each story, allowing more than 50% of the code to be covered by tests and KIF was at the core of making all this possible.
           - the thing I like about it is that it will build and run your project in a unit testing manner, which means you have access to all the code inside your app, to mock and stub as you please
        3. [Quick](https://github.com/Quick/Quick) - "The Swift (and Objective-C) testing framework". + [Nimble](https://github.com/Quick/Nimble) - "A Matcher Framework for Swift and Objective-C" - I haven't used those, but I do recommend them based on a lot of feedback I'm getting from fellow developers and the high number of people that recommend them at conferences
    5.  Acknowledgements
        1.  [AcknowList](https://github.com/vtourraine/AcknowList) - "View controller listing CocoaPods acknowledgements"
            - it's a nice addition to each project if you're using CocoaPods. It will autogenerate a list of all the 3rd parties and their licenses and make that available via a `UIViewController` which you can just use in your app to display those
2.  Communication and data handling
    1.  Networking
        1.  **[Alamofire](https://github.com/Alamofire/Alamofire)** - "Elegant HTTP Networking in Swift"
            - it used to be AFNetworking, now it's Alamofire the default "go-to" option for a networking library
            - huge community, constant stream of releases, very reliable
        2.  **[AFNetworking](https://github.com/AFNetworking/AFNetworking)** - it's good to see people still maintain and carry on AFNetworking. I think it can still be a good networking library, even if it's in Obj-C and our new projects are more and more Swift-only
        3.  [Moya](https://github.com/Moya/Moya) - Network abstraction layer written in Swift
        4.  [Reachability.swift](https://github.com/ashleymills/Reachability.swift) - Replacement for Apple's Reachability re-written in Swift with closures
    2.  Image caching
        1.  **[SDWebImage](https://github.com/rs/SDWebImage)** - great extensible image caching, good performance, great community, still well maintained
            - again, I might be buyest, cause I am a maintainer for this project, but I think a lot of people are still using this lib over new Swifty ones
        2.  [Kingfisher](https://github.com/onevcat/Kingfisher) - "A lightweight, pure-Swift library for downloading and caching images from the web."
        3.  [AlamofireImage](https://github.com/Alamofire/AlamofireImage) - "AlamofireImage is an image component library for Alamofire"
            - if you already use Alamofire in your project and you don't need a very complicated image dowloading and caching library, AlamofireImage might be what you need
    3.  Data Persistence
        1.  [CoreData](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/index.html) - after all those years, CoreData is still a solid and improved framework for data storage. There were always abstractions on top of it that come and go, but in the end, using CoreData for storing all your data has many benefits (Apple is constantly maintaing that, adding new features and integrations with other services like CloudKit). I'm not saying: use it blindly, I'm saying "consider it when choosing your storage stack and look for solid arguments against it before looking at other options" - at least that's what I do
    4.  Keychain
        1.  [Locksmith](https://github.com/matthewpalmer/Locksmith)
    5.  JSON
        1.  [SwiftyJSON](https://github.com/SwiftyJSON/SwiftyJSON)
        2.  [AlamofireObjectMapper](https://github.com/tristanhimmelman/AlamofireObjectMapper)
3.  UI
    1.  AutoLayout
        1.  **[SnapKit](https://github.com/SnapKit/SnapKit)** - "A Swift Autolayout DSL for iOS & OS X"
        2.  [Cartography](https://github.com/robb/Cartography) - "A declarative Auto Layout DSL for Swift"
    2.  UI controls
        9.  **[Material](https://github.com/CosmicMind/Material)** - "A UI/UX framework for creating beautiful applications"
        2.  many more on [Cocoa Controls](https://www.cocoacontrols.com/) - great place to find open source controls
4.  Maintenance
   1.  Crash reporters
       1.  [PLCrashReporter](https://www.plcrashreporter.org/) - backbone tool of all Crash Reporting solutions
       2.  **[Crashlytics by Firebase](https://github.com/firebase/firebase-ios-sdk.git)** - since Crashlytics by Fabric was discontinued (May 2020), I've migrated all my projects to the very similar Crashlytics by Firebase
   2.  Performance Monitoring
       1.  [Performance by Firebase](https://firebase.google.com/products/performance)
   3.  Tracking
       1.  Firebase
       2.  [Google Analytics](https://developers.google.com/analytics/devguides/collection/ios/)
   4.  Distribution
       1.  Distribution by Firebase

Happy coding!

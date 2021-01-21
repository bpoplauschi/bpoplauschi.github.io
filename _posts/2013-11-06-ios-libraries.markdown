---
layout: post
title:  "Worthy iOS libraries (v1)"
date:   2013-11-06 13:48:40.000000000+02:00
comments_id: 2
categories:
tags: [AFNetworking, analytics, BPForms, cache, caching, Cocoa, controls, cocoadocs, CocoaLumberjack, Crashlytics, CrashlyticsFramework, debug, dev, developer, image, JSON, keychain, library, logging, MagicalRecord, Masonry, media, open source, plugins, ReactiveCocoa, reuse, SDWebImage, social, tool, UI7Kit, unit, test, Xcode, XML]

---

<img style="float: right; margin: 0px 10px 0px 0px" width="20%" src="/assets/framework_icon.png">

Whether we like it or not, iOS (and MacOS X) development has significantly evolved during the past years. A big part of that is how we write code, reuse it and rely on open source to build great products.
So I want to recommend some libraries and frameworks that make developers\' lives easier \...

# Reusing code

<img style="float: right; margin: 0px 10px 0px 0px" width="20%" src="/assets/recycle1.png">

The days when a developer reinvents the wheel over and over again should be gone (at least for us iOS developers). One should always know what\'s available on the internet before starting to do anything (and I\'m referring to non-trivial tasks).
Why? You can find a good open source project that does exactly what you need. Even if you don\'t, people who tried to do similar things definitely encountered **issues** you might encounter. Some of them found good **solutions**, other just listed **bottlenecks**, **concerns**. Adding this info to the one you started from determines some boundaries of the potential solution. This global experience is priceless.

# Global delegation

<img style="float: right; margin: 0px 10px 0px 0px" width="20%" src="/assets/online-delegate-networking.jpg">

We used to have each development team having to build a networking system, an image caching system or the classic logging component. Now, we should be relying on other people who are specialists in a certain area.
For example: most of the people no longer worry about networking, because [Mattt Thompson](http://mattt.me/) does :) (with the great
[AFNetworking](https://github.com/AFNetworking/AFNetworking)). We just need to keep up with him and guys like him, bringing in our own contribution to the community. It\'s only decent to take and give back in a balanced way.
The great advantage of this is developers can focus on their apps\' business challenges, on a great architecture, as solutions for the most common problems are already out there.
Note: Apple has recently added a submission phase where the developers must say if they used 3rd party content, including open source. A good practice seems to be (and a fair one to those who spend their time in doing open source) to list the open sources used together with their [VTAcknowledgementsViewController](https://github.com/vtourraine/VTAcknowledgementsViewController) is a great tool for that.\
**We build better apps \... faster**.

# Cocoapods, mother of all libraries

<img style="float: right; margin: 0px 10px 0px 0px" width="20%" src="/assets/cocoapods-logo.png">

**[This awesome tool](http://cocoapods.org/)** has made it very easy for developers to experiment, add, remove and update libraries and
frameworks. Not to mention the [2700+ and growing repos](https://github.com/CocoaPods/Specs) that can be included in a
project with a single line of text. Plus documentation](http://cocoadocs.org/). See a [great article](http://www.objc.io/issue-6/cocoapods-under-the-hood.html) that
will explain how Cocoapods works.
You no longer have to read a n-page document on how to hook a library to a project, fix warnings and stuff, the pods come out of the box.

# Libraries are like steaks. We like them well done

<img style="float: right; margin: 0px 10px 0px 0px" width="20%" src="/assets/steak.jpg">

Now that we have so many libraries and frameworks available, a new question arises: **which libraries to use**?
This is still subject to subjectivism, but there are definitely some solutions out there that are universally accepted by the iOS community.
At last, I want to briefly list the most common ones, projects from each and every one of us could and should learn.

# The libraries and frameworks (finally)

#### Objective-C

1.  Development tools
    
    1.  Functional Reactive Programming
        1.  [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) - implementation of a paradigm that makes development easier. Strong points:
            - replacement for KVO;
            - possibility of chaining operations;
            - handling asynchronous or event-driven data sources
    2.  Toolkit
        1.  [NimbusKit](https://github.com/jverkoey/nimbus)
        2.  [SSToolKit](https://github.com/soffes/sstoolkit)
        3.  [BlocksKit](https://github.com/pandamonia/BlocksKit) - block based APIs for some system classes and more
        4.  [APUtils](https://github.com/andrei512/APUtils) - great collection of utils I am currently using
        5.  [GBDeviceInfo](https://github.com/lmirosevic/GBDeviceInfo) - retrieve device info
    3.  Debugging
        1.  [DCIntrospect](https://github.com/domesticcatsoftware/DCIntrospect) - for UI debugging
        2.  [PonyDebugger](https://github.com/square/PonyDebugger)
        3.  [SimulatorRemoteNotifications](https://github.com/acoomans/SimulatorRemoteNotifications/) - allows testing notifications on the simulator (faking them)
    4.  Logging
        1.  **[CocoaLumberjack](https://github.com/robbiehanson/CocoaLumberjack)** - easy to use, super extensible and great performance logging
            component
    5.  Formatting
        1.  [FormatterKit](https://github.com/mattt/FormatterKit) - data formatting out of the box (addresses, distance between 2 points, time intervals, \...)
        2.  [TransformerKit](https://github.com/mattt/TransformerKit)
    6.  Unit Testing (read more about iOS Unit Testing [here](http://iosunittesting.com/))
        1. [Kiwi](https://github.com/allending/Kiwi) - good BDD library (specs, expectations, mocks, stubs, async tests). I did have to drop it due to async tests failing to work as expected
        
        2.  [Specta](https://github.com/specta/specta) - another TDD/BDD library. Very similar to `Kiwi`. Works great together with `Expecta` and `OCMock`. Outmatches `Kiwi` with a good async testing feature and the possibility of writing easy to read
            tests (due to `Expecta`):
            
            ```objective-c
            expect(op.retryCount).will.beGreaterThan(0);
            expect(error).willNot.beNil();
            ```
            
        3.  [Expecta](https://github.com/specta/expecta) - expectations for unit tests
        
        4. [OCMock](https://github.com/erikdoe/ocmock)
        
        5. [OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs)
        
        6. [xctool](https://github.com/facebook/xctool) - very useful replacement for xcodebuild tool. It allows running Application Test targets from the command line and has a beautiful output. [Read more](http://nshipster.com/xctool/)
    7.  Code Documentation
        1.  [Cocoadocs](http://cocoadocs.org/) - documentation for all Cocoapods projects
        2.  [appledoc](https://github.com/tomaz/appledoc) - command line tool for generating \"Apple like\" code documentation from comments
    8.  Acknowledgements
        
        1.  Listing the licenses of all the open source libraries used is a recommendation (might become a must in the near future). [VTAcknowledgementsViewController](https://github.com/vtourraine/VTAcknowledgementsViewController)
    9.  Xcode plugins
        1.  [Alcatraz](https://github.com/mneorr/Alcatraz) - Xcode package manager
        2.  [KFCocoaPodsPlugin](https://github.com/ricobeck/KFCocoaPodsPlugin) - CocoaPods plugin, requires Xcode 5
        3.  [VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode) - great plugin that eases code documentation. You just need to press the shortcut (by default `///`) and the code section below to the cursor gets documented (template). Then fill in the missing info - it\'s very easy and it looks great.
        4.  [XcodeColors](https://github.com/robbiehanson/XcodeColors) - used with `CocoaLumberjack` to highlight errors and warnings
        5.  [HOStringSense-for-Xcode](https://github.com/holtwick/HOStringSense-for-Xcode) - takes care of string escaping
        6.  [KSImageNamed-Xcode](https://github.com/ksuther/KSImageNamed-Xcode) - autocomplete for image names
        7.  [Snippets](https://github.com/mattt/Xcode-Snippets) - Mattt Thompson gives away some of the snippets he uses. More about [Xcode Snippets](http://nshipster.com/xcode-snippets/)
2.  Communication and data handling
    1.  Networking
        1.  **[AFNetworking](https://github.com/AFNetworking/AFNetworking)** - the best networking library for iOS and more than that, a true model on how an open source project should be run. It\'s Github repo is a great place to learn.
        2.  [RestKit](https://github.com/RestKit/RestKit) - easy integration with REST webservices based on AFNetworking
        3.  [Reachability](https://github.com/tonymillion/Reachability)
        4.  [CocoaAsyncSocket](https://github.com/robbiehanson/CocoaAsyncSocket) - for low level networking (TCP, UDP)
        5.  deprecated, but worth mentioning as it used to be \#1 [ASIHTTPRequest](https://github.com/pokeb/asi-http-request)
    2.  Model
        1.  [Mantle](https://github.com/Mantle/Mantle) - model framework for easily mapping different format info (i.e. `JSON`, `XML`, ...) to model objects
    3.  Browser
        1.  [KINWebBrowser](https://github.com/dfmuir/KINWebBrowser) or [TSMiniWebBrowser](https://github.com/tonisalae/TSMiniWebBrowser)
    4.  Image caching
        1.  **[SDWebImage](https://github.com/rs/SDWebImage)** - great extensible image caching, good performance
        2.  [Fast Image Cache](https://github.com/path/FastImageCache) - new competitor for `SDWebImage`, claims to get the most out of image caching performance
    5.  Data Persistence
        1.  [CoreData](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/coredata/cdProgrammingGuide.html)
        2.  **[MagicalRecord](https://github.com/magicalpanda/MagicalRecord)** - great wrapper for `CoreData`, makes it easy for everybody to use it
        3.  [fmdb](https://github.com/ccgus/fmdb) - for people that want just a `SQLite` wrapper
    6.  Keychain
        1.  [SSKeychain](https://github.com/soffes/sskeychain)
    7.  JSON
        1.  [NSJSONSerialization](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSJSONSerialization_Class/Reference/Reference.html)
        2.  [JSONKit](https://github.com/johnezang/JSONKit) - high performance JSON parser and serializer
        3.  [comparision between several JSON libs](https://github.com/bontoJR/iOS-JSON-Performance)
    8.  XML
        1.  [NSXMLParser](https://developer.apple.com/library/mac/documentation/cocoa/reference/foundation/Classes/NSXMLParser_Class/Reference/Reference.html)
        2.  [How to choose the best XML parser for your project](http://www.raywenderlich.com/553/xml-tutorial-for-ios-how-to-choose-the-best-xml-parser-for-your-iphone-project)
    9.  HTML
        1.  [Ono](https://github.com/mattt/Ono)
    10. Social
        1.  [Facebook SDK](https://github.com/facebook/facebook-ios-sdk) + [documentation](https://developers.facebook.com/docs/ios/)
        2.  [ShareKit](https://github.com/ideashower/ShareKit)
3.  Media
    1.  Gallery
        1.  [MHVideoPhotoGallery](https://github.com/mariohahn/MHVideoPhotoGallery)
    2.  Image processing
        1.  [GPUImage](https://github.com/BradLarson/GPUImage) - great kit for image and video processing
    3.  Video
        1.  [VLCKit](https://wiki.videolan.org/VLCKit/) - ObjC wrapper for libvlc, the famous tool for video playback, playlists, streaming and transcoding from VLC
        2.  [iOS VideoKit](http://www.binpress.com/app/ios-videokit/1828) - video playing and streaming framework
    4.  2D gaming
        1.  [cocos-2d](https://github.com/cocos2d/cocos2d-iphone)
4.  UI
    1.  UI controls
        1.  **[UI7Kit](https://github.com/youknowone/UI7Kit)** - super useful, enables apps to have a single design on all iOS versions, rather than having dedicated iOS 7 design and another one for prior versions
            With one line of code, makes your UI 99% iOS7 like.
        2.  **[BPForms](https://github.com/bpoplauschi/BPForms)** - Dynamic forms for iPhone/iPad - iOS 6, 7 and later
        3.  [iRate](https://github.com/nicklockwood/iRate)
        4.  [SVProgressHUD](https://github.com/TransitApp/SVProgressHUD) or [MBProgressHUD](https://github.com/jdg/MBProgressHUD)
        5.  [SVPullToRefresh](https://github.com/samvermette/SVPullToRefresh)
        6.  [TTTAttributedLabel](https://github.com/mattt/TTTAttributedLabel)
        7.  [TWSReleaseNotesView](https://github.com/iGriever/TWSReleaseNotesView/) - display the \"What\'s new\" section directly from the AppStore to users that have upgraded.
        8.  **[Masonry](https://github.com/cloudkite/Masonry)** - wrapper for Autolayout
        9.  many more on [Cocoa Controls](https://www.cocoacontrols.com/) - great place to find open source controls
    2.  Helpers
        1.  [iOS-Artwork-Extractor](https://github.com/0xced/iOS-Artwork-Extractor) - gain access to the icons used by Apple frameworks
    3.  Charts
        1.  [CorePlot](http://code.google.com/p/core-plot/)
        2.  [iOSPlot](https://github.com/honcheng/iOSPlot)
    4.  Map
        1.  [CCHMapClusterController](https://github.com/choefele/CCHMapClusterController/) - great performant tool for clustering map pins
        2.  [ADClusterMapView](https://github.com/applidium/ADClusterMapView)
        3.  [REMarkerClusterer](https://github.com/romaonthego/REMarkerClusterer)
    5.  PDF
        1.  [PDFTouch SDK](http://www.binpress.com/app/pdftouch-sdk-for-ios/859)
5. Maintenance
   1.  Crash reporters
       1.  [PLCrashReporter](https://www.plcrashreporter.org/) - backbone tool of all Crash Reporting solutions
       2.  **[Crashlytics (Fabric)](https://www.crashlytics.com)** -now available via CocoaPods (look for Crashlytics and Fabric)
       3.  [Bugsense](https://www.bugsense.com/)
       4.  [Crittercism](https://www.crittercism.com/)
       5.  [QuincyKit](http://quincykit.net/)
   2.  Performance Monitoring
       1.  [New Relic](http://newrelic.com/)
   3.  Tracking
       1.  [Answers (Fabric)](https://answers.io/)
       2.  [Google Analytics](https://developers.google.com/analytics/devguides/collection/ios/)
       3.  [Flurry Analytics](http://www.flurry.com/)
   4.  Testing and distribution
       1.  [BugshotKit](https://github.com/marcoarment/BugshotKit) - in app bug reporting for devs and QAs, generates editable screenshots and collects logs
       2.  Crashlytics Beta - great for distribution as a replacement for the gone TestFlight
       3.  ~~[TestFlight](https://testflightapp.com) - no longer available~~
       4.  [HockeyApp](http://hockeyapp.net)

#### Swift

1.  Development tools
    1.  [awesome-swift](https://github.com/matteocrippa/awesome-swift) - a huge list of Swift resources (libraries and more)
    2.  Functional Reactive Programming
        1.  [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa): Although ReactiveCocoa was started as an Objective-C framework, as of version 3.0, all major feature development is concentrated on the Swift API.
            
        2.  [RxSwift](https://github.com/ReactiveX/RxSwift)
    3.  Unit Testing
        1.  [Quick](https://github.com/Quick/Quick)
    4.  Xcode plugins
    1.  [Build Time Analyzer](https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode) - break down of Swift build times
        2.  [Swift Refactorator](https://github.com/johnno1962/Refactorator) - by default, Xcode does not support Refactoring for Swift. This plugin adds the support
        3.  [Swift Code Snippets](https://github.com/CodeEagle/SwiftCodeSnippets)
2.  Communication and data handling
    1.  Networking
        1.  **[Alamofire](https://github.com/Alamofire/Alamofire)**
    2.  Image Caching
        1.  [Kingfisher](https://github.com/onevcat/Kingfisher)
    3.  JSON
        1.  [SwiftyJSON](https://github.com/SwiftyJSON/SwiftyJSON)
3.  UI
    1.  [SnapKit](https://github.com/SnapKit/SnapKit) - the Swift version of Masonry (same team)
    2.  [Charts](https://github.com/danielgindi/Charts)

Happy coding!

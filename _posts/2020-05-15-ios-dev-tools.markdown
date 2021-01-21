---
layout: post
title:  "Worthy iOS development tools (v2)"
date:   2020-05-15 18:42:41.000000000 +02:00
comments_id: 9
categories: 
tags: []


---

![](/assets/ios-tools.jpg)

This is an update to [another old article called Worthy iOS development tools](../../../2014/03/07/ios-dev-tools.html).

Here is the updated list of tools:

### **IDE and editors**

1.  **[Xcode](https://developer.apple.com/xcode/)**: \#1 IDE from Apple. 
    I think Xcode needs no introduction, it's been around for so long and, for better and worse, it's our go to development tool. (I say that because people sometimes complain about it, but we can always do our thing with it)
2.  [AppCode](http://www.jetbrains.com/objc/) - JetBrains\' alternative to Xcode, very similar to IntelliJ IDEA
4.  **[Sublime Text](http://www.sublimetext.com/)** - text editor for code, especially useful for multiple selection and editing
4.  [**Typora**](http://typora.io) - a nice free markdown editor

I do almost all my coding (Swift and Obj-C) in Xcode and all the rest in Sublime (Ruby, HTML, read log files, ... even open the occasional Swift file) since it has so many plugins. I recently started used Typora for all markdown documents, I find it very nice.

### **Dependency management**

1. **[CocoaPods](http://cocoapods.org/)** 

   > CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It has over 73 thousand libraries and is used in over 3 million apps. CocoaPods can help you scale your projects elegantly.

2. [**Swift Package Manager**](https://swift.org/package-manager/) - 

   > The Swift Package Manager is a tool for managing the distribution of Swift code. It’s integrated with the Swift build system to automate the process of downloading, compiling, and linking dependencies.

   - comes with any Swift 3+ version and is now available directly in Xcode
   - don't let the name fool you, many Objective-C projects support this dependency manager

3. [Carthage](https://github.com/Carthage/Carthage) - 

   > A simple, decentralized dependency manager for Cocoa

I will keep this article short and write a more detailed article about CocoaPods vs SPM vs Carthage and how to choose one vs the other.

For now, I use CocoaPods in all my projects for the abilities to customize the install flow and to pick static vs dynamic linking for each dependency.

### **Design**

1. [**Sketch App**](https://www.sketchapp.com/)

   > Sketch is a vector graphics editor for macOS developed by the Dutch company Bohemian Coding. It was first released on 7 September 2010 and won an Apple Design Award in 2012. It is primarily used for user interface and user experience design of websites and mobile apps and does not include print design features.

2. **[GIMP](http://www.gimp.org/)**

   > The Free & Open Source Image Editor

3. [Photoshop](http://www.photoshop.com/)

   > Adobe *Photoshop* is a raster graphics editor developed and published by Adobe Inc.

4. [Sip](https://setapp.com/apps/sip) - color picker + transform to `UIColor` / `NSColor` / ...

5. [Zeplin](https://zeplin.io)

   > Beautiful products happen here
   > The better way to share, organize and collaborate on designs—built with developers in mind.

I used all the tools I listed here in different moments and on different projects. The one constant is I always have GIMP installed and ready for some quick editing.

### **Image handling**

1.  [Resizer](https://itunes.apple.com/us/app/resizer/id411277085?mt=12) - generate non-retina images from retina ones
2.  [ImageOptim](http://imageoptim.com/) - nice&easy ui for [pngcrush](http://pngcrush.com/)

ImageOptim is something I use constantly, before checking in files to git, just to keep the small.

### **Debugging**

1. [Charles Web Debugging Proxy](https://www.charlesproxy.com)

   > Charles is an HTTP proxy / HTTP monitor / Reverse Proxy that enables a developer to view all of the HTTP and SSL / HTTPS traffic between their machine and the Internet. This includes requests, responses and the HTTP headers (which contain the cookies and caching information).

2. [SQLite Browser](https://sqlitebrowser.org) - SQLite browser

Charles is a tool that comes very handy when debugging a networking component or a networking problem. It's not very easy to learn, as it has very low level details, but once you do, it will give you all the information you need.

### **Source control**

1. **[Git](http://gitscm.org/)** - the default option for source control for most developers

   > Git is a [free and open source](https://git-scm.com/about/free-and-open-source) distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

2. **[Github](github.com)** - web based hosting service for git / mercurial / svn projects

   > GitHub is a development platform inspired by the way you work. From [open source](https://github.com/open-source) to [business](https://github.com/business), you can host and review code, manage projects, and build software alongside 50 million developers.

3. [GitLab](https://gitlab.com)

   > From project planning and source code management to CI/CD and monitoring, *GitLab* is a complete DevOps platform, delivered as a single application.

4. [BitBucket](https://bitbucket.org/product)

   > Bitbucket is more than just Git code management. Bitbucket gives teams one place to plan projects, collaborate on code, test, and deploy.

5. **[SourceTree](http://www.sourcetreeapp.com/)** - git, mercurial, svn (svn via git) client

Over the last years, I started working more and more with git (almost all projects are git hosted) and GitHub, as it's a very powerful dev platform with a lot of integrations (like CI tools, ...). GitLab and BitBucket are good alternatives that offer free private repos (where GitHub only offers free open-source repos and you need to pay for the private ones).
GitLab is a company I follow and admire, they have a very interesting trajectory and have always been a "remote" company with high standards.

### **Continuous Integration**

1. [GitHub actions](https://github.com/features/actions) - new and very nice

   > *GitHub Actions* connects all of your tools to automate every step of your development workflow. Easily deploy to any cloud, create tickets in Jira, or publish a package to npm. Want to venture off the beaten path? Use the millions of open source libraries available on *GitHub* to create your own *actions*.
2. [Travis CI](https://travis-ci.org)

   > *Travis CI* enables your team to test and ship your apps with confidence. Easily sync your projects with *Travis CI* and you'll be testing your code in minutes.
3. [Bitrise](https://app.bitrise.io)

   > Continuous integration and delivery built for mobile: Automate iOS and Android builds, testing and deployment from your first install to the one millionth.
4. [Jenkins](https://www.jenkins.io)

   > The leading open source automation server, Jenkins provides hundreds of plugins to support building, deploying and automating any project.

I used Travis CI, Bitrise, Jenkins and now recently started using GitHub actions, I think they all have pros and cons, so I can't really recommend one vs the other.

### **Diff and merge**

1.  **[Araxis Merge](http://www.araxis.com/merge/)** - I have been using Araxis Merge for over a decade and I still like it very much. They kept adding features over the years, but the core diff tool is just great.
2.  [Kaleidoscope](http://www.kaleidoscopeapp.com/)

### **Crash reporting**

1.  **[Crashlytics by Firebase](https://firebase.google.com/products/crashlytics)**

### **Linting**

1. **[SwiftLint](https://github.com/realm/SwiftLint)**

   > A tool to enforce Swift style and conventions, loosely based on [GitHub's Swift Style Guide](https://github.com/github/swift-style-guide).
   >
   > SwiftLint hooks into [Clang](http://clang.llvm.org/) and [SourceKit](http://www.jpsim.com/uncovering-sourcekit) to use the [AST](http://clang.llvm.org/docs/IntroductionToTheClangAST.html) representation of your source files for more accurate results.

2. [IBLinter](https://github.com/IBDecodable/IBLinter)

   > A linter tool to normalize `.xib` and `.storyboard` files. Inspired by [realm/SwiftLint](https://github.com/realm/SwiftLint)

I usually setup SwiftLint for every project I work on, just to make sure everyone follows the same conventions. I think it helps a lot, especially avoid unwanted antipatterns like forced unwraps.
If you want to keep your Interface Builder files clean and without issues, I also recommend setting up IBLinter in a similar manner.

### **Command line tools**

1. **[fastlane](https://github.com/fastlane/fastlane)**

   > The easiest way to automate building and releasing your iOS and Android apps
2.  [Nomad CLI](http://nomad-cli.com/)
    1. `CUPERTINO` - Automate administrative tasks that you would normally have to do through the Apple Dev Center website
    2. `HOUSTON` - Send push notifications from the command line
    3. `DUBAI` - Generate Passbook .pkpass files with ease
    4. `VENICE` - Secure your In-App-Purchases
    5. `SHENZHEN` - Create development builds and then distribute them via TestFlight, HockeyApp, Amazon S3, or FTP
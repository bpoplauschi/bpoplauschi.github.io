---
layout: post
title:  "Worthy iOS development tools"
date:   2014-03-07 13:53:41.000000000 +02:00
categories: 
tags: [Alcatraz, analytics, AppCoode, Araxis Merge, coding style, crash, Crashlytics, debug, dependency, design, dev, developer, diff, GIMP, Git, Github, IDE, Kaleidoscope, merge, Nomad CLI, Objective Clean, plugins, Pony Debugger, Reveal, SourceTree, Sublime, tool, VVDocumenter]


---

<img style="float: right;" width="30%" src="/assets/developertools.png">

A while ago I published a compiled list of [Worthy iOS libraries](2013-11-06-ios-libraries/).
A list of iOS dev tools will complement that, so here it is\...

The list of tools:

1.  **IDE and editors**
    1.  **[Xcode](https://developer.apple.com/xcode/)**: \#1 IDE for Objective-C products, made by Apple
    2.  Xcode plugins:
        1.  **[Alcatraz](http://alcatraz.io/)**: Xcode package (plugins, themes, templates) manager - has recently release the 1.0 version with support for Xcode 5
        2.  [Cocoapods Xcode plugin](https://github.com/kattrali/cocoapods-xcode-plugin) - Dependency management helper for your CocoaPods, right in Xcode
        3.  **[VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode)** - great tool for writing documentation, integrates perfectly with appledoc
        4.  [XCodeColors](https://github.com/robbiehanson/XcodeColors) - allows using colors in the Xcode debugging console (TTY). Works great with [CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack)
        5.  [HOStringSense](https://github.com/holtwick/HOStringSense-for-Xcode) - working with strings made easy (especially for escaping chars)
        6.  [KSImageNamed](https://github.com/ksuther/KSImageNamed-Xcode) - autocomplete for `imageNamed`
        7.  [DerivedData-Exterminator](https://github.com/kattrali/deriveddata-exterminator)
        8.  for Xcode 4 users, there is a collection of plugins [Xcode 4 Fixins](https://github.com/davekeck/Xcode-4-Fixins)
    3.  [AppCode](http://www.jetbrains.com/objc/) - JetBrains\' alternative to Xcode, very similar to IntelliJ IDEA
    4.  **[Sublime Text](http://www.sublimetext.com/)** - text editor for code, especially useful for multiple selection and editing
2.  **Project analysis**
    1.  **[Faux Pas](http://fauxpasapp.com/)** - Faux Pas inspects your iOS or Mac app's Xcode project and warns about possible bugs, as well as about maintainability and style issues.
3.  **Dependency management**
    1.  **[CocoaPods](http://cocoapods.org/)** - CocoaPods is the dependency manager for Objective-C projects. It has thousands of libraries and can help you scale your projects elegantly. Includes [documentation](http://cocoadocs.org/) for the libraries. [Specs are here](https://github.com/CocoaPods/Specs)
4.  **Design**
    1.  [Sketch App](https://www.sketchapp.com/)
    2.  **[GIMP](http://www.gimp.org/)**
    3.  [Photoshop](http://www.photoshop.com/)
    4.  [Sip](https://itunes.apple.com/us/app/sip/id507257563?mt=12) - color picker + transform to `UIColor` / `NSColor` / ...
5.  **Images**
    1.  [Resizer](https://itunes.apple.com/us/app/resizer/id411277085?mt=12) - generate non-retina images from retina ones
    2.  [ImageOptim](http://imageoptim.com/) - nice&easy ui for [pngcrush](http://pngcrush.com/)
6.  **Debugging**
    1.  **[Reveal](http://revealapp.com/)** - debug and inspect UI, see live changes without recompile
    2.  **[Pony Debugger](https://github.com/square/PonyDebugger)** - Remote network and data debugging for your native iOS app
    3.  [SQLiteManager](http://sourceforge.net/projects/sqlitebrowser/) - SQLite browser
7.  **Source control**
    1.  **[Git](http://gitscm.org/)** - widely used source control
    2.  **[Github](github.com)** - web based hosting service for git / mercurial / svn projects
    3.  **[SourceTree](http://www.sourcetreeapp.com/)** - git, mercurial, svn (svn via git) client
8.  **Diff and merge**
    1.  **[Araxis Merge](http://www.araxis.com/merge/)**
    2.  [Kaleidoscope](http://www.kaleidoscopeapp.com/)
9.  **Crash reporting**
    1.  **[Crashlytics](http://www.crashlytics.com)**
10. **Coding style**
    1.  [Objective Clean](http://objclean.com/index.php) - perform analysis of source code and points out styling issues
11. **Sales and analytics**
    1.  [AppFigures](appfigures.com) - website with insights into iTunes Connect data (sales, purchases, trends, reviews&ratings, ads, ...)
    2.  [AppViz](https://appviz.com) - Mac app with insight into iTunes Connect data
    3.  [Applyzer](applyzer.com) - app statistics
12. **Command line tools**
    1.  [Nomad CLI](http://nomad-cli.com/)
        1.  CUPERTINO - Automate administrative tasks that you would normally have to do through the Apple Dev Center website
        2.  HOUSTON Send push notifications from the command line
        3.  DUBAI Generate Passbook .pkpass files with ease
        4.  VENICE Secure your In-App-Purchases
        5.  SHENZHEN Create development builds and then distribute them via TestFlight, HockeyApp, Amazon S3, or FTP

---
layout: post
title:  "Handling iOS dependencies with CocoaPods Rome and git-lfs"
date:   2021-01-03 14:00:00 +0300
categories: []


---

# Problem

Handling the 3rd party dependencies for an iOS project is not the simplest of jobs.

I'm saying this because:

- over years, projects got used to adding more and more 3rd party dependencies
- CocoaPods, Carthage and now Swift Package Manager are good tools, but we sometimes want a combination of their features
- Xcode doesn't always deal well with a lot of source files (anyone familiar with the nevereneding Indexing...?) , so the CocoaPods setup where the Pods.xcodeproj is linked and those pod targets are built everytime is a bit non-optimal
- some 3rd parties just don't support Carthage or SPM

# How do other platforms do this

Our community has always learned from other communities. If we look at the way Java projects handle their dependencies via maven, we'll see they have nice features:

- they also use a file to specify which dependencies and what versions they need
- maven can just install the compiled libs, saving precious time on each machine
- maven has a cache layer

# Designing a solution

I would want my dependency managament system to:

- be supported by all the 3rd parties I need (not that easy to achieve :) )
- for those that don't have support for it, to be super easy to add it
- use a simple text file (that will be checked in to the repo) to keep track of all the dependencies
- avoid any additional projects and overloading Xcode with many source files
- cache precompiled versions of each dependency
- that cache should be available to all dev machines and to any CI machines as well, to speed up the builds

# Solution

- CocoaPods + Rome plugin + store the binaries inside git but using git-lfs
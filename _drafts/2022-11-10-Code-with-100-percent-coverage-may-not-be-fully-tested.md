---
layout: post
title:  "Code with 100% coverage may not be fully tested"
date:   2022-11-10 07:00:00 +0300
categories: []
tags: []
---

I recently fell again in the trap of thinking 100% code coverage means you got everything covered. Even though I know code coverage doesn’t tell the entire story, and actually asserting every aspect of your code is what matters.
So I thought I share this with the community.

Context:
- legacy codebase (per Michael Feathers - code without tests), wanted to refactor a larger ViewController containing a vertical UIScrollView and multiple  UIScrollView s for each section displayed horizontally. VC loads the UI layout from xib

Actions:
- step 1 - cover this VC with unit tests, got to 100% code coverage, all tests passing. Added snapshot tests for each state this VC could be in to make sure refactoring won’t break the UI layout. All good here
- step 2 test-drive new type - a generic HorizontalListViewController with just a horizontal UICollectionView 
- step 3 - refactor the main VC by replacing each hardcoded section with an instance of the generic HorizontalListViewController . Run tests after each change, always green
- step 4 - still run app because of old habits to make sure I didn’t break anything - I had the surprise that all the button actions were broken

“The catch”:
- when I tested each IBAction in the VC, I didn’t do it as recommended in the program, which is via the simulateTap on each button through the little extension on UIControl that just iterates all the targets and all the actions and calls each selector
- instead I called the IBAction methods directly from the tests
- when I replaced content from the main VC xib with my generic HorizontalListVC I didn’t change the xib setup accordingly, so basically when I deleted the views from my main xib, these IBActions were left hanging
- but because I called them directly, my tests were still green

Lessons learned:
- code coverage alone doesn’t say much. 
- I like and follow the advice in the Program that when adding tests to already written code, comment parts of the production code to make sure you get a test failure when running the tests, just to make sure you properly assert that behavior.
- when dealing with code + xib / storyboard / …, like in the case of a ViewController, make sure you understand that and test them together, because the production app will
- just follow the instructions from the program :smile: 
- in this particular instance, buttons should be tested by simulating the tap, not calling their action directly which can leave gaps in your coverage




Of course, as you said, code coverage doesn’t tell you that you are doing things right. The point is to have suite of tests that you can trust, so then you can refactor the code with confidence. Being able to trust the tests means you have a very high coverage, but if you do, it’s not sufficient 
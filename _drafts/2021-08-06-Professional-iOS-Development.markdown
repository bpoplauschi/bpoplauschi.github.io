---
layout: post
title:  "Professional iOS Development"
date:   2021-08-06 15:00:00 +0300
categories: []
tags: []
---

# Professional iOS Development

What traits make a professional developer and how do those apply to the world of iOS (and other Apple platforms)?
How do iOS dev standards compare to ones from other platforms?
I certainly don't know the full answer, but I know enough to start this conversation.
I invite you to an open talk about Clean code, Clean architecture and modular design, SOLID principles, automated testing, TDD, CI and CD, and more.

## What makes a professional developer
- who is Robert C Martin
- his quest to answer this question and establish industry standards
- see The Clean Coder
- what makes a professional in other fields: medicine, research, sports? They ahdere to a clear work guideline, have rules, ethics and comitees that verify their output

## Professionalism is about taking responsibility
- Do no harm: don't create bugs (almost impossible, but still...). Analogy: software systems are very complex and thus impossible to be bug-free. But the human body is also incredibly complex, but doctors still take the oath to do no harm
- QA should find nothing: delivering faulty code is just unprofessional
- You mush know it works: automated tests
- Work ethic: your career is your responsibility (trainings, conferences, books, ... and the time needed to learn)
- Know your field:
  - design patterns: GOF
  - design principles: SOLID
  - methods: XP, Scrum, Kanban, Waterfall, ...
  - disciplines: TDD, OOD, Structured programming, CI, Pair programming
  - artifacts: UML, State Transition Diagrams and tables, flow charts, ...
- Practice
- Collaboration
- Mentoring
- Know your domain
- Humility

## Saying No and Saying Yes
- Professionals have the courage to say No to their managers and they work hard to find creative ways to make Yes possible.
- Your manager is counting on you to defend your objectives as aggressively as they defend theirs. Both you and your manager need to get to the best possible outcome through negotiation.
- Professionals pursue and defend their objectives as aggressively as they can.
- Recognize lack of commitment words and phrases like “hope” and “Let’s see if we can get this done…”. A sincere commitment sounds like “I will do something… by this certain date…”
- Bring up blockers or red flags as soon as they come up — actively communicate.

## Coding
- Coding is an intellectually challenging and exhausting activity.
- If you are tired, worried, or distracted, do not code. Your code will have bugs or a bad structure.
- Spend personal time before work trying to resolve or mitigate personal issues or demands so you can focus your mental energy on being a productive problem solver at work.
- Be prepared to be interrupted and help someone — it’s the professional thing to do.
- Ask for help and ask to give help - be a mentor.

## Test-Driven Development
- Writing your tests first:
- Good tests function like good documentation.
- TDD is a discipline that enhances certainty, courage, defect reduction, documentation, and design.

## Practice, Practice, Practice
- When performance matters, professionals practice.
- All professionals practice their art by engaging in skill-sharpening exercises.
- Doing anything quickly requires practice. It’s not always wise to go fast, but sometimes it is better to do it as fast as possible and is highly productive.
- Practice coding outside of work by doing kata.
- Open source: Take on some pro-bono work by contributing to an open-source project.

## Testing Strategies
Every professional development team needs a good testing strategy, and we can start by following the "Test Pyramid":
(image)
- Unit tests
- Component tests
- Integration tests
- System tests
- Manual tests

## Time Management
- What strategies can you use to ensure that you don’t waste time?
- Meetings are both necessary and huge time wasters. You do not have to attend every meeting — be careful about which ones you decline and those you choose to attend.
- Use tools like the Pomodoro Technique.
- Evaluate the priority of each task, disregarding personal fears and desires, and execute those tasks in priority order.

## Estimation
- Estimation is one of the simplest, yet most frightening activities that software professionals face.
- Know the difference between estimates and commitments.
- A commitment is something you must achieve. An estimate is a guess.
- Learn methods to get better estimates like PERT, Fingers in the Air and Planning Poker
- Always include error bars with your estimates so that the business understands the uncertainty.
- Don’t make commitments unless you know you can achieve them.

## Stay Cool Under Pressure
- The professional developer is calm and decisive under pressure.
- Under pressure? Be sure to manage your commitments, follow disciplines, keep code clean, communicate, and ask for help.
- Don’t succumb to the temptation to create a mess in order to move quickly.

## Collaborate
- Programming is all about working with people. It’s unprofessional to be a loner or a recluse on a team.
- Often programmers have difficulty working closely with other programmers. That’s no excuse. Being a developer means working with people.
- Meet the needs - collaborate with your managers, business analysts, testers, and other team members in order to deeply understand the business goals.
- Pairing is a great way to share knowledge and the best way to review code.

## Get Aligned with Your Teams
- Strive to have a “gelled” team.
- A gelled team is one that forms relationships, collaborates, and learns each other's quirks and strengths.
- Teams are harder to build than projects. It’s better to form persistent teams that move together from one project to the next and can take on more than one project at a time.

## Mentoring, Apprenticeship, and Craftsmanship
- The software development industry has gotten the idea that programmers are programmers, and that once you graduate you can code.
- Software apprenticeship is a three-step journey: starting from apprentice and moving to journeyman before becoming a master.
- Be a craftsman - someone who works quickly, but without rushing, provides reasonable estimates and meets commitments. Know when to say No but try hard to say Yes.
- If craftsmanship is your way of life, keep in mind that you cannot force other programmers to become craftsmen.

## Ideas to be mentioned
- Software definition, it should be easy to change, cost of change should not increase with time
- MVC, MVVM, MVP, VIPER are just design patterns for the UI code. They don't solve the problem of who should handle many other responsibilities like: data access and storage, notifications, navigation and deeplinking, ...
- Low coupling + high cohesion
- number of 3rd party dependencies -> risk. DI principle

## References
- [iOS Lead Essentials program](https://iosacademy.essentialdeveloper.com/p/ios-lead-essentials/) - Online program meticulously thought out for iOS developers who want to become world-class senior developers and be part of the highest-paid iOS devs in the world. Focuses on key concepts like Swift, TDD, BDD, DDD, Clean Architecture, Design Patterns, Git, Automation, CI/CD, and Modular Design.
- [The Clean Coder by Robert C. Martin](https://www.goodreads.com/book/show/10284614-the-clean-coder)
- [Clean Code by Robert C. Martin](https://www.goodreads.com/book/show/3735293-clean-code)
- [Dependency Injection: Principles, Practices, and Patterns by Mark Seemann and Steven van Deursen](https://www.goodreads.com/en/book/show/44416307-dependency-injection-principles-practices-and-patterns)
- [Test Driven Development: By Example by Kent Beck](https://www.goodreads.com/book/show/387190.Test_Driven_Development)
- [Working Effectively with Legacy Code by Michael C. Feathers](https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code)
- [Design Patterns by Gamma, Johnson, Vlissides, Helm](https://www.goodreads.com/book/show/85009.Design_Patterns)
- [Clean Architecture by Robert C. Martin](https://www.goodreads.com/book/show/18043011-clean-architecture)
- [Pro Git by Scott Chacon](https://www.goodreads.com/book/show/6518085-pro-git)
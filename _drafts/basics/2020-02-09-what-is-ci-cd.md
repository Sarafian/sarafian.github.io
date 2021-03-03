---
title: Basics - What is continuous integration (CI) and continuous deployment (CD)
excerpt: 
tags: 
  - 123
categories: 
  - basics
---

This post is part of a series intended to explain software engineering topics to an audience that is not technical or experts. Please consider the original [introduction post]({% link _posts/basics/2020-02-08-new-series-basics-software-engineering.md %}) for more context.

If you would search on google "what is continuous integration" and "what is continuous deployment" for Wikipedia links you would get the following two definitions respectively:

> In software engineering, continuous integration (CI) is the practice of merging all developers' working copies to a shared mainline several times a day. 

> Continuous deployment (CD) is a software engineering approach in which software functionalities are delivered frequently through automated deployments.

The focus of the CI and CD is to catch as early as possible a potential error without adding a heavy workload to the members of the team.

To simplify CI and CD we first need to understand a typical workflow of a developer and tester in a more traditional setup. Though for most readers this will sound too old school, I'll use such a paradigm to focus on the pain points and what CI and CD offers. 

As the developer programs (create, edit code lines), he/she also runs tests to make sure that what has been created works as intended. The problem is that the average developer won't run all possible tests, either because of time constraints or because he/she is unaware of other scenarios. The issue becomes more complicated when the involved code base has an impact on a wider scope and that happens recursively as the application has usually thousands of lines of code. At some point, the developer considers the job done and builds an artifact. Normally the artifact would be picked by a tester, who would perform tests. Though more knowledgeable, the tester faces the same constraints as the developer. On top of that, both personas are human and we humans make mistakes. We can make mistakes both by overlooking an issue but also by making a mistake in the process which could result in false positives. In the end, the involved code is deployed in production and if anything was overlooked you have problems. 

As the industry evolved, certain aspects for quality were introduced like unit tests, smoke tests, static and dynamic code analysis and other sorts of tests that intend to improve the quality of the team. Though powerful, these tests require repetition and are also subject to errors when performed by humans. Then, there is the part where the proposed code is merged with the rest of the code base and all the above needs to be reviewed and happen again. As the teams started growing, there were more developers and testers in a team and they would often start clashing with each other over the same process which would result in chaotic situations, missed deadlines and worse of all severe problems after deployment.

The goals of the CI are

1. Increase the quality of the developed code and test coverage.
1. Reduce the risk.
1. Reduce the manual effort involved. We want people to do more creative stuff and less repetitive tasks.
1. Increase collaboration.
1. Reduce the burden of responsibility from the members of the team. 

With CI, a lot of the steps are automated. Tests are run automatically that produce reports that are visible to everyone. This could be an early sign that the involved code is not good enough and needs rework. Machines will always do the same and therefore with automation, we can remove the human error factor and also improve the daily life of the team members. CI can be layered and as the code moves through the process, heavier and lengthier tests can be run that would have normally been a blocker for the developer. This means that the team members can focus on other topics while the heavy-duty testing happens. Automation can also help with code generation. Using templates to produce code that has always the same consistency and quality. 

Once the deliverable is prepared and the developers and testers are happy with the quality, a production release happens. What this means is that the release artifact is handed over to the operations team which is responsible to deploy it. The release artifact can have many forms but for sake of simplicity let's assume that it is compromised by execution assets, instructions on how to install them and any other useful information for the OPS. The OPS now faces a challenge. They need to deploy the release and as with the previous paradigm, this can be a tedious effort and with mistakes. Because it is considered high risk and laborious operation, organizations don't want to perform frequent releases which have many implications both on the internal velocity but also how the organization is perceived.

CD, like CI, tries to change this using automation. Imagine if the release artifact has not instructions in word but scripts that could perform the installation. As long as the conditions are stable, then this automation can produce much more servers, quicker and with higher quality than their human counterpart. Now consider that this automation is not just about the execution part but also for the infrastructure. This would be mean that the entire environment can be created or modified in such a similar way. But if creating an environment is that easy, then we can use this principle to create the dev and test environments to match what exists in production, thus offering the development teams an environment that represents production much better. Overall, the developers are more connected to the conditions of production and can produce better quality artifacts, the testers can quickly spin up an environment to check something and operations can trust that everyone is more aligned with the production setup to reduce surprises and problems from lack of communication. Operations can quickly do a full test on the release before applying it for production without putting in much effort. 

Note, that the complexity of the release artifact is much more than described here and in a future post about containers I'll explain why they have been an industry game-changer.

Overall, CI and CD is about building and using software that the end-user doesn't see and doesn't appreciate. It's a principle to maximize throughput of delivering new features with higher quality standards while at the same time we keep the creative people happy and freer to do what humans are best, that is to be creative.
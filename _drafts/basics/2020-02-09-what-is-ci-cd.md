---
title: Basics - What is continuous integration (CI) and continuous deployment (CD)
excerpt: 
tags: 
  - 123
categories: 
  - basics
---

This post is part of a series intended to explain software engineering topics to an audience that is not technical or experts. Please consider the original [introduction post]({% link _posts/basics/2020-02-08-new-series-basics-software-engineering.md %}) for more context.

If you would search on google "what is continuous integration" and "what is continuous deployment" for wikipidia links you would get the following two definitions respectevely:

> In software engineering, continuous integration (CI) is the practice of merging all developers' working copies to a shared mainline several times a day. 

> Continuous deployment (CD) is a software engineering approach in which software functionalities are delivered frequently through automated deployments.

The focus of the CI and CD is to catch as early as possible a potential error without adding a heavy workload to the members of the team.

To simplify CI and CD we first need to understand a typical workflow of a developer and tester in a more traditional setup. Though for most readers this will sound too old school, I'll use such a paradigm to focus on the pain points and what CI and CD offers. 

As the developer programs (create, edit code lines), he/she also runs tests to make sure that what has been created works as intented. The problem is that the average developer won't run all possible tests, either because of time constraints or because he/she is unaware about other scenaries. The issue becomes more complicated, when the involved code base has impact on a wider scope and that happens recursively as the application has usally thousands of lines of code. At some point, the developer considers the job done and builds an artifact. Normally the artifact would be picked by a tester, who would perform tests. Though more knowledgeful, the tester faces the same constraints as with the developer. On top of that, both personas are human and we human make mistakes. We can make mistakes both by overlooking an issue but also by making a mistake in the process which could result in false possitives. At the end, the involved code is deployed in production and if anything was overlooked you have problems. 

As the industry evolved, certain aspects for quality where introduced like unit tests, smoke tests, static and dynamic code analysis and other sort of tests that intend to improve on the quality of the team. Though powerful, these tests require repetition and are also subject to errors when performed by humans. Then, there is the part where the proposed code is merged with the rest of the code base and all above needs to be reviewed and happen again. As the teams started growing, there where more developers and testers in a team and they would often start clashing with each other over the same process which would result into chaotic situations, missed deadlines and worse of all severe problems after deployment.

The goals of the CI are

1. Increase quality of the developed code and test coverage.
1. Reduce the risk.
1. Reduce the manual effort involved. We want people to do more creative stuff and less of repetitive tasks.
1. Increase collaboration.
1. Reduce the burden of responsibility from the members of the team. 

With CI, a lot of the steps are automated. Tests are run automatically that produce reports that are visible to everyone. This could be an early sign that the involved code is not good enough. Automation, that is work run by machines, will always be the same hence removing the human error factor and also improving the daily life of the team members. CI can be layered and as the code moves through the process, heavier and lenghier tests can be run that would had normally been a blocker for the developer. This means that the team members can focus on other topics while the heavy duty testing happens. Automation can also help with code generation. Using templates to produce code that has always the same consistency and quality. 

Once the deliverable is prepared and the developers and testers are happy with the quality, a release to production happens. What this means is that the release artifact is handed over to the operations team which is responsible to deploy it. The release artifact can have many forms but for sake of simplicity let's assume that it is compromised by executional assets, intrustrions on how to install them and any other useful information for the OPS. The OPS now face a challenge. They need to deploy !!!!!!!





With a process that focuses on more efficiency and confidence of the engineers, the team feels more empowered but also feels the benefit of the safety nets introduced against their own mistakes. This allows the team to be more comfortable and to be more creative and explore more innovative ideas. After all the above have high risk and for this reason "threatening" ideas are avoided with teams without a CI. These teams feel and perceived as slow when compared to other teams that have embraced automation.


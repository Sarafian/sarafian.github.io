---
title: DEVOPS. What is it, why should you embrace it and is it the same as with automation?
excerpt: DEVOPS has become the new hype word and very often misused. In this post, I'll explain what DEVOPS is, what it isn't and why it is often confused with automation.
tags: DEVOPS
categories: Coaching
---

During 2016 the term DEVOPS was a hype word. At least this how I experienced it then through my professional engagements. Everywhere and anywhere, somehow you would hear about DEVOPS. It became such a hype, that it became a common topic even in those daily, weekly or monthly internal emails about company culture etc.

What I found very strange, was that everyone seemed to have a difference of opinion about what it two main different core understandings. There was one, that used the term in the context of organizations. And there was another which seemed to derive its understanding from something like "lets develop/code operations".

The difference is significant and since it is not the first time that elements in the industry are misusing certain words wrongly, I've decided to do some investigation. Internet was also as confusing so I decided to look into literature, since books are much better in building the foundations around a concept. Unfortunately, the Internet is full of self opinionated and marketing oriented type of content and often it is not easy to distinguish the truth. Proper research that is required to write a book is always indispensable. This situation brought back memories from when agile was a hype in its time. Then everyone talked about it but few actually managed to do agile, yet alone understand it.

![The DEVOPS Handbook](https://www.safaribooksonline.com/library/cover/9781457191381/360h/){: .align-left}
Following some twitter profiles that seem to know a bit or two about certain things, I noticed an excitement for [The DEVOPS Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/) which was getting released during that time. So I decided to buy it. After reading it, it became to clear to me that the Internet in this case acts a lot like fake news. Partly correct to get readers a buy-in and excitement, but mostly wrong. So this post is about trying to help you understand what DEVOPS is and what it is not. Since this is also a blog post on the Internet, I understand the controversy so I already suggest you to read a book about the subject. If you decide to still read this blog post, then please keep in mind that what you will read mostly originates from this book and from my experiences in organizations that claimed to do or understand DEVOPS.

## What is DEVOPS NOT?

When discussing with other about DEVOPS, I find it very useful to first clear the air from the usual misunderstandings. So these are what DEVOPS **is not**:

- Not a skill. DEVOPS is not something you ask a person to have as skill.
- Not **DEV**elopment+**OP**eration**S**.
- Not automation. Yes there is some relationship between the two but they are not a synonym.
- Not about the continuous integration nor the continuous deployment pipeline. This quite often the problem with job advertisements.

## Why DEVOPS?

It is also very useful to understand the purpose of DEVOPS because the motivation sets the foundation for a better apprehension of the meaning and goal of DEVOPS. I'll use a quote from [The DEVOPS Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/) which I find that it offers the best summary for motivation.

> DEVOPS benefits us all in the value stream, whether we are Dev, OPS, QA, InfoSec, Product Owners or customers. It brings joy back to developing great products, with fewer death matches. It enables humane working conditions with fewer weekends and missed holidays with our loved ones. It enables teams to work together to survive, learn, thrive, delight our customers and help our organization succeed.
{: .text-center}

Notice that this doesn't refer to words such as skill, automation, programmer or etc. Instead it focuses on concepts such as **great product**, **organization** and **way of working**.
{: .notice}

## What is DEVOPS then?

DEVOPS is first and foremost a culture. Having read the summary above, it can't be something personal. It needs to be bigger than you, I or a group of 5 developers. It is a discipline within an organization that develops software. It is all about how everyone in the organization works and as such it applies to all members regardless of rank.

One of the most interesting summaries I've ever come across is by Adam Jacom ([@adamhjk)](https://twitter.com/adamhjk)) during [Chef Style DevOps Kungfu](https://www.youtube.com/watch?v=_DEToXsgrPc&feature=youtu.be) and he said:

> A cultural and professional movement, focused on how we build and operate high velocity organizations, born from the experiences of its practitioners
{: .text-center}

Notice that this also doesn't refer to words such as skill, automation, programmer or etc. Instead, it focuses on concepts such **movement**, **culture** and **organization**.
{: .notice}

![devops-philosophy-culture-movement](/assets/images/posts/2018-09-12-devops-philosophy-culture-movement.png){: .align-right}
Philosophy, culture and movement are the building blocks of DEVOPS and one is pushing the other. They are at the foundation of the organization and this means that the organization lives and breaths DEVOPS. Anyone who would be hired in such an organization, will operate within this construct. He or she, will either contribute and flourish or fail like with any other rules of engagement. This is extremely important because this flows from the top to the bottom ranks and from the bottom to the top.

When an organization decides to embrace DEVOPS, then during the transition period it must flow from the top ranks more that it flows from the bottom.
{: .notice}

![devops-principals-flow-feedback-continuous-learning-experimentation](/assets/images/posts/2018-09-12-devops-principals-flow-feedback-continuous-learning-experimentation.png){: .align-left}
DEVOPS is build upon four principals. They are the pillars of how the organization operates. Continuous learning is something that is common with the agile methodology. Feedback is also common to agile but in the case of DEVOPS, the feedback originates from the customer or end users and it is not just the internal feedback. Let's call the customer or end user the **consumer**. Continuous experimentation is common to agile as well, but with DEVOPS the participants are the consumers. As with Agile, flow and speed are necessary but with DEVOPS, flow is much wider. It starts at the inspiration of an idea, either from feedback or internally, and it must flow fast to the point where the consumer benefits.

It could be said that DEVOPS is some sort of Agile on steroids with wider scope and feedback loop. But once you start drilling into what this means, then you can not but realize that this is not just a choice of a development team but it is something that all of the organization is engaged with. For DEVOPS to work, traditional silos need to be broken and the established organization needs to be re-evaluated on every level.

## DEVOPS vs automation

If you would try to break down DEVOPS into a fluent process you would more or less end up with the following steps.

{% include figure image_path="/assets/images/posts/2018-09-12-devops-build-code-plan-monitor-operate-deploy-release-test.png" alt="devops-build-code-plan-monitor-operate-deploy-release-test" caption="DEVOPS flow" %}

Some of the steps of this process should be already present. To showcase the difference, I'll split the process into two groups, each representing established structures that you can identify with.

| Group | Purpose |
| ----- | ------- |
| **Dev**elopment | Someone has an idea and that becomes a feature. Then after planning it gets implemented, then it is built and then tested. when all goes well then it is packaged and released. This part is common with Agile. High performing teams have figured out that in order to optimize their flow, automation is necessary. Employing automation allows them to work more on features of the product and less on the repetitive tasks. |
| **OP**eration**S** | The release artifact is picked up and deployed in staging environment and eventually on production. Maintaining those environments is an ever lasting process and the best performing OPS teams have figured out that monitoring should be "automated" by usually using appropriate tooling. In non DEVOPS organizations, this group is either internal or external, depending on who is going to pick the release artifact |

Notice also that the best performing teams, employ tools and practices to automate and manage higher loads, avoid human errors and most importantly focus on what really matters. This means that automation, regardless of whether the DEVOPS culture is present, it is an enabler for efficiency. And because DEVOPS is all about efficiency across the organization, it means that it cannot be achieved without embracing automation everywhere. Simply put, there can be no DEVOPS without an automation mindset. That doesn't mean that DEVOPS is automation and in fact they are not even comparable. They just have a very tight relationship with each other.

I hope with the above, it becomes clear where the **DEV**elopment+**OP**eration**S**=**DEVOPS** confusion originates from. People just made the link between the two because it simply also fits. Then the "you build it you run it" slogan got in the mix. It doesn't take that much to go wrong after all. Obviously, as with automation, developing automation is not unknown to the DEVOPS culture but DEVOPS is far more than that.
{: .notice}


## What is the difference ?

Returning to the above mentioned flow, one would ask so what is so different with DEVOPS? Why should I break the silos?

The main difference is that in DEVOPS culture, the feedback and planning is much closer to the consumer, who as explained above represents the customer or the end user. The producer, that is the organization that develops and offers a software service, maintains a special kind of trust with the consumer of these services. This trust has a very important role to play in the development of the service for both parties.

- The producer leverages the consumer's trust to experiment and evaluate what the consumer really needs and uses that information to plans accordingly. This is another topic but to make it simple, consider feature toggling, especially when toggling is applies to small user groups. In a non DEVOPS oriented culture, that would be possible with user surveys etc, but often people don't really know what they want until they get the chance to use it. An experimental has so much more to offer in terms of feedback compared to any other theoretical survey, because it can't get more real.
- The consumer trusts the producer that this process is for his own good, has patience when something goes wrong because he knows it will be fixed soon enough and often is willing to provide feedback and even ideas. Not always will he/she like a feature, but when he feels heard he will actively express his opinion. He/She gets engaged with the evolution of his favorite service.

The incentives are simple:
- The consumer expects an ever improving product, while it often feels that the producer had read his mind. 
- The producer wants to deliver the best possible services to maintain loyalty and secure revenue. 

When something goes wrong, then as long as the producer can remove the problem fast, then it all works out for the best. People are more willing to offer trust to services that have a proven record of excellence, especially during difficult time, than with services that revealed their weakness and can't solve a problem fast. 

One of the reasons that problems take time to resolve are silos. They are after all created to provide isolation. Problem with isolation is that when the problem is big and beyond the control or understanding of one of the silos, then it takes time to mobilize and catch up and silos become a bottleneck. This lack of flexibility can cause irrevocable loss of precious trust between the producer and the consumer. Trust acts like credit and that is not indefinite. When trust is lost,  this very tight loop of continuous product improvement is impossible. For this reason, DEVOPS requires any organization to re-evaluate their structure and remove all high risk factors that could cause irrevocable damage. 

To my knowledge, this concept was introduced and used in the gaming industry where the fans where more than willing to be the beta testers because the cared a lot for the delivered game and the fun and they would have with it. They were so loyal or excited that they actually provided free services to the developer studio. With Internet services establishing themselves as a dominant part of our daily lives, this concept jumped from a particular fan base to casual users who looked into reaping the benefits of this new era. We all are part of the DEVOPS process of our favorite services, although most of us don't realize it. Just consider the rhythm of upgrading the Apps on our mobile devices and how excited we become when we realize a new feature suddenly became available on our favorite service. With our most favorite services, the developer has built up so much credit, that we are willing to look the other way around when things go wrong. We even justify to ourselves as this being normal and that all can make a mistake. While this not being incorrect, if you compare it with the traditional customer-developer contract based relationships, then you can not but realize how different it can be with a DEVOPS mindset when possible.

This is much easier when working with products provided as software as a service (SAAS) and much more difficult to employ with on premise deployments at a customer's location. It is not forbidding but it certainly will have impact on the above mentioned process. If you think about it, early beta testing for games was the equivalent of on customer premise deployments of this loop. It didn't always work out but for the games and producers that did manage the concept well and respected their beta/free loyal testers, it resulted in unforgettable experiences. They didn't just sell their games. They built up credit and loyalty with them, something that you can't buy with money.

## How to get started?

## Summary

The following are the most interesting points to take away:

- DEVOPS is not a skill. Instead, it is culture in an organization.
- DEVOPS is not automation. Automation is an enabler for DEVOPS. 
- DEVOPS is a completely different operational model. 
  - Trust with your customers or consumers is one of the most important factors of success.
  - Requires core changes in how the organization works.
- DEVOPS works best with products and especially products deployed with the SAAS model. Projects have an end date and as such they break the loop.
- DEVOPS can work with on promise deployments of products as well. 
  - Requires adaptation.
  - Requires alignment with your customers.
  - Trust is even more important and often goes beyond the signed contracts like a between gentlemen agreement.

And before signing off, here are some tips:

Start small, prove the success of the paradigm and then expand in the organization.
{: .notice--info}

DEVOPS requires 100% commitment and support by the leaders of the organization. Failure to understand and provide this, will most certainly result in big problems.
{: .notice--warning}

Thank you for reading this post. Having said that, if you really want to understand and embrace DEVOPS, then my advice is: Please forget this post, forget the Internet and **read a book** about the topic.
{: .notice}
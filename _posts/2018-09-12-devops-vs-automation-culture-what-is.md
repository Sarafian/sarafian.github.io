---
title: DEVOPS. What is it, why embrace it and what how it relates to automation
excerpt: DEVOPS has become the new hype word and very often misused. In this post, I'll explain what DEVOPS is, what it isn't and why it is often confused with automation.
tags: DEVOPS
categories: Coaching
---

During 2016 the word DEVOPS was hyping. At least this when I experienced through my professional engagements the hype. Everywhere it was about DEVOPS. You could not not hear about it. It became such a hype that it started appearing even within internal organization meetings and emails about culture and how to work.

What I found very strange about this, was that everyone seemed to have a difference of opinion about what it is with two main devisions. One that used the word in the context of organizations. The other one seemed to derive its understanding from something like lets develop/code operations.

As the difference is massive and since it is not the first time that the industry is massive misusing certain words out of their right meaning, I've decided to do some investigation. Internet is good but literature and books are much better. Unfortunately, the Internet is full with self opinionated and marketing oriented type of references. It is very convoluted and it is not easy to distinguish the truth. 

The situation brought memories from when agile was a hype word. How then everyone talked about this but few actually managed to do agile and yet alone understand it.

![The DEVOPS Handbook](https://www.safaribooksonline.com/library/cover/9781457191381/360h/){: .align-left}
So I decided to read a book about DEVOPS. Following twitter profiles that seem to know a bit or two about certain things, I noticed an excitement for [The DEVOPS Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/). After reading it, it became to clear to me that the Internet in this case acts a lot like fake news do. Partly correct to get readers buy in and excitement, but mostly wrong. So this post is about trying to help you understand what DEVOPS is and what it is not. I understand that as an blog post on the Internet, it falls into the same fallacy, I'm trying to avoid but this is the only means I have at this moment. 

Most of this blog originates from this book and from my experiences in organizations that claimed to do or understand DEVOPS and also from job advertisements for DEVOPS engineers.

## What is DEVOPS NOT?

First let's make two important clarifications of what DEVOPS **is not**.

- Not a skill. DEVOPS is not something you ask a person to possess as skill.
- Not **DEV**elopment+**OP**eration**S**.
- Not automation. Yes there is some relationship between the two but they are not a synonym.
- Not about the continuous integration nor the continuous deployment pipeline.

## Why DEVOPS?

Before I explain what DEVOPS is, let's see what the purpose of DEVOPS really is about. I'll use a quote from [The DEVOPS Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/) which I find that it offers the best motivation.

> DEVOPS benefits us all in the value stream, whether we are Dev, OPS, QA, InfoSec, Product Owners or customers. It brings joy back to developing great products, with fewer death matches. It enables humane working conditions with fewer weekends and missed holidays with our loved ones. It enables teams to work together to survive, learn, thrive, delight our customers and help our organization succeed.
{: .text-center}

Notice that this doesn't refer to words such as skill, automation, programmer or etc. Instead it focuses on **great product**, **organization** and **way of working**.
{: .notice}


## What is DEVOPS then?

DEVOPS is first and foremost a culture. It can't be otherwise if you understand the motivation to embrace it. It is a discipline within an organization that develops software. It is all about how everyone in an organization works and as such it applies to all members regardless of rank.

One of the most interesting summaries I've ever come across is by Adam Jacom ([@adamhjk)](https://twitter.com/adamhjk)) during [Chef Style DevOps Kungfu](https://www.youtube.com/watch?v=_DEToXsgrPc&feature=youtu.be) and he said:

> A cultural and professional movement, focused on how we build and operate high velocity organizations, born from the experiences of its practitioners
{: .text-center}

Notice that this doesn't refer to words such as skill, automation, programmer or etc. Instead, it focuses on the words **movement**, **culture** and **organization**.
{: .notice}

![devops-philosophy-culture-movement](/assets/images/posts/2018-09-12-devops-philosophy-culture-movement.png){: .align-right}
DEVOPS is all about having within an organization the philosophy, the culture and the movement. All are at the foundation of the organization and this means that the organization lives and breaths DEVOPS. Anyone who would be hired in such an organization, will operate within this understanding. He or she, will either contribute and flourish or fail like with any other rules of engagement. This is very important because this flow from the top to the bottom ranks and vice versa.

It is important to understand that especially during the transition period, it really needs to flow from the top as an example.
{: .notice}

![devops-principals-flow-feedback-continuous-learning-experimentation](/assets/images/posts/2018-09-12-devops-principals-flow-feedback-continuous-learning-experimentation.png){: .align-left}
DEVOPS is build upon four principals. They are the pillars of how the organization operates. Continuous learning is something that is common with the agile methodology. Feedback is also common to agile but in the case of DEVOPS, the feedback originates from the customer or end users and it is not just the internal feedback. Continuous experimentation is common to agile as well, but like with feedback the group that is part of the experimentation is the customer or the end users. As with Agile, flow and speed are necessary but with DEVOPS, flow is about a wider circle. It starts at the inspiration of an idea, either from feedback or internally, and it needs to flow fast and consistently up to the point that the idea is in use.

Some could say that DEVOPS is some sort of Agile on steroids with wider scope and feedback loop. But once you start drilling into what this means, then you can not but realize that this is not just a choice of development team but it is something like all of the organization is involved with. For DEVOPS to work, traditional silos need to be broken and the established organization needs to be re-evaluated on every level.

## DEVOPS vs automation

If you would try to break down DEVOPS into a fluent process you would more or less end up with the following flow.

{% include figure image_path="/assets/images/posts/2018-09-12-devops-build-code-plan-monitor-operate-deploy-release-test.png" alt="devops-build-code-plan-monitor-operate-deploy-release-test" caption="DEVOPS flow" %}

It actually doesn't matter where you start but suffice it say, that when adopting DEVOPS some of the parts of the chain should be already present. Let's start from planning and to build up the argument I'll split the process into two groups.

| Group | Purpose |
| ----- | ------- |
| **Dev**elopment | Someone has an idea and that becomes a feature. Then after planning it gets implemented, then it is built and then tested if all goes well then it is packages and released. This part is common with Agile and in fact with all development processes. High performing teams have figured out that in order to optimize the flow, automation is necessary so they get to work more on features and the business of the product and less on the repetitive tasks. |
| **OP**eration**S** | The next steps already exist but the main difference with DEVOPS is that they are usually executed by different groups internal or external. The release artifact is picked up and deployed in staging environment and eventually on production. Maintaining those environments is an ever lasting process and the best performing OPS teams have figured out that monitoring should be "automated" by employees tools that help do the job. |

I think it now becomes clear where the **DEV**elopment+**OP**eration**S**=**DEVOPS** confusion originates from. 
{: .notice}

Notice also that the best performing teams from each group are employing tools and in general automation to manage the load, avoid human errors and most importantly focus on what really matters. This means that automation, regardless of whether DEVOPS culture is present, is an enabler for efficiency. And because DEVOPS is all about efficiency across the organization, it cannot be done without embracing automation practices. It is simply impossible.

Simply put the relationship between DEVOPS and automation is tight but it is not the same and not even comparable. 

Automation is one of the tools that enable a DEVOPS culture.
{: .notice--info}

## What is different to development and operation silos?

Returning to the above mentioned flow, one would ask so what is so different with DEVOPS? Why should I break the silos? 

The main difference is that in DEVOPS culture, the feedback and planning is much closer to the customer and end user. 

- The producer leverages the consumer's trust to experiment and evaluate what does the consumer really need and plan accordingly. This is another topic but to make it simple, consider feature toggling, especially when toggling is applies to small user groups. 
- The consumer also trust the producer that this process is for his own good, has patience when something goes wrong because he knows it will be fixed soon enough and often is willing to provide feedback and even ideas. 

The above requires a high level of trust between the two involved parties. Both need and leverage this trust each for his own reasons
- The consumer expects an ever improving product, while it often feels that the producer had read his mind
- The producer wants to deliver the best possible services to maintain loyalty and secure revenue. 

When something goes wrong, then as long as the producer can remove the problem fast, then it all works out for the best. Silos have been identified to be the bottleneck to this process and thus a high risk factor when trying to build and maintain the trust. Without trust this very tight loop of continuous product improvement is impossible and employing DEVOPS requires any organization to re-evaluate their structure and remove all high risk factors that could cause irreplaceable damage. 

To my knowledge, this concept was first widely used by the gaming industry where the fans where more than willing to be the beta testers because the cared a lot for the delivered game and the fun and they would have with it. With Internet services establishing themselves as a dominant part of our daily lives, this concept jumped from a particular fan base to all users who looked into reaping the benefits of this new era. We all are part of this although often we don't realize it. Just consider the rhythm of upgrading the apps on our mobile devices and how excited we become when we realize a new feature suddenly became available on our favorite service.

For this reason, for DEVOPS to work it needs the target of a product to be engaged and part of it. This is much easier when working with products provided as software as a service (SAAS) and much more difficult to employ with on premise deployments at a customer's location. It is not forbidding but it certainly will have impact on the above mentioned process. If you think about it, early beta testing for games was the equivalent of on customer premise deployments of this loop. It didn't always work but for the games and producers that did, it resulted in unforgettable experiences and user loyalty.

## Summary

The following are the most interesting points to take away:

- DEVOPS is not a skill. Instead it is culture in an organization.
- DEVOPS is not an automation. Automation is an enabler for DEVOPS.
- DEVOPS is a completely different operational model. 
  - Trust with your customers or consumers is one of the most important factor for success.
  - Requires core changes in how the organization works.
- DEVOPS works best with products and especially products deployed with the SAAS model.
- DEVOPS can work with on promise deployments
  - Requires adaptation.
  - Requires alignment with your customers.
  - Trust is even more important.

And before signing off, here are some tips:

Start small, prove the success of the paradigm and then expand in the organization.
{: .notice--info}

DEVOPS requires 100% commitment and support by the leaders of the organization. Failure to provide this, will most certainly result in big problems.
{: .notice--warning}

Thank you for reading this post. Having said that, if you really want to understand and embrace DEVOPS, then my advice is: Please forget the internet and read a book about it.
{: .notice}
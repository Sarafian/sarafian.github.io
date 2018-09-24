---
title: A high-level overview of DevOps
excerpt: After reading a book about DevOps, it became clear to me that DevOps has been the new hype word and very often misunderstood and misused.
Tags:
- DEVOPS
categories:
- Culture
---

During 2016 the term DevOps was a hype word. At least this how I experienced it then through my professional engagements. Everywhere and anywhere, somehow you would hear about DevOps. It became such a hype, that it was always part in of those daily, weekly or monthly internal emails about company culture etc.

What I found very strange, was that everyone seemed to have a different understanding. The understandings seemed to be grouped around two groups. There was one, that used the term in the context of organizations. And there was another which seemed to derive its understanding from something like "let's develop/code operations".

The difference is significant and since it is not the first time that elements in this industry are using words wrong, I've had decided to do some investigation. With the internet being very confusing, I decided to look into literature, since books are much better in building the foundations around a concept. Unfortunately, the Internet is full of the self-opinionated and marketing oriented type of content and often it is not easy to distinguish and extract the truth. Proper research which is required to write a book is always irreplaceable. This situation brought back memories from when agile was a hype in its time. Then everyone talked about it but few actually managed to do agile, let alone understand it. But you would see job advertisements such as Agile engineer or SCRUM XYZ developer, similar to DevOps engineer. And all have one thing in common, that is they don't mean anything.

So this post is about trying to help you understand what DevOps is about and what it is not, as digested by me reading [The DevOps Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/) and compared with my experiences. I understand the controversy of me contributing yet another analysis to the internet, so I already suggest you read a book about this topic, regardless of whether you chose to continue reading this post.

## The DevOps Handbook

![The DevOps Handbook](https://www.safaribooksonline.com/library/cover/9781457191381/360h/){:.align-left}
Following some twitter profiles that seem to know a bit more about certain things, I noticed much excitement for [The DevOps Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/) which was getting released in October 2016. So I decided to buy it. After reading it, it became so clear to me that the Internet, in this case, acts a lot like fake news. Partly correct to get readers a buy-in and excitement, but often wrong as well. The book goes over the history and explains concepts around DevOps. How did the industry start almost 20 years ago envisioning such a setup and how some groups started taking the first small steps. After the introduction, the book refers to and analyzes case studies, to help showcase what DevOps has to offer and how it was adopted in each organization. 

## What is DevOps NOT?

When discussing with others about DevOps, I find it very useful to first clear the air from the usual misunderstandings. So these are what DevOps **is not**:

- Not a skill. DevOps is not something you ask a person to have as skill.
- Not **DEV**elopment+**OP**eration**S** just because it seems to fit right.
- Not automation. Yes, there is some relationship between the two, but they are not a synonym.
- Not about the continuous integration nor the continuous deployment pipeline. This quite often the problem with job advertisements.

## Why DevOps?

It is also very useful to understand the purpose of DevOps because the motivation sets the foundation for a better apprehension of the meaning and goal of DevOps. I'll use a quote from [The DevOps Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/) which I find that it offers the best summary of motivation.

> DevOps benefits us all in the value stream, whether we are Dev, OPS, QA, InfoSec, Product Owners or customers. It brings joy back to developing great products, with fewer deathmatches. It enables humane working conditions with fewer weekends and missed holidays with our loved ones. It enables teams to work together to survive, learn, thrive, delight our customers and help our organization succeed.
{:.text-center}

Notice that this doesn't refer to words such as skill, automation, programmer or etc. Instead, it focuses on concepts such as **great product**, **organization** and **way of working**.
{:.notice}

## What is DevOps then?

DevOps is first and foremost about culture. Having read the summary above, it can't be something personal. It needs to be bigger than the individual or a team of developers. It is a discipline within an organization that develops software. It is all about how everyone in the organization works and as such it applies to all members regardless of rank.

One of the most interesting summaries I've ever come across is by Adam Jacob ([@adamhjk)](https://twitter.com/adamhjk)) during [Chef Style DevOps Kungfu](https://www.youtube.com/watch?v=_DEToXsgrPc&feature=youtu.be) and he said:

> A cultural and professional movement, focused on how we build and operate high-velocity organizations, born from the experiences of its practitioners
{:.text-center}

Notice that this also doesn't refer to words such as skill, automation, programmer or etc. Instead, it focuses on concepts such **movement**, **culture** and **organization**. It is also in full alignment with the historical analysis provided in the [The DevOps Handbook](https://www.oreilly.com/library/view/the-devops-handbook/9781457191381/).
{:.notice}

![devops-philosophy-culture-movement](/assets/images/posts/2018-09-14-devops-philosophy-culture-movement.png){:.align-right}
Philosophy, culture and movement are the building blocks of DevOps and one is pushing the other constantly. The fundamental in an organization and this means that the organization lives and breathes DevOps. Anyone who would be hired in such an organization, will operate within this construct even he has never had done so. He or she, will either contribute and flourish or fail. DevOps flows from the top to the bottom ranks and from the bottom to the top.

When an organization decides to embrace DevOps, then during the transition period it must flow from the top ranks more that it flows from the bottom. The reason is discussed further below.
{:.notice}

![devops-principals-flow-feedback-continuous-learning-experimentation](/assets/images/posts/2018-09-14-devops-principals-flow-feedback-continuous-learning-experimentation.png){:.align-left}
DevOps is built upon four principals. They are the pillars of how the organization operates, and they are not that different with Agile. Continuous learning is something that is common with the agile methodology. Feedback is also common to agile but in the case of DevOps, the feedback originates from the customer or end users and it is not just about internal feedback. For convenience, I'll refer to the customer or end user as the **consumer** and the developing party as the **producer**. Continuous experimentation is common to agile as well, but with DevOps, the participants are often the consumers. As with Agile, flow and speed are necessary but with DevOps, the scope is much wider. It starts with the inspiration of an idea, either from feedback or internally, and it must flow fast to production.

It could be said that DevOps is some sort of Agile on steroids with a wider scope and feedback loop. But once you start drilling into what this means, then you can not but realize that this is not just a choice of a development team but it is something that all the organization is engaged with. For DevOps to work, traditional silos need to be broken and the established organization needs to be re-evaluated on every level.

## DevOps vs automation

If you would try to break down DevOps into a fluent process you would more or less end up with the following steps.

{% include figure image_path="/assets/images/posts/2018-09-14-devops-build-code-plan-monitor-operate-deploy-release-test.png" alt="devops-build-code-plan-monitor-operate-deploy-release-test" caption="DEVOPS flow" %}

For most organizations, some of the steps are already present but there is a difference. To help showcase it, I'll split the process into two groups, each representing established structures that you can identify with.

| Group | Purpose |
| ----- | ------- |
| **Dev**elopment | Someone has an idea and that becomes a feature. Then after planning it gets implemented, then it is built and then tested. When all goes well then it is packaged and released. This part is common with Agile. High performing teams have figured out that in order to optimize their flow, automation is necessary. Employing automation allows them to work more on features of the product and less on the repetitive tasks. |
| **OP**eration**S** | The release artefact is picked up and deployed in a staging environment and eventually on production. Maintaining those environments is an everlasting process and the best performing OPS teams have figured out that monitoring should be "automated" by using appropriate tooling. In non-DevOps organizations, this group is either internal or external, depending on who is going to pick the release artefact |

Notice also that the best performing teams, employ tools and practice automation techniques to manage higher loads, avoid human errors and most importantly focus on what really matters. This means that automation, with or without DevOps culture, it is an enabler for efficiency. And because DevOps is all about efficiency across the organization, it means that automation is key for success. Simply put, there can be no DevOps without an automation mindset. That doesn't mean that DevOps is automation and in fact, they are not even comparable. They just have a very tight relationship with each other.

This explains where the **DEV**elopment+**OP**eration**S**=**DEVOPS** confusion originates from. People just made the link between the two because it simply fits. Then the "you build it you run it" slogan got in the mix. And it is catchy and also clicks. It doesn't take that much to go wrong after all.
{:.notice}


## What is the difference?

Returning to the aforementioned flow, one would ask so what is so different with DevOps? Why should I break the silos?

The main difference is that in DevOps culture, the feedback and planning are much closer to the consumer. The producer, that is the organization that develops and offers a software service, maintains a special kind of trust with the consumer of these services. This trust has a very important role to play in the development of the service for both parties.

- The producer leverages the consumer's trust to experiment and evaluate what the consumer really needs and uses that information to plan accordingly. This is another topic but to make it simple, consider feature toggling, especially when toggling is applies to small user groups. In a non DevOps oriented culture, that would be possible with user surveys etc, but often people don't really know what they want until they get the chance to use it. An experiment in the field has so much more to offer in terms of feedback compared to any other theoretical survey, because it can't get more real than that.
- The consumer trusts the producer and understands that this process is for his own good. He is patient when something goes wrong because he knows that it will be fixed soon enough. Often he/she is willing to provide feedback and even ideas. Not always will he/she like a feature, but when he feels heard he will actively express his/her opinion. He/She gets engaged with the evolution of his/her favourite service and builds up loyalty.

The incentives are simple:
- The consumer expects an ever improving product, while it often feels that the producer had read his mind. 
- The producer wants to deliver the best possible services to maintain loyalty and secure revenue. 

When something goes wrong, then as long as the producer can remove the problem fast, then it all works out for the best. People are more willing to offer trust to services that have a proven record of excellence, especially during difficult times, than with services that revealed their weakness and can't solve the problem fast. 

One of the reasons that problems take time to resolve is silos. They are after all created to provide layers of isolation and protection. Problem with isolation is that when the problem is big and beyond the control or understanding of one of the silos, then it takes time to mobilize and catch up and silos become a bottleneck. This lack of flexibility can cause irrevocable loss of precious trust between the producer and the consumer. Trust acts like credit and it is not indefinite but most importantly it extremely difficult to regain when lost. Furthermore, when trust is lost, it is like a key ingredient is missing. For this reason, DevOps requires an organization to re-evaluate their way of working and remove all high-risk factors that could cause irrevocable damage. 

To my knowledge, this concept was introduced and used in the gaming industry where the fans were more than willing to be the beta testers because they cared a lot for their precious game. They were so loyal or excited that they actually provided free services to the developer studio. With Internet services establishing themselves as a dominant part of our daily lives, this concept jumped from a particular fan base to casual users who looked into reaping the benefits of this new era. All of us are part of the DevOps process of our favourite services, although most of us don't realize it. Just consider the rhythm of upgrading the Apps on our mobile devices and how excited we become when we realize a new feature suddenly became available from our favourite service. Consider as well, the disappointment when a beta feature that looks interesting is not available to us. There is so much loyalty built up, that we are willing to look the other way when things go wrong. We even justify this to ourselves, as this being normal and that everyone can make a mistake or problems are unavoidable. While this is not incorrect, if you compare it with the traditional customer-developer contract-based relationships, then you can not but realize how different it can be with a DevOps mindset when possible.

This is much easier when working with products provided as software as a service (SAAS) and much more difficult to employ with one premise deployments at a customer's location. It is not forbidding but it certainly will have an impact on the aforementioned process. If you think about it, early beta testing for games was the equivalent of the customer on-premise deployments. It didn't always work out but for the studios and producers that did manage the concept well and respected their beta/free loyal testers, it resulted in unforgettable experiences and fan base. They didn't just sell their games. They built up credit and loyalty with them, something that you can't buy with money.

## How to get started?

Quite often, the people I meet consider their product's technical debt to be their biggest problem and quite often a unique situation. I feel that this is incorrect. Given the willingness to resolve the issue, reducing the technical debt is a matter of time, which in essence translates into cost spent with money. Technical dept usually implies knowledge of what is wrong which implicitly means there is a known solution to the problem. So, it is only a matter of commitment and investment to reduce or remove this dept.

What is often forgotten is the culture that had led to the situation. Like with all other human activities, software is developed in a manner that represents the dominant culture in an organization. And it's not just about the development but the total culture of how the organization operates. You clearly recognize the priorities, past and present. The validity of a culture is something that changes over time. The unfortunate attribute of culture is that it often fails to realize when it becomes irrelevant or obsolete, simply because times change.

Culture is the most difficult to change in an organization because it relates to human attributes that are the most resilient to change, like for example habits. It is not uncommon to enter meetings where every suggestion for improvement or change gets quickly dismissed based on an argument that is abstract, theoretical or historical. Probably this has happened so many times, that this counter-argument is so polished and shines the most. Even if you decide to try to win the argument, you will rarely succeed because everyone is fine-tuned to operate in a manner that protects the established culture, in the same way, we humans tend to defend our bad habits. The resistance is even stronger with people from senior members and it is understandable. They feel they took part in building this culture which they consider successful. Most probably, it was indeed successful in the past, but they usually fail to realize that the world has moved on. As with the rest of their lives, they are not keen on changing and learning new ways of working, so any alternative suggestion feels like a threat to the status quo, and they react accordingly. To set the record straight, there are plenty of young people who behave like that as well but as we humans grow old, we tend to learn new things more with more difficulty.

In this kind of situations, the only way to have a meaningful discussion is to pre-emptive remove non-relevant and fancy counter-arguments. To do this, you need basic proof that automatically dismisses any theoretical argument fueled by the momentum of resistance. I once read the following which sounds harsh but it is really on the spot for these type of situations.

> Don’t fight stupid, make more awesome
{:.text-center}

By Jesse Robbins [@jesserobbins](https://twitter.com/jesserobbins)
{:.text-right}

DevOps is a culture and to embrace it, change in the organization is required and that is going to be extremely difficult. Even if the higher management understands DevOps and has fully committed to this, just asking people to change won't work. You need proof to help fend off all non-relevant negative energy and focus on moving forward. In the DevOps literature, a small change is suggested to be the best practice for adopting DevOps. You usually start with a small team that will apply the concept to one specific part of the organization. It is not going to be 100% perfect, but this group acts as a proof of concept. It shows how it can be done. They are explorers. Most importantly, the group provides hard and tangible proof about the merits of DevOps. This naturally means that all is measured and all are monitored.

The chosen task for this small dedicated team needs to be **impressive**. It can be something that most consider an impossible task or something that is completely broken in the organization and it is stuck in a dead end. The team must regularly share their progress and more importantly, show how they actually address the issues. They need to showcase that although it is a difficult task, it is not an impossible one by being honest and admitting to their high but also low points. Overall, their commitment and resolve are all that matters, and those they need to share on every occasion.

For this to work this team needs:

- to consist of members that are "believers". They are driven and it is not in their culture to stop.
- there are no silos. The team acts like one and it is not interested in any sort of finger-pointing. They all succeed and they all fail together.
- clear and transparent support from the very top of the organization. 

The last one is, in my opinion, the most important one. If the management is not fully committed, if the management decides to back down on first signs of trouble that will only deliver the opposite message, which is to stay in the current culture. And such a behaviour of management is usually the historical explanation of why an organization has failed to evolve with time.

## Technical excellence

One big requirement for DevOps to succeed is that it requires technical excellence. Building up and maintaining such a high level of trust with the consumer, requires talent, skill and passion.

**Technical Excellence** is the ability to foresee and eliminate an issue before it has the potential to jeopardize safety, schedule, budget, quality, and most importantly, client satisfaction.
{:.notice--info}

We will all make mistakes at some point and that is unavoidable. Our ability to protect ourselves from hiccups and our behaviour when all prevention measures have failed are what matter the most. 

Most managers won't like the following, but going cheap will only create problems. When left unchecked, it will lead to serious issues. When trust is the foundation of your process, you can't jeopardize it with mediocrity. Instead, managers need to find a way to monetize the benefits of DevOps and pay for the cost of the required talent. Organizations that realized this first and made the proper investments, now dominate their industry. Organizations that haven't, need to adopt only to stay in the game and to avoid becoming completely obsolete.

## Summary

The following are the most interesting points to take away:

- DevOps is not a skill. Instead, it is a culture in an organization.
- DevOps is not automation. Automation is an enabler for DevOps. 
- DevOps is a completely different operational model. 
 - Trust with your consumers is one of the most important factors for success.
  - Requires core changes in how the organization works.
- DevOps works best with products and especially products deployed with the SAAS model. Projects have an end date and as such, they break the loop.
- DevOps can work with on promise deployments of products as well. 
 - Requires some adaptation in the process.
  - Requires alignment with your customers.
  - Trust is even more important and often goes beyond the signed contracts, similar to a gentlemen's agreement.
- DevOps requires technical excellence. Without it, you are just looking for trouble.  
- DevOps is not cheap but it pays out.
- When starting with DevOps, take one small step at a time.

Most importantly of all

DevOps requires 100% commitment and support by the leaders of the organization. Failure to understand will most certainly result in big problems.
{:.notice--warning}

Thank you for reading this post. Having said that, if you really want to understand and embrace DevOps, then back to my original advice: Please forget this post, forget the Internet and **read a book** about the topic.
{:.notice}

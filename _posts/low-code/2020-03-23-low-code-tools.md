---
excerpt: I've always had reservations for such tools and decided to take a look into this world of low (no) code tools. If you are looking for a light weight look into Buble, AirTable, Adalo, dashdash, Zapier, Integromat, chatfuel, ManyChat and others from a software engineer's perspective, then this is your post.
tags: 
    Automation
    Low-Code
categories: Low-Code
title: Low (No) Code tools from a software engineer's perspective

---

## Overcoming initial reservations

Full disclaimer here, my background in software engineering made me always suspicious about such type of tools or accelerators. Historically, we had plenty of them and they all performed the necessary function but with a huge price to pay in the long term. Who doesn't remember applications built in Access or even fancy Excel sheets driving big organization businesses? Anyone who has worked in a big organization has noticed a big amount of work being done in Excel sheets that are moved around by emails. But the temptation is big because it looks so easy at the beginning but it is also very easy to move from a convenience to a disaster. Applications in Excel sheets don't scale. We had to wait for Google Docs to show to the world on how to collaborate on the same resource online and that is still tricky. Microsoft's Office 365 collaboration is possible but still not very functional and the reality is that many people haven't picked up on the idea. Just mentioning the concept is for most like rocket science and this is not a technical topic but merely one of efficiency and productivity. Then we had the rise of the workflows with all the promises of *business* people configuring and running their processes without the necessity of the *developers* but we all know how that went as well. At the moment, some basic logic needed to be implemented, the *developer* was necessary which kind of beats the purpose. Yes it takes longer to setup an actual application but the longer term benefits are well worth the effort.

So, if I'm so negative about such tools why this blog post? Some time ago I started following the podcasts of [Software Engineering Daily] and one of the most interesting ones I listened to was [Makerpad: Low Code Tools with Ben Tossell]. I was impressed to be honest because the host [Jeff Meyerson] who expressed similar reservations to mine seemed to be at least intrigued and towards changing his mind. So I decided to look deeper into this.

Please keep in mind that this blog post is written by a person of strong technical background and it oriented towards an audience with similar understanding. Non technical people, might get confused and I'm sure that other articles would be more appropriate. Still, if you want to read the side of the story of those who build software applications, then by all means please read this post as well because it will provide some interesting remarks about the limitations of these tools from a long term and technical standpoint. I'm not going to go heavy on the technical so it won't be difficult to follow.

## Quick and dirty investigation

Initially, I found two very nice links that provide a list and a summary of each service

- [30 no-code and low-code tools for your next project]
- [Every Tool You Need to Build Without Code]

Some of the tools are known. For example [WordPress] is a web site development and hosting tool that has a long and lasting history, starting from a blog engine and evolved to include more versatile flows.

## My discovery and filtering process

After reading a bit on each service's website, watching some [YouTube] videos when available, I've kept only the ones that I've found the most interesting to do a deeper check. 

But each person has different drivers and I want first to explain my drivers and conditions that led to this smaller *filtered* list.

- Genuine curiosity. This is my strongest driver of them all. Note to myself, I should be doing this more frequently.
- At the aviation-related organization where I work there is some discussion about automating certain flows of different departments. While all fancy words are thrown into the mix, the typical culture is to buy a big fancy tool when simpler solutions or a bit of code could just suffice. After all, there are so much plethora of fancy services, that the hardest parts of the flow are available as a SAAS and only some glue code is required. The constant problem is that nobody wants to see a console to help make the automation simple.
- Recently I thought of an application that is missing. Like discussed in the podcast, I'm not keen on developing the entire stack and if there would be something that could help me prototype then that would be the best.
- In the past, I had used some PowerShell automation to develop a website for cheap weekend escapes in Europe with [RyanAir]. I've discussed this in a previous [post]({% link _posts/2016-06-27-RyanAirWeekend.md %}). For various reasons that are related with restrictions from [RyanAir]'s API, this stopped being functional but if these *low code* tools can help then why not.
- This became important while already in my discovery process. Without going into much detail, because of the [Coronavirus disease 2019] I want to get to somehow in my cell phone notifications of specific emails and calendar events.
- For a long time now I've been contemplating with the idea of having a chat bot representative of myself for recruiters. This could help both of us spare time but I'm not sure the culture is there for Belgium. Regardless, this is still on mind and I would like to give it a try.

During this process, my main condition was wether the tool or service has a free pricing tier. I quickly noticed that these tools can be very expensive and I'm not going to do full registrations for just exploring them. Also, although this didn't drive my filtering process, I started looking into whether the service provides support for automation pipelines like [Zapier]. Later on, I discovered [Integromat] as well.

## The tools and services of interest

At the moment of writing this post, I've not investigated all the services mentioned here. But, I've seen enough to know what to expect. Below, I'll split the tools into groups of how I understand with a technical background and share some insight and comments

**WebSite builders**

This category is about developing a website with visual flows.

| Name | Observations |
| ---- | ------------ |
| [Bubble] | Builds web and mobile applications without code. Looks very powerful and requires a lot of digging around |
| [WIX Corvid] | An open development platform that lets you build advanced web applications with a visual editor. I haven't checked it yet but from what I read is very good as well. |

Everything you would expect is supported by this applications. Form validations, grids, connections with data stores, authentication and authorization. It's all there including a quite a lot of options for design.

**Mobile app builders**

Nowadays, there is little difference between a website and a mobile application. Still, the ability to produce an application that is published for Android or IOS is quite the differentiator.

| Name | Observations |
| ---- | ------------ |
| [Bubble] | Builds web and mobile applications without code. Looks very powerful and requires a lot of digging around |
| [Adalo] | Builds mobile application following the flow of a slide deck. Restricted and focused on creating very presentation-oriented mobile applications. Think of basic mobile applications that exist for marketing purposes or just to provide a presence and an entry into your brand. |
| [Glide] and [Sheet2Site] | I've not worked with them yet, but these tools seem to offer an even simpler case of [Adalo], where instead of slide-show driven flow, the app will present based on a template, items from a Google spreadsheet. Add picture links on the sheet and you can have a very nice look. |

**Database like**

Although the tools themselves don't identify themselves as databases, in my view they are or all about data with some very interesting extra features.

| Name | Observations |
| ---- | ------------ |
| [Airtable] | This the closest to a database concept with rules of validation for columns and linking entities. On top, it can create very interesting and fancy views of the data. It offers API to interact with the data. As far as I've seen it only supports integration with [Zapier] |
| [dashdash] | This is a spreadsheet on steroids. While on **beta** at this moment of writing this post, what it does is that it can enhance or react upon a column on a row by driving an external API. It can also create the sheet by expanding on the results of an API call. Interesting and very focused for specific needs |

**Automation workflows**

These tools are all about creating flows that react on some external stimulus, do some data processing and then usually push that data to other external services, very similar to the traditional concept of workflows. For example, you can react upon a tweet, run it through a sentiment analysis engine and notify the appropriate person or group.

| Name | Observations |
| ---- | ------------ |
| [Zapier] | Probably the most mature service of them all. It is the only one that has integrations that allow the flow or zap, as they call it, to execute as a reaction to external events. Developers would know this pattern as a webhook integration which is used quite a lot in the CI/CD world. |
| [Integromat] | Very similar to [Zapier] and looks more elegant and elaborate. It allows conditional flows in a bit different way. At this moment all flows are scheduled on intervals. |
| [ActionDesk] | I've not checked this yet because I'm still waiting for admission but from what I read this is supposed to be a child of [Airtable] and [dashdash] |

What I found limiting on both tools is that they are very simple flow oriented. Normal workflows allow conditional and loop patterns but these tools don't. This could be to avoid driving non-technical profiles away.

**Notebook style collaborations**

Both [Notion] and [Coda] allow the mix of *functional* with text. You can create wiki-like content, planning, to-do lists etc. The difference with [Airtable] is that these tools are more focused on the mix and the page and the data is there just for the embedding. They are quite powerful and act as a self-contained application.

They remind me a bit of the concept of a [Jupyter Notebook]where the mix of text and execution blocks from code is amazing. Opem the notebook, run it and voila you get some very rich page with dynamic table and charts to share and print. With these notebooks, the code is very visible and scary for many people but when I spoke with [dashdash], they showed me an upcoming feature that resembles such a functionality. I think if [Notion], [Code] and [dashdash] could provide such a feature, where instead of python code you just see and interact with a grid or chart then this could be a killer feature in my opinion.

**Chatbots**

If you have tried creating a chatbot then you know that this is not easy when it needs to be proper. It is not just that the logic is difficult but also that hosting the bot can be also very difficult with Facebook's messenger being the most demanding in my opinion to a fault. These tools allow a person to develop a chatbot application in a similar way to creating a website with e.g. [Bubble] but now instead of screens, you get dialogues.

Like [Bubble] and [WIX Corvid], [ManyChat] and [chatfuel] can go deep in creating advanced functionality so I skipped this for now. What I ended up using them for is to send messages to messenger to use as notification on my cell phone. When I tried creating my application on Facebook to integrate with [Zapier] I kind of gave up and I found these tools to be a simpler alternative.

## Drawbacks

To my knowledge, none of these applications has this concept of change management and releases. I believe [Bubble] has this concept, where you have a developer version website to experiment and the actual *production* one. Still, the process of change management, that is being able to review the changes while implementing features, is not there. Maybe it would be too scary for some people but in my opinion, this is a very important feature when collaborating. 

I've also noticed that many of these services can become expensive especially if you need to combine some of them. But this is something to be evaluated on a case by case basis. Like many accelerators, you get what you want fast and cheap but in the long term you might end up paying double and triple the cost.

## Low-Code or No-Code

There seems to be a confusion about whether to use the terms `low-code` or `no-code`. Although the marketing is oriented towards people who don't want to see any code, most of these tools require some basic understandings of how code works and hence the proper term is `low-code`. It is not that one needs to understand code but it is good that some very programming principles like conditions, loops, variables, assignments, HTTP, rest, authentication, authorization etc are understood. Things have changed, and I'm sure that younger people are not so alienating or dismissing to these basic concepts because they are very much familiarized with technology since children. This is also something to keep in mind when trying to understand for who these applications and services are primarily intended for.

## Conclusion

I have to admit, I'm convinced. Not entirely but let me elaborate.

With proper understanding the scope and limitations of these tools you can have some serious things created. You don't have to pay for marketing apps if you have some creativity and you can have a basic interaction through a chat interface. What troubles me the most is the lack of change management. Maybe it's the software engineer inside me that is so concerned. For most basic applications that are oriented to serve information and help the brand grow, this would be an overkill. But when teams are involved and when bidirectional flows are implemented, then I'm concerned. Not sure if I would ever run serious production loads on the likes of [Bubble], [AirTable], [ManyChat] and [Zapier]. I would still advice to use these tools though for rapid prototyping. It is just that I know from experience, how a monster can be created by such tools when the prototype becomes the workhorse. If your company is your one-man show, then don't look elsewhere. There is incredible power here.

[Software Engineering Daily]: https://softwareengineeringdaily.com/2020/02/27/makerpad-low-code-tools-with-ben-tossell/
[Makerpad: Low Code Tools with Ben Tossell]: https://softwareengineeringdaily.com/2020/02/27/makerpad-low-code-tools-with-ben-tossell/
[Jeff Meyerson]: https://twitter.com/the_prion?lang=en.
[30 no-code and low-code tools for your next project]: https://makermag.com/30-no-code-tools/
[Every Tool You Need to Build Without Code]: https://www.integromat.com/en/blog/2019/11/18/every-tool-you-need-to-build-without-code/
[WordPress]: https://wordpress.com/
[YouTube]: https://www.youtube.com/
[RyanAir]: https://www.ryanair.com/
[Zapier]: https://zapier.com/
[Integromat]: https://www.integromat.com/
[Coronavirus disease 2019]: https://en.wikipedia.org/wiki/Coronavirus_disease_2019
[Bubble]: https://bubble.io/
[WIX Corvid]: https://www.wix.com/corvid/
[Adalo]: https://www.adalo.com/
[Airtable]: https://airtable.com/
[dashdash]: https://dashdash.com/
[ActionDesk]: https://www.actiondesk.io/
[Notion]: https://www.notion.so/
[Coda]: https://coda.io/
[Jupyter Notebook]: https://jupyter.org/
[ManyChat]: https://manychat.com/
[chatfuel]: https://chatfuel.com/
[Glide]: https://www.glideapps.com/
[Sheet2Site]: https://www.sheet2site.com/

---
excerpt: If you need a bot for your personal home automation, then this post describes how to achieve what is perceived as a private bot on Facebook's messenger platform.
tags: 
    Chatbot
    Low-Code
    Facebook
    Messenger
categories: Low-Code
title: Creating a private Facebook Messenger chatbot

---

In this post, I want to explain how to create a private bot on [Facebook]'s [Messenger] platform. 

## Not exactly a private bot

With *private*, I'm referring to bots that are not possible to be found by others or won't interact with others. These sort of bots are meant usually for some personal automation, where the bot act as the delivery entity to you or your family.

On my previous post [Creating a private Telegram chatbot]({% link _posts/2016-06-27-RyanAirWeekend.md %}), a comment was raised on the [dev.to] platform about the "private" nature of [Telegram]'s bots. With private, I'm referring to a bot that though not private can help establish your very own private communication channel. You control who can interact with the bot and who not. If the bot is discoverable, then there is no functionality supported to others and most importantly, your communication is only visible to yourself or others you have invited.

## Creating a Facebook page

On Facebook, everything is driven by a page. Organizations will create an organization page but as many do, we can also create own small pages. A page is **always required** for a [Messenger] bot to work. The page serves its purpose to establish an entry point of interaction with other parties of interest but it is also the entry point for any chat related communication and this is where the bot needs to plug-in. So for the purpose of this post, the page needs to be something very simple.

1. Navigate to [Facebook Pages].
1. Click on `Create a page` and `Community or Public Figure` and `Get Started`.
1. Name the page `Example Private Messenger Bot` and as category choose something appropriate e.g. `Personal Blog`.
1. You can upload pictures for the page if you want to but it is not necessary.
1. The page should be ready like this [Example Private Messenger Bot]. Notice that the page can be discovered by others. Confirm this by navigating to your page with an incognito tab.
1. Go to `Setting->General->Page Visibility` and select `Page unpublished` and chose `Other` for reason with e.g. `Personal Bot`. Notice that the page is not available any longer on the incognito tab.
1. From `Setting->Messaging` use the `Your Messenger URL` to initiate a chat session with your page. This is the *private* session we need with our chatbot.

## Creating the bot

Developing an actual bot on [Messenger] is quite complicated. You would need to create an application, submit policies to be able to get keys and tokens that would allow you to integrate with the page's application. For this reason, we'll keep this simple and use a service that makes things very simple. There are three options in order of complexity:

1. [chatfuel]
1. [ManyChat]
1. [Dialogflow]

Given that the purpose here is to create a simple *private* [Messenger] bot, I'll choose [chatfuel] for the purpose of this post. Just select `Add blank bot` on [chatfuel] and let's name it `Example Private Messenger Bot` as well.

The bot is now available and you can try a test chat with it by activating the `Test Your Bot` from the `Automate` panel within the bot's configuration.

## Connecting the bot to your page

This is not necessary, but it helps that you can discover your bot from [Messenger] or [Facebook] through the page we just created. If this is your first time in [chatfuel] then this is straight forward when connecting first time [chatfuel] with [Facebook]. The trickier flow that I will explain is when you've already made this connection and you need to find the new page. I've chosen this flow because I ran into this problem when writing this post. I've already have another bot connected with another [Facebook] page and I couldn't find how to connect the `Example Private Messenger Bot` to its respected page.

1. Navigate to [Facebook's Business Integration].
1. Select `View and edit` of the `Chatfuel` application.
1. Scroll down and select the `Example Private Messenger Bot` on each of the following sections:
  1. `Manage your Pages`.
  1. `Manage and access Page conversations in Messenger`
  1. `Send messages from Pages you manage at any time after the first user interaction`
  1. `Show a list of the Pages you manage`
  1. `Log events on your Page's behalf and send those events to Facebook`
1. Save.
1. Wait for a short time and then refresh [chatfuel]. If this doesn't work try to logout and login again.
1. Enter the `Example Private Messenger Bot` and select the `Configuration` panel from the left.
1. Notice that the `Example Private Messenger Bot` is listed with a `Connect to Page` button to the right which you activate.

That's it.

## Integrating with Zapier

On my previous post [Creating a private Telegram chatbot]({% link _posts/2016-06-27-RyanAirWeekend.md %}) I've explained how to send messages to the bot. This allows for any integration with e.g. [Zapier] or [Integromat]. To demonstrate the ability to send messages to the `Example Private Messenger Bot` I'll show how to hook it up with [Zapier] which offers an integration. To keep it simple and focused on [chatfuel] and this post, I'll use a two-step zap.

1. `Schedule by Zapier` configured to activate every hour.
1. `chatfuel` configured to target a session with the `Example Private Messenger Bot`.

The `Schedule by Zapier` configuration is very easy and straightforward. [Zapier] is good at helping configuration with UI navigation. The `chatfuel` configuration is the trickier one and it compromises by two activities in the overall zap flow.

- Find a Subscriber in Chatfuel
- Send Text Message in Chatfuel

It will look like this:

![2020-03-27-create-private-facebook-messenger-chatbot-zap](/assets/images/posts/low-code/2020-03-27-create-private-facebook-messenger-chatbot-zap.png "2020-03-27-create-private-facebook-messenger-chatbot-zap")

To configure the `Find a Subscriber in Chatfuel` follow these steps:

1. Type or select `chatfuel`.
1. For the `Choose Action Event`, scroll down and select `Find a Subscriber`. This step is important and retrieves a value that helps target the session to send the message later on. In more advanced flow, this value is driven as a reaction to an incoming trigger.
1. For the `Chatfuel account` select `Add a New Account` which will open a page and there select the `Example Private Messenger Bot` to connect with.
1. For the `Search Type` there are multiple options. Let's choose the `Search by {{messenger user id}}` which is the [user page-scoped ID (PSID)].
1. For the `User ID` follow these steps to retrieve it:
   1. Make sure you have exchanged one or two messages with the bot.
   1. Navigate to [chatfuel]'s `Example Private Messenger Bot`'s page.
   1. Open the `People` panel and you should see your name on the list.
   1. Open your entry and scroll down to get the value from `messenger user id` which is the value to fill in the `User ID` on [Zapier]
1. Activate the `TEST & REVIEW` to confirm.

It will look like this:

![create-private-facebook-messenger-chatbot-zap-find-subscriber](/assets/images/posts/low-code/2020-03-27-create-private-facebook-messenger-chatbot-zap-find-subscriber.png "create-private-facebook-messenger-chatbot-zap-find-subscriber")

To configure the `Send Text Message in Chatfuel` follow these steps:

1. Type or select `chatfuel`.
1. For the `Choose Action Event` field, select `Send Text Message`.
1. For the `Chatfuel account` field select the account that was created `Chatfuel Example Private Messenger Bot`.
1. For the `Subscriber` field select from the previous activity the `Chatfuel User ID`.
1. For the `Message Tag` field, just choose `Update`.
1. For the "Message Text` field, just enter `Test from Zapier`.
1. Activate the `TEST & REVIEW` to confirm.

It will look like this:

![2020-03-27-create-private-facebook-messenger-chatbot-zap-send-message](/assets/images/posts/low-code/2020-03-27-create-private-facebook-messenger-chatbot-zap-send-message.png "2020-03-27-create-private-facebook-messenger-chatbot-zap-send-message")

And on [Messenger] it will look like this:

![create-private-facebook-messenger-chatbot-chat](/assets/images/posts/low-code/2020-03-27-create-private-facebook-messenger-chatbot-chat.png "create-private-facebook-messenger-chatbot-chat")

[Telegram]: https://desktop.telegram.org/
[Facebook]: https://www.facebook.com/
[Messenger]: https://www.messenger.com/
[Facebook pages]: https://www.facebook.com/pages/?category=your_pages
[Example Private Messenger Bot]: (https://www.facebook.com/Example-Private-Messenger-Bot-100180854977501/)
[chatfuel]: https://chatfuel.com/
[ManyChat]: https://manychat.com/
[Dialogflow]: https://dialogflow.com/
[Facebook's Business Integration]: https://www.facebook.com/settings?tab=business_tools
[Zapier]: https://zapier.com/
[Integromat]: https://www.integromat.com/
[user page-scoped ID (PSID)]: https://developers.facebook.com/docs/pages/access-tokens/psid-api/
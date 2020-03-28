---
excerpt: If you need a bot for your personal home automation, then this post describes how to achieve what is perceived as a private bot on Telegram.
tags: 
    Chatbot
    Low-Code
    Telegram
categories: Low-Code
title: Creating a private Telegram chatbot

---

In this post, I want to explain how to create a private bot in [Telegram]. With *private*, I'm referring to bots that are not possible to be found by others or won't interact with others. These sort of bots are meant usually for some personal automation, where the bot act as the delivery entity to you or your family.

## Not exactly a private bot

With [Telegram] bots are not private. Everybody can find them. The difference is that a certain communication channel with the bot can be made private. This is a group with the bot that you and the bot are members of. This sort of private channels is usually useful for home automation, where the bot is to speak with one person only.

## Creating the bot

Creating the bot is generally easy by following instructions on [Bots: An introduction for developers] using the [BotFather]. 

1. Open a session with [BotFather].
1. Enter `/newbot`.
1. Enter the name of the bot. `Example Bot for Blog`
1. Enter a username for the bot. **it must end with `bot`**. `example_blog_bot`

> Me, [25.03.20 16:02]
> /newbot
> 
> BotFather, [25.03.20 16:02]
> Alright, a new bot. How are we going to call it? Please choose a name for > your bot.
> 
> Me, [25.03.20 16:03]
> Example Bot for Blog
> 
> BotFather, [25.03.20 16:03]
> Good. Now let's choose a username for your bot. It must end in `bot`. Like > this, for example: TetrisBot or tetris_bot.
> 
> Me, [25.03.20 16:03]
> example_blog_bot
> 
> BotFather, [25.03.20 16:03]
> Done! Congratulations on your new bot. You will find it at t.me/> example_blog_bot. You can now add a description, about section and profile > picture for your bot, see /help for a list of commands. By the way, when > you've finished creating your cool bot, ping our Bot Support if you want a > better username for it. Just make sure the bot is fully operational before > you do this.
> 
> Use this token to access the HTTP API:
> 1101361374:AAHS_DYrAUohT-HQXVKKz-M1howAxvXdRLA
> Keep your token secure and store it safely, it can be used by anyone to > control your bot.
> 
> For a description of the Bot API, see this page: https://core.telegram.org/> bots/api

That's it. The new bot is available and anyone can find it as [Example Bot for Blog]. 

The bot has been deleted and therefore all sensitive information is irrelevant.

## Create a group with your bot

This is a simple step. We need to do this now because afterward, we will disable the ability of the bot to be added into conversations.

This group we have created is going to be also our private chat with the bot.

## Disable joining groups

By default, the bot can be added to a group conversation by anybody who can find it. To make sure this doesn't happen, we need to disable this.

1. Open a session with [BotFather].
1. Enter `/setjoingroups`.
1. Enter the name of the bot. `@example_blog_bot`.
1. Enter `Disable`

> Me, [25.03.20 16:13]
> /setjoingroups
> 
> BotFather, [25.03.20 16:13]
> Choose a bot to change group membership settings.
> 
> Me, [25.03.20 16:13]
> @example_blog_bot
> 
> BotFather, [25.03.20 16:13]
> 'Enable' - bot can be added to groups.
> 'Disable' - block group invitations, the bot can't be added to groups.
> Current status is: ENABLED
> 
> Me, [25.03.20 16:13]
> Disable
> 
> BotFather, [25.03.20 16:13]
> Success! The new status is: DISABLED. /help

If you try to add your bot into a group, you can't. So it is best that you already start a group with it before doing this step. You can always enable the group joining feature, create a group with it and yourself and then disable it again.

## Interacting with the bot

In the [Telegram Bot API] the API of [Telegram] is described. To interact with this bot we need the API token `1101361374:AAHS_DYrAUohT-HQXVKKz-M1howAxvXdRLA` which the [BotFather] provided to us. The general uri pattern is `https://api.telegram.org/bot<token>/METHOD_NAME`. **Notice** that the token follows the word `bot` in the URL.

For example the `getMe` method will provide information about the bot. Just do a `GET` with this URL `https://api.telegram.org/bot1101361374:AAHS_DYrAUohT-HQXVKKz-M1howAxvXdRLA/getMe`. 

```json
{
   "ok":true,
   "result":{
      "id":1101361374,
      "is_bot":true,
      "first_name":"Example Bot for Blog",
      "username":"example_blog_bot",
      "can_join_groups":false,
      "can_read_all_group_messages":false,
      "supports_inline_queries":false
   }
}
```

To send messages to this *private* group we've created with the bot, we will need to target it by providing the `chat_id` parameter that represents this group. The `chat_id` is also necessary for all integrations with other tools like [Zapier] or [Integromat] and this is why it is important.

 To extract it use the `getUpdates` method. Do a `GET` with this URL `https://api.telegram.org/bot1101361374:AAHS_DYrAUohT-HQXVKKz-M1howAxvXdRLA/getUpdates`. If the result is empty then just type something to the bot in this group.
 
 **Note that this method returns results only when the bot is not enabled for privacy**. If you want to enable privacy, make sure you do it after extracting the `chat_id` as explained below.

```json
{
   "ok":true,
   "result":[
      {
         "update_id":726362299,
         "message":{
            "message_id":3,
            "from":{
               "id":877419474,
               "is_bot":false,
               "first_name":"Alex",
               "last_name":"Sarafian"
            },
            "chat":{
               "id":-475387861,
               "title":"Example Blog Bot",
               "type":"group",
               "all_members_are_administrators":true
            },
            "date":1585150611,
            "text":"Hello"
         }
      }
   ]
}
```

`Hello` is the message I had sent to the bot to make sure I could get results with `getUpdates`.

From the JSON, the `chat_id` is `-475387861`. We can use this send a message using the `sendMessage` method. 

The `sendMethod` is much more complicated but for the purposes of this demonstration, a simple `GET` with a simple message will suffice like this URL `https://api.telegram.org/bot1101361374:AAHS_DYrAUohT-HQXVKKz-M1howAxvXdRLA/sendMessage?chat_id=-475387861&text=Hello`.

```json
{
   "ok":true,
   "result":{
      "message_id":4,
      "from":{
         "id":1101361374,
         "is_bot":true,
         "first_name":"Example Bot for Blog",
         "username":"example_blog_bot"
      },
      "chat":{
         "id":-475387861,
         "title":"Example Blog Bot",
         "type":"group",
         "all_members_are_administrators":true
      },
      "date":1585151160,
      "text":"Hello"
   }
}
```

And this has happened in the group with the bot

> Me, [25.03.20 16:36]
> Hello
> 
> Example Bot for Blog, [25.03.20 16:46]
> Hello



## Configuring for privacy

To enable encryption of the communication with the bot, you must enable the privacy feature. 

1. Open a session with [BotFather].
1. Enter `/setprivacy`.
1. Enter the name of the bot. `@example_blog_bot`. **Note** that the `@` is required.
1. Enter `Enable`

> Me, [25.03.20 16:09]
> /setprivacy
> 
> BotFather, [25.03.20 16:09]
> Choose a bot to change group messages settings.
> 
> Me, [25.03.20 16:09]
> @example_blog_bot
> 
> BotFather, [25.03.20 16:09]
> 'Enable' - your bot will only receive messages that either start with the '/' > symbol or mention the bot by username.
> 'Disable' - your bot will receive all messages that people send to groups.
> Current status is: ENABLED
> 
> Me, [25.03.20 16:09]
> Enable
> 
> BotFather, [25.03.20 16:09]
> Success! The new status is: ENABLED. /help

**Remember** that after doing this the `getUpdates` method from the API will return empty results or the last encrypted message.

[Telegram]: https://desktop.telegram.org/
[Bots: An introduction for developers]: https://core.telegram.org/bots#6-botfather
[BotFather]: https://telegram.me/BotFather
[Example Bot for Blog]: https://telegram.me/example_blog_bot
[Telegram Bot API]: https://core.telegram.org/bots/api
[Zapier]: https://zapier.com/
[Integromat]: https://www.integromat.com/

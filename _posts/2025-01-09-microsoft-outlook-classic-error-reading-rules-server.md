---
title: Microsoft Outlook Classic - Error reading rules from server.
excerpt: Troubleshoot and resolve the error from Microsoft Outlook "There was an error reading the rules from the server. The format of the server rules was not recognized."
categories: Tips
---

Recently my Outlook (classic) application started throwing the following error:

> There was an error reading the rules from the server. The format of the server rules was not recognized.

![There was an error reading the rules from the server. The format of the server rules was not recognized.](/assets/images/posts/2025-01-09-microsoft-outlook-rules-error.png "There was an error reading the rules from the server. The format of the server rules was not recognized.")

I couldn't find much about the issue online and this post is to help others. 

Many thanks to the people from my IT department who helped resolve this issue. Many people complain about IT departments, but unfairly in my opinion. When you have encountered and solved that many problems, experience is beneficial.

## The problem

During the launch of Microsoft Outlook in classic mode, this error would occur with a classic `Win32` error message dialog. There was no problem though, when launched in the New (Beta at the time of writing) mode. Nor was there a problem when launched on the web application.

## Troubleshooting

We tried the usual processes when things break with Outlook but without success, that is to clear/reset and even create a new profile.

We even tried installing Outlook on a new laptop but the problem persisted. This is when we realized that the problem is with the rules.

## Solution

While looking at the rule screens between Outlook (classic) and Outlook (Web), we noticed that there was at least one rule shown from the server among the other Client Side rules I have configured. We deleted the next rule from the list and the issue was resolved.

We don't know what the problem was exactly, except that it was in the definition of one rule. I use the same logic with all my rules, that is conditions on the sender or words in the subject and then move to email to a specific folder. This is why we didn't think of this sooner. There was no reason to suspect that one rule was the cause of problem.

The error message is misleading unfortunately. It implies that all the rules failed to load but in reality it stops loading with the first error. A better error handling and a better message would had been so much more useful. 

I hope this post, is the alternative to this poor error handling by the developer.
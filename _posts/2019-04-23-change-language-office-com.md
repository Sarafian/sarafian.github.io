---
title: Configure lamguage in office.com
excerpt: This post explains how office.com is organized in terms of configuration and how to change the language
categories:
- Tips
---

I've recently joined an organization where the [office.com](https://outlook.office.com) was configured to use the French language, despite the fact that I do not speak French. I found quite easily how to change the language for the mail. But for applications like Word, Excel, PowerPoint and OneDrive the situation was very strange. This post is to explain how to resolve this issue and to share the explanation I've received from the it guys who helped me out.

{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-french.PNG" alt="onedrive-french" caption="OneDrive in French" %}
{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-french.PNG" alt="onedrive-french" caption="OneNote in French" %}

Naturally, I created an IT ticket as I couldn't find a solution for my issue. After some interactions with different technicians, someone provided a reasonable explanation which is quite interesting to know in my opinion. First he mentioned, that [office.com](https://outlook.office.com) chooses the language based on the country that the requests originates from. I currently live and work in Belgium, but my organization's proxy setup has a "random" logic and I am accessing internet as if as I was in Paris, France. So [office.com](https://outlook.office.com) decided to set up everything in French. He also explained that the [office.com](https://outlook.office.com) is devided into 3 logical applications, each with its own configurations:

- Mail
- Sharepoint
- OneDrive

OneDrive includes all applications that depend on an actual file, like most office applications do. For this reason, while I had configured English as the language for the mail application, I was still getting Office applications in French because the configuration is not shared. Strangely enough, the UI option to enter the settings page was missing or maybe it is not available by choice of Microsoft so the technician helped me create the url for the language settings page. 

{% include figure image_path="/assets/images/posts/2019-04-23-settings-french.PNG" alt="onedrive-french" caption="OneDrive settings in French" %}

The url is like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/muisetng.aspx` where the **{sortofemail}** is in essence a "url friendly" version of your email. To find your own, just naviagate to the OneDrive application and extract your **{sortofemail}** from the url on your browser address bar. It should look like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/onedrive.aspx`.

{% include figure image_path="/assets/images/posts/2019-04-23-settings-english.PNG" alt="onedrive-english" caption="OneDrive settings in English" %}

Once you select an alternative language, all other applications will be rendered in your language of choice after just refreshing the tabs on your browser.
{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-english.PNG" alt="onedrive-english" caption="OneDrive in English" %}
{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-english.PNG" alt="onedrive-english" caption="OneNote in English" %}

**Note** that the url for the language settings can also lead to the general settings of OneDrive which are a lot. Just adapt the following and you are good to go. `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/settings.aspx`



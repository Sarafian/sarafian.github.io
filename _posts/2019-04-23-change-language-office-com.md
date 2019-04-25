---
title: Configure language in office.com
excerpt: This post explains how office.com is organized in terms of configuration and how to change the language
categories:
- Tips
---

I've recently joined an organization where the [office.com](https://office.com) was configured to use the French language, despite the fact that I do not speak French. It is very easy to change the language for the mail application. But for applications like Word, Excel, PowerPoint and OneDrive I couldn't and at the end I reached out to the IT organization who did some digging and asked questions to Microsoft. This post is the share the insight I received, share some basics on how [office.com](https://office.com) applications are organized and provide a quick tip on how to change the language and other settings your self.

{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-french.PNG" alt="onedrive-french" caption="OneDrive in French" %}
{% include figure image_path="/assets/images/posts/2019-04-23-onenote-french.PNG" alt="onenote-french" caption="OneNote in French" %}

As you would normally expect, upon first access of the [office.com](https://office.com), a language is chooses automatically based on the detected IP address. Now, this is quite problematic with organizations because quite often consolidated proxies are utilized which means that the detected IP is of a different country and therefore the detected language is probably not good. In my particular case, my organization employees a "random" logic in the proxy and we don't access the internet from the same location. For example, for me the proxy makes me look as if I'm in downtown Paris but others it is in Brussels, Amsterdam or Berlin. Especially for expats, this can be particularly tricky and I think that Microsoft and any other organization should re-consider their approach into the user experience. With people mobilizing for work, we should be able to select our language on first access and not try to figure out solutions in languages that are foreign to us. This makes it particularly difficult when the option is hidden or difficult to find like it is for [office.com](https://office.com).

The insight is that [office.com](https://office.com) applications are divided into three groups, each with its own settings and configurations that are not shared. 

- **Mail** which provides a relatively easy configuration of the language and other related options. 
- **Sharepoint** for which the language is normally controlled through the page properties.
- **OneDrive** for which the configuration menu options were not available to me. 

**OneDrive** includes all applications that depend on an actual file as most office applications do. As the

With my mail application configured to English, I was always running into a French UI when accessing OneDrive or the rest of the Office applications. In the same time, opening the same assets from the locally installed applications would be in English. The difference in localization settings between the web, mobile and laptop/pc applications should be also something to consider from Microsoft. I could never find a UI option to enter settings like I normally do for the mail application and IT had to help me construct a URL to access the language configuration page. 

The URL is like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/muisetng.aspx` where the **{sortofemail}** is, in essence, a "url friendly" version of your email. To find your own, just navigate to the OneDrive application and extract your **{sortofemail}** from the URL on your browser address bar. It should look like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/onedrive.aspx`.

{% include figure image_path="/assets/images/posts/2019-04-23-settings-french.PNG" alt="settings-french" caption="OneDrive settings in French" %}

Once you select an alternative language, that is English in my case, the configuration pages will be rendered in English.

{% include figure image_path="/assets/images/posts/2019-04-23-settings-english.PNG" alt="settings-english" caption="OneDrive settings in English" %}

Once this is done, refresh or open again all tabs with OneDrive applications and they should be rendered in your chosen language

{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-english.PNG" alt="onedrive-english" caption="OneDrive in English" %}
{% include figure image_path="/assets/images/posts/2019-04-23-onedrive-english.PNG" alt="onedrive-english" caption="OneNote in English" %}

If you want to access all OneDrive settings, then use the above technique with the only difference being that the landing page is `settings.aspx` instead of `muisetng.aspx`. It should be like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/settings.aspx`.{:.tip}
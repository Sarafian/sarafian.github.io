I've recently joined an organization where the [office.com](https://outlook.office.com) was somehow configured to use the French language, which I don't understand. I found quite easily how to change the language for the mail and sharepoint but I couldn't fix it for OneDrive and other office appications. This post is to explain how to resolve this issue, if you have run into it and to share the explanation I've received from the it guys who helped me out.

First some context. After speaking with multiple IT support guys, someone explained to me that that [office.com](https://outlook.office.com) is devided into 3 regions each with its own configurations
- Mail
- Sharepoint
- OneDrive

OneDrive includes all applications that depend on an actual file, like most office applications do. For this reason, while I had configured English as the language for the mail application, I was still getting Office applications in French. This was also very strange, because when navigating to e.g. Word, everything would be in English but as soon as I would open a file, then everything would switch to French which was very annoying.

To configure the language for the OneDrive section, create a url like this `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/muisetng.aspx`. The `{sortofemail}` is in essence a url friendly version of your email and can be acquired from the url when navigating to the OneDrive application e.g. `https://brusselsairlines-my.sharepoint.com/personal/{sortofemail}/_layouts/15/onedrive.aspx`

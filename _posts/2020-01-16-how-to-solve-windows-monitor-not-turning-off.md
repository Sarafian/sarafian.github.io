---
title: Monitors in Windows 10 don't turn off
excerpt: Background option in the lock screen setting in Windows seems to break the power settings for turning the screens off
categories:
- Tips
header:
  overlay_image: /assets/images/posts/2020-01-16-windows10.jpg
---

I recently did a fresh installation of Windows 10. At some points I realized that my monitors won't turn off when after the locking timeout as configured.

Initially I thought that [Windows Insider Program][1] was at fault because I noticed the issue just after enabling it and after activating the [Windows Subsystem for Linux 2][2] and the [Docker Desktop WSL 2 backend][3]. I was hesitant to believe that they were the issue but I had looked everywhere in the operating system and in fact at the time I thought that this was the only difference with the OS configuration before my fresh installation. 

I've looked in processes, registry key and I even [configured the lock screen display timeout on Windows][4] to be different from the timeout of turning off the display. This is quite a useful feature and I don't understand why it is hidden from the majority. Some people asked to check on drivers as they are some cases when a program or a driver intervenes and doesn't allow the monitor to turn off.

![Power Options\Display](/assets/images/posts/2020-01-16-power-options-display-turn-off-console-lock-separated-timeout.png "Power Options\Display")

But nothing worked and I was ready to give up but I decided to have another look and investigate again anything result when searching for **lock**`**. My investigation would require experimentation and for this reason I lowered my timeouts to 1 and 2 minutes respectively for locking and turning off. At the end I found where the problem was. In the **Lock screen** settings there is a **Background** option. When the background is set to **Slideshow** then the monitors don't turn off. 

![Settings\Lock Screen\Background](/assets/images/posts/2020-01-16-lock-screen-background-slideshow.png "Settings\Lock Screen\Background")

Switching it to the default value of **Picture** solves the problem.

![Settings\Lock Screen\Background](/assets/images/posts/2020-01-16-lock-screen-background-picture.png "Settings\Lock Screen\Background")

In my opinion there is an obvious bug in Windows because the setting should control only what is shown in the screen and not when the monitor turns off which is managed by the power settings.

[1]: https://insider.windows.com/en-us/
[2]: https://docs.microsoft.com/en-us/windows/wsl/wsl2-about
[3]: https://docs.docker.com/docker-for-windows/wsl-tech-preview/
[4]: https://www.ghacks.net/2018/06/02/configure-the-lockscreen-display-timeout-on-windows/

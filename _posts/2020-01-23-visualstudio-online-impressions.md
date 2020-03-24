---
title: First impressions with using the Visual Studio Online preview environments
excerpt: First impressions with using the Visual Studio Online preview environments.
tags: 
  - vsonline
categories: Tips
---

Recently I read about the [Visual Studio Online][1] and I was initially suspicious. 

> Doing normal developing tasks from a browser? 

That sounded too *science fiction like* to me, not because it is technically not possible but because I am always worried about the usability and productivity of such environments. They sound great on paper but usually disappoint in real life.

## The problem

Now and then, I want to contribute to some of my repositories and my professional laptop is not helpful. Its no secret that many tools exist only for Linux and when available on Windows, they usually have troubles and limited support by the community. Within the windows ecosystem, there were two possible solutions until now. Either use the [Windows Subsystem for Linux (WSL)][3] or use docker to run linux containers. The original WSL is slow and the new version [WSL 2][4] requires to be a member of the [Windows Insider program][5]. Even when it will become available, I expect some problem from IT from organizations that practically Windows only focused. So this works from my home system but not my professional one. As an alternative, I've always considered setting up a VM on Azure or AWS and use it on demand but I don't need to develop that often, the price of VMs is high and maintaining another system seems too much in head. There is also the option of doing remote desktop to my home system but there are too many complications and therefore I don't consider it even a solution.

This blog is powered by [github pages][7] and [Jekyll][6]. Jekyll requires [Ruby][8] which is one of the *difficult* ones on Windows so at home I used to use WSL and recently I upgraded to WSL 2. Until now, my solution to authoring a new post while not home was to create a branch and edit files on my [Github][9] repository[10]. I would write most of my content, use the Github's markdown preview and eventually go home, add some pictures, finish up and run Jekyll to see if it works before publishing. Not a very nice experience considering that when using [VSCode][11] you leverage great productivity enhancements available through extensions like live markdown preview and contextual spell checking.

## Visual Studio Online as a solution

What I felt that I really needed was a container like environment that could provide developer oriented functionality with a UI to use. Not sure if this was the intention but Microsoft added recently some incredible capabilities to the Visual Studio Code. To understand the basics of how Visual Studio Online works, it is important to understand that Visual Studio Code is split in two basic layers by design, that are one for the client (UI) and one for the actual execution. With that in mind, the [Remote Development][12] extension was released which allows using VSCode as client to develop on a remote system, for example in WSL instance or even within a container instance. This is not just editing code but for all tasks normally expected by developers. The UI runs on your local system but everything else like the files, the git, the terminals, the extensions etc run on the remote system. Once this was possible it was a matter of time for someone to release a service that offers on demand development instances and that was Microsoft with [Visual Studio Online][1]. [Visual Studio Online][1] is not to be confused with the original Visual Studio Online which was later renamed to Visual Studio Team Services which was finally renamed to [Azure DEVOPS][13].

## Getting started

This is how it works. You navigate to https://online.visualstudio.com/environments/ and signin with your Azure credentials. There you create a new instance. You get two options depending on how much power you need but also how much you want to pay. I choose the cheaper option because as I mentioned I don't need much from my "on-demand" development environment. Once the environment is created then you can start it and connect to it either through you web browser or from your local VSCode using the [Visual Studio Online][15] extension. More information about the process is available [here][14]. 

In the following screenshots show me working on my previous post from the browser and from my local VSCode instance. Note that I've changed my local them and this is used by the local instance. But you can also change the theme on the remote instance and then your browser would adjust as well.

| Access from web browser | Access from Visual Studio Code |
| ----------------------- | ------------------------------ |
| ![Access from web browser](/assets/images/posts/2020-01-21-visual-studio-online-web.png "Access from Visual Studio Code") | ![Access from web browser](/assets/images/posts/2020-01-21-visual-studio-online-vscode.png "Access from Visual Studio Code") |

## What is good?

There are many good things about this setup and many are similar to the benefits of using virtual environments or even better docker containers. 

You can simply create and destroy one with almost no hassle especially if you can automate a couple of things. In other words, you can create a development environment, use it to experiment and learn and then destroy it. You can do this without any fear of permanent impact but you can also do this to fine tune a certain process. For example, I first needed to understand how to make Jekyll available on the instance. To get there I needed to get Ruby, update it, update the gems, install Jekyll and Bundler and update my gems etc before anything else. I'm not experienced with Linux, so I had to go over a couple of iterations until I figured it out and as a last step I had to confirm my flow before writing about it on [Configure Ruby and Jekyll in VisualStudio online environment][16]. I actually used my blog environment to author and another environment created just for the purpose of writing the steps and taking pictures. Once I finished, I just deleted the 2nd environment.

When creating the instance you can specify a git repository that will be used as your workspace when the environment starts. Effectively this makes the instance dedicated to a repository and you can always get a clean setup very easily. The instance is created with the master branch of the repository checkout and ready to work on.

You can install extensions to the embedded VSCode of the remote environment. What I do for example is that I install the [Settings Sync][17] extensions and use it to download my preferred extensions and configuration on the remote environment using a gist. Unfortunately, this requires some extra actions on the UI as some of the settings need to be explicitly installed on the remote instance.

![Install remote extensions](/assets/images/posts/2020-01-23-vscode-install-remote-extension.png "Install remote extensions")

You can drag and drop files from your local environment to the remote environment.

![Drag and drop files](/assets/images/posts/2020-01-23-vscode-drag-and-drop.png "Drag and drop files")

You can suspend the environment and later continue from where you left off.

## What is not that good?

Honestly I cannot find something that is bad with this product considering that it is still in preview. 

I've noticed some disconnections happening which disturb my productivity but it could be because of the network infrastructure I depend on. For this reason it is best to frequently save the files and this way they are at least persisted. For me this is not a problem as I'm a `Ctrl+S` addict.

The [pricing][17] is I think the biggest problem if it could be considered a problem at all. An environment is priced based on Environment Units (EU) which represent all the necessary resources (CPU, Memory, Disk, Network, etc) required for the instance to function. The smallest of the two configurations, has when suspended a base rate of 2 EU per hour and an active rate of 125 EU per hour. Each EU costs currently 0.00395 Euros. Given that a month has 730 hours, an environment would cost you minimum `2*730*0.00395=`5.767 Euros while suspended for the entire month. On average, 2h of activity on the environment would cost you around 1 Euro.

## Summary 

Considering the cost of 1 Euro per 2 hours, Visual Studio Online is not suited for heavy and frequent use because costs would add up and it would probably be better to use a VM or invest in other alternatives. But for infrequent use like I need it for, it is good I think. 

If I Microsoft would lower the active rate then I think we would have a great product and a winner. This idea is so good that it could become part of training and educations. Imagine you could create a template. You could spin up 10 environments for a given repository (maybe first fork) and ask the audience to use their basic VSCode to follow the exercises. Besides training, this could be applicable for any sort of onboarding of developers with a ready to work on environment as well.

[1]: https://online.visualstudio.com/environments/
[2]: https://github.com
[3]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux
[4]: https://docs.microsoft.com/en-us/windows/wsl/wsl2-install
[5]: https://insider.windows.com/en-us/
[6]: https://jekyllrb.com/
[7]: https://pages.github.com/
[8]: https://www.ruby-lang.org/en/
[9]: https://github.com/
[10]: https://github.com/Sarafian/sarafian.github.io
[11]: https://code.visualstudio.com/
[12]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack
[13]: https://azure.microsoft.com/en-us/services/devops/
[14]: https://code.visualstudio.com/docs/remote/vsonline
[15]: https://marketplace.visualstudio.com/items?itemName=ms-vsonline.vsonline
[16]: {% post_url 2020-01-21-develop-author-jekyll-blog-visualstudio-online %}
[17]: https://azure.microsoft.com/en-us/pricing/details/visual-studio-online/
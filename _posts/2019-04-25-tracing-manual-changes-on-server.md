---
title: Tracing manual changes on servers
excerpt: Track changes of consistency between servers, especially when Remote Desktop is allowed
categories:
- Tips
---

Granted that with modern practices, manual changes on server(s) should not be allowed, the reality is that many organizations still employee such practices where privileged users have direct access to production assets like servers and databases. **This post is not about** discussing the merits of this practice but to suggest an idea of how to easier mitigate problems that derive from this practice. I also want to clarify that as I'm not Kubernetes expert, please consider that some references from my part might be wrong.
{: .notice--warning}

The idea is not 100% mine, to be honest. It is a spin-off from something that [Pieter Lange](https://github.com/pieterlange) shared yesterday. Pieter is a Kubernetes and a CI/CD expert that helps teams setup their CI/CD pipelines and provides the necessary tooling to help the developer monitor production. In essence, he enables development teams with the infamous 

> You build it you run it

Pieter, being an expert in Kubernetes, focuses on container technology and his idea is naturally based on related concepts respectfully. He has developed a tool for Kubernetes [kube-backup](https://github.com/pieterlange/kube-backup), that converts the state of a Kubernetes cluster into text configuration and then leverages *change management* techniques to track the cluster's evolution. Here are the high levels steps I understood:

1. Export the state of Kubernetes into a configuration file.
1. Upload the configuration file to a git repository.
1. Use git to track in time the evolution of the cluster by comparing the files among the commits.
1. Use git hooks to connect such changes to any other *DEVOPS* related tool.

First step is the foundation. The tool expresses the active runtime configuration of a Kubernetes cluster in a file. Then all other steps are about using source control to track changes over time.

Now consider replacing **Kubernetes** with any resource that has a runtime state. As long as the behaviour of the runtime can be described in a configuration file, then the above concept is applicable for anything. For example

- The configuration of an operating system
- The configuration of an application
- The configuration of a database, including e.g. stored procedures

Once you are able to combine the basic two pillars, then you can *easily* get a tool that tracks changes to a dynamic system over time. 

- Export runtime behaviour to one or many files
- Store the files into a system that provides an evolution trace over time

If you actually think about it, we have been doing stuff like this for some time now, but never as clever as what Pieter shared. When a customer would create a ticket in a CRM system e.g. JIRA, then we would ask him for log files, specific configuration files etc. In other words, we have been already exporting the part of the behaviour of systems in another tool. It just that the priorities have been really different and automation and other goodies were not the priority and often it was the other way around as this flow was monetized. But things are changed, people have a different expectation and the existing inefficient flow can be vastly improved.


This is an example from a real situation from past experience. My organization would develop a CMS type of application, which we would deliver as an artifact to a group of professionals to customize and depending on contracts the customer or us would deploy the customized artifact to his servers. Naturally, like in most setups this, there were silos and different responsibilities and some from the customer and us had **administrator* rights on the servers and databases. As the customer requested changes to the behaviour of the application, often small and sometimes bigger changes would be applied directly on the servers. Changes were made on servers with limited toolings, like editing XML files in the Windows notepad tool, often resulting in errors. When the customer employed multiple servers, then things became much more dangerous, as the potential for human error is multiplied and you lose control. With servers sharing heavy workloads, often in the scale of hours, the problems became inconsistent, unpredictable and very difficult to resolve. With such a tool as described above, the following would have been possible for easy troubleshooting.

- Differences between servers of any kind
- What were the differences
- Timeline correlation of when problems started and when changes were made

The only thing that is not possible to track is the person who made the mistake. But in situations like this, where human error is so much accepted as a factor in the working culture, I think it is wrong to try and track responsibility. It is a dead end, to begin with. Instead, you need a process for quick resolution until the root cause is mitigated. But even when automation becomes available and better practices are employed, such a tool will always be useful when needed. 

{% include figure image_path="https://www.azquotes.com/picture-quotes/quote-everything-fails-all-the-time-werner-vogels-64-10-11.jpg" alt="everything fails" caption="" %}

As a reminder, as with any crisis solving tool, you hope that you never have to use it, you always want one when the crisis does happen. This what chaos engineering is all about, after all, and that is accepting that things will go wrong, being prepared to resolve the issue and extract prevention mechanisms by each iteration.
{: .notice--info}
---
title: Introduction to posts about JSONPath, SOAP and Amadeus with PowerShell
excerpt: This is an introduction to upcoming posts on how to use JSONPath syntax with PowerShell, consume Amadeus SOAP API and in general 
categories:
- Amadeus
tags:
- PowerShell
- SOAP
- JSONPath
---

# Airline industry

The airline industry as well as with the rest of the travelling industry depend quite a lot on SOAP to integrate the systems. All companies within the travelling and transportation sector base their business on well established platforms like Amadeus, Sabre etc. These platforms are what runs behind the clerks desks at the airport check in, or at the gates or many other places you as a passenger don't see. With the industry being heavily standardized by [IATA][3], there isn't much room for the airlines to differentiate and this is why we, as passengers, experience the process of air travelling in a consistent manner regardless of the airline. These platforms exist for a very long time and they are not easy to change. There is an incredible amount of business flows implemented around their APIs and the standardization and that means the technology doesn't change as well. You could consider them as legacy systems but they are still very much functioning.

Most airlines, don't even own for example the reservation system. If you actually look into the history, then one the rest of the world discovered the *Software as a service* the traveling sector was doing already since the 70s.

The airline I'm in involved with particularly, uses [Amadeus][1] (aka 1A) as their main driver. [Amadeus's API][2] is a stateful SOAP API and to makes maters worse, it is very much complicated in terms of the data contracts used.

# UI to drive process

When my journey started, I noticed that people use UI tools like [SOAPUI][4] as the development tool for all sorts of processes such as: 
- Visualizer of the API like people use [Postman][8].
- Proof of concepts (POC) flows
- Some sort of workflow. where test cases combined with groovy code take csv as input, do some processing and then export to csv.

The main indication was that you could recognize its ui on the screens of many people including developers, architects and product owners. I also noticed that people, who complained about SOAPUI found an alternative with similar features but with a more modern ui. Regardless, the approach and usage remained the same while inheriting the same problems for some of the above mentioned processes.

# Why PowerShell

With my background in automation this felt very strange and troubling. I always use UI tools when available as the visual accelerator to help establish a basic understanding. But when things need to be combined or repetitions is expected, then code is my first choice.

The fact that the [Amadeus's API][2] is statefull automatically meant that some custom code was required to extract the session variables from one invocation and feed them to the next ones. SOAPUI can't do this by itself and therefore Groovy code is required in between two requests. Looking back into my PowerShell experience with SOAP, I remembered that once you had a soap client from [New-WebServiceProxy][5], then the headers were automatically managed across different invocations, in a similar way that browsers manage the cookies for example.

So, I started looking into this with high hopes.

# Trouble in paradise

Unfortunately things didn't go easy with PowerShell and there were a lot of complications. The complications were not easy to address and I can understand why someone would skip this approach and fallback to a UI tool, especially when everyone else is doing the same. But, often with software, what the majority does is not the best solution and innovation requires avoiding the established approach.

The problems were the followings:

- [New-WebServiceProxy][5] related issues.
  - It has limitations. This is because the workhorse is the `System.Web.Services` assembly which used to be driver for ASMX technology which is the predecessor of WCF). For this particular case, there is a expected header that is not recognized by the cmdlet and is not available on the created proxy. This inevitably causes leads to errors related to bad headers.
  - It is not available in PowerShell 6/Core scope, mostly because SOAP is not getting much attention from the .NET Core team. Remember that the `System.Web.Services` assembly is about ASMX which is an obsolete technology and difficult to justify its port to .NET Core.
  - The interface and data contracts generated are compiled into memory. Most people understand this as the current PowerShell session or windows. Although there is some preventive code, the reality is that multiple invocations of the cmdlet mess up the session, especially when developing a solution.
- The data contracts referenced by the operations are very complicated and difficult to understand from a scripting language. The only good visualization is an xml fragment but the xml doesn't reveal the types and relationships that are described in the WSDL and drive the code generation.
- One must use the data contract types generated by the cmdlet, because they implicitly contain the annotations that drive the XML serialization. This is has to be very specific because this process produces SOAP envelopes which are very strict, something that was considered one of the powers of SOAP. Any attempt to deviate away from the generated types and use dynamic objects, will not serialize to the expected XML. In fact, the variable will not be even accepted as a parameter to the operation because the types won't match.

These issues are manageable but the [Amadeus's API][2] enlarges them to a point that reveal the weakness of SOAP when using a scripting language.

Here is an example of an XML fragment for the `PNR_Retrieve` operation from [Amadeus][1]. 

```xml
<retrievalFacts>
  <retrieve>
    <type>2</type>
  </retrieve>
  <reservationOrProfileIdentifier>
    <reservation>
      <companyId>SN</companyId>
      <controlNumber>MTHOH2</controlNumber>
    </reservation>
  </reservationOrProfileIdentifier>
</retrievalFacts>
```

Based on this XML, one would expect that a path like `retrievalFacts.reservationOrProfileIdentifier.reservation.companyId` would make sense but in reality it is `retrievalFacts.reservationOrProfileIdentifier.companyId`. When developing in a typed language with a powerful IDE, this is relatively easy to spot and connect the dots but when in PowerShell, errors about properties start showing up.

Remember, than there are no code files generated that can be consulted for the structure. The only way to try and establish a similar picture of the data structures is to use `GetType()` and `Get-Member` and start drilling in. But this is very tedious and a difficult process. Anyone with a C# background would probably give up at this point, switch to a C# project and generate the respected service references. But I want to keep it dynamic not because this is the nature of scripting languages, but because of something I noticed in the [Amadeus's API][2]. Their API is really big and the operations are organized in different functional areas as they call them. Also, they don't offer all of the operations to every client. In fact what they do, is that they ask the candidate consumer request which operations he is interested in and they compose a specific endpoint with only those. They do this by some sort of an API Gateway. For example, I go to Amadeus and ask for the following operations from the functional areas of `PNR`, `Security` and `Inv`
- PNR_Retrieve
- Inv_AdvancedGetFlightData
- Security_SignOut

When using the code generated approach e.g. with Visual Studio, then this particular code is applicable only for this particular endpoint. This means, that any solution based on this static approach is very specific and not suitable for a dynamic scripting language like PowerShell. Even if someone would build a binary module, this module would be so specific that would only serve the purpose specific purpose and maybe a template for other endpoints. 

To make matters worst, my airline adds a second layer of SOAP composition through our very own API Gateway named [Sentinet][6]. With Sentinet, we compose endpoints with a mixture of operations provided by a custom Amadeus endpoint and other SOAP endpoints developed internally.

Static code generation is surely not the solution.

While looking around, I soon realized that there isn't anything available and my options became:

- Accept the situation and fallback to SOAPUI
- Code something that generates the SOAP for the SOAP envelope and do a POST. This would be a very dirty solution and felt very unattractive.
- Figure out a solution for PowerShell.

# My goals

Being a bit stubborn and not easily accepting something as impossible with code, I naturally chose the option to solve this in PowerShell and share with the community. Based on the problems and challenges already faced, I set my goals for the solution:

1. Make working with the request and response types of the operations much easier. One of the best aspects of any scripting language is its ability to utilize dynamic objects and allow for expressions like `$var.A.B.C.D="a value". This is not exactly like this in PowerShell when setting but if with the process PowerShell could improve, then even better.
1. Protect the session from the multiple assemblies generated by `New-WebServiceProxy` for the same types for the same endpoint. 
1. Wrap the life cycle management of the Amadeus session state to make code simpler. **This is actually the only goal that is specific to Amadeus**. I set this goal this because I pursuing overall cleaner and simpler code. It would be a shame to ruin the code experience by asking the developer to constantly move around the session values.
1. Find a solution for the missing header of `New-WebServiceProxy`.

In the following posts, I'll discuss about the solution with an individual post about each goal. Unfortunately, the only goal that I haven't solved yet, is the last one. My workaround is to use leverage [Sentinet][6]'s (API Gateway) processing pipeline to inject the missing header when necessary. I believe that this issue can't solved, because of how old technology is that `New-WebServiceProxy` depends on and because the proxies created by the cmdlet don't offer much flexibility to do custom header injections. The other issue that is not currently solved is the lack of support for PowerShell Core. This makes the solution Windows platform specific. The only possibility at this moment is to raise yet another issue, [requesting SOAP support from PowerShell][9] or WCF parity from .NET. In this [comment][10], it becomes clear why there is much uncertainty if we will ever get cross platform support for SOAP in PowerShell.

# 1ASOAP repository

My solution to each of the above goals is already available in the [1ASOAP][7] repository. 1ASOAP offers 3 independent PowerShell modules currently residing in this single repository. None of the modules are published at this moment to the gallery but they work independently of each other.

| Module | Scope | Description | Post |
| ------ | ----- | ----------- | ---- |
| [SOAPProxy][10] | General | Initializes and manages SOAP proxies with `New-WebServiceProxy`. Establishes the concept of default proxy. Simplifies creation of request objects for a given operation. | Not yet published |
| [JSONPath][11] | General | Offer [JSONPath][13] approach with expressions such as `A.B.C` when performing `Set` and `Find` operations. `Trace` operation is a big accelerator to understand the structure and received data | Not yet published |
| [1ASOAP][12] | [Amadeus API][2] | Abstraction over [Amadeus API][2] state management. **Does not** include any specifics for operation and hence works with any composed endpoint. | Not yet published |

[1]: https://www.amadeus.com
[2]: https://developers.amadeus.com/enterprise
[3]: https://www.iata.org/
[4]: https://www.soapui.org/
[5]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-webserviceproxy?view=powershell-5.1
[6]: Sentinet
[7]: https://github.com/Sarafian/1ASOAP/
[8]: https://www.getpostman.com/
[9]: https://github.com/PowerShell/PowerShell/issues/9838
[10]: https://github.com/Sarafian/1ASOAP/tree/master/Source/SOAPProxy
[11]: https://github.com/Sarafian/1ASOAP/tree/master/Source/JSONPath
[12]: https://github.com/Sarafian/1ASOAP/tree/master/Source/1ASOAP
[13]: https://goessner.net/articles/JsonPath/
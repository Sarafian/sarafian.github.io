---
title: Improved SOAP proxies management in PowerShell
excerpt: This is part 1 of a series of posts regarding simplification of PowerShell when working with complicated SOAP interfaces e.g. Amadeus. SOAPProxy, part of 1ASAOP, is a PowerShell module, to help manage proxies created by `New-WebServiceProxy`.
categories:
- Amadeus
tags:
- PowerShell
- SOAP
---

# Under the hood of New-WebServiceProxy

When using [New-WebServiceProxy][1], PowerShell will download the API's WSDL and use it to generate the proxy interface and the data contracts that drive the parameters, response and headers. This is all achieved by leveraging the `System.Web.Services` assembly which was the workhorse in .NET for the ASMX support back on those days.

What is not clear to most, is that the generated types reside within an in memory assembly. This assembly contains the interface and all types that are relevant to it.

Let's imagine that we create a proxy against a SOAP interface with one method `DoSomething` that accepts a parameter `DoSomethingRequest` and returns a `DoSomethingResponse`. 

```powershell
$proxy1=New-WebServiceProxy -Uri $uriDoSomething -Namespace Example
```

The `-Namespace` parameter is optional and when not specified then it gets a random value from the cmdlet. To help simplify this post, we'll keep the parameter.

When the proxy is created, the following types should be available

- `Example.DoSomethingRequest`
- `Example.DosomethingResponse`

If `DoSomethingRequest` or `DosomethingResponse` reference other complex types, then those types will also be created within the `Example` namespace. The expectation is that a code fragment like below executes.

```powershell
$request=[Example.DoSomethingRequest]::new()
$request.Property1="something"

$response=$proxy.DoSomething($request)
```

All the types are part of an in memory assembly with full name `5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`.

# The problem

In the meantime, if the `$proxy=New-WebServiceProxy -Uri $uriDoSomething -Namespace Example` is executed then problems surface because this execution created yet another set of the same types but in a different assembly `4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`. It would be like this:

| Invocation | Full type name |
| ---------- | -------------- |
| 1st | `Example.DoSomethingRequest, 5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |
| 1st | `Example.DosomethingResponse, 5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |
| 2nd | `Example.DoSomethingRequest, 4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |
| 2nd | `Example.DosomethingResponse, 4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |

`$proxy1` expects as parameter an instance of `Example.DoSomethingRequest, 5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` and `$proxy2` expects `Example.DoSomethingRequest, 4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`. The problem is that when code like `[Example.DoSomethingRequest]::new()` is executed then it is unclear which exact type was instantiated. Sure they have the same full name `Example.DoSomethingRequest` but in .NET, the underlying technology of PowerShell, types are matched based on the assembly qualified name which included the assembly. So, without knowledge of which exactly type was used when executing `[Example.DoSomethingRequest]::new()` we might be using the instance with a proxy from a different assembly and that would throw type mismatch exceptions.

# SOAPProxy

The [SOAPProxy][2] PowerShell module helps address this problem. Its main functionality is to wrap the `New-WebServiceProxy` and make sure to only invoke it when necessary. The functionality is supported by `Initialize-SOAPProxy` and internally, the cmdlet maintains a sort of proxy caches in the global variables. When the parameters of `Initialize-SOAPProxy` match an existing proxy, then `New-WebServiceProxy` is not executed and the one from memory is used. The condition to match an existing proxy is the `-Uri`. The `-Uri` is also used by the `Get-SOAPProxy` cmdlet to retrieve a proxy for a given URI.

The `Initialize-SOAPProxy` will also leverage the global variables to track a default proxy. When a proxy is created as default, then all cmdlets in the module can be used without specifying the `-URI` parameter. This is a very common case, because usually a script targets one endpoint and by not having to copy the `-URI` parameter around, it makes the script cleaner and easier to read and also increases its maintainability.

For example, imagine a script that targets to SOAP endpoints `$uri1` and `$uri2`. In the following example, the proxy for `$uri1` is considered the default.

```powershell
# Initialize for $uri1
Initialize-Proxy -URI $uri1 -AsDefault

# Initialize for $uri2
Initialize-Proxy -URI $uri2
```

To retrieve the proxy for `$uri1`, we can invoke `Get-Proxy` implying the default proxy or by specifying the `-URI` parameter.

```powershell

# Get default proxy for uri1
$proxy1=Get-SOAPProxy
# Get proxy for uri1
$proxy1=Get-SOAPProxy -Uri $uri1
```

To retrieve the proxy for `$uri2`, we need to be explicit and specify the `-URI` parameter.

```powershell
# Get proxy for uri2
$proxy2=Get-SOAPProxy -URI uri2
```

For the rest of the examples, we will assume that a default proxy is created like this `Initialize-Proxy -URI $uri -AsDefault`.







[1]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-webserviceproxy?view=powershell-5.1
[2]: [10]: https://github.com/Sarafian/1ASOAP/tree/master/Source/SOAPProxy


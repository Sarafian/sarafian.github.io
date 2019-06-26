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

When using [New-WebServiceProxy][1], PowerShell will download the API's WSDL and use it to generate the respected proxy interface and the respected data contracts for the parameters, the response and the headers. This is all achieved by leveraging the `System.Web.Services` assembly which in .NET is the workhorse for the ASMX back on those days. ASMX was the predecessor of WCF and is also known as the supporting technology for SOAP 1.1.

What is not clear to most, is that the generated types reside within an in-memory assembly. Let's imagine that we create a proxy against a SOAP interface containing one operation `DoSomething` that accepts a parameter of type `DoSomethingRequest` and returns an output of type `DoSomethingResponse`. 

```powershell
$proxy1=New-WebServiceProxy -Uri $uriDoSomething -Namespace Example
```

The `-Namespace` parameter is optional and when not specified then it gets a random value from the cmdlet. To help simplify this post, we'll keep the parameter with value `Example`.

When the proxy is created, the following types should be available

- `Example.DoSomethingRequest`
- `Example.DosomethingResponse`

If `DoSomethingRequest` or `DosomethingResponse` reference other complex types in the WSDL, then those types will also be created within the `Example` namespace. The expectation is that the following code fragment is enough to initialize an instance of `Example.DoSomethingRequest` and use it to invoke the `DoSomething` operation.

```powershell
$request=[Example.DoSomethingRequest]::new()
$request.Property1="something"

$response=$proxy.DoSomething($request)
```

All the types are part of an in-memory assembly that has a random name. To help this post, this assembly is named `5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`.

# The problem

In the meantime, if the `$proxy=New-WebServiceProxy -Uri $uriDoSomething -Namespace Example` is executed again then problems start surfacing because this execution created yet another set of the same types but in a different assembly e.g. `4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`. The two invocations would have resulted in following set of types:

| Invocation | Assembly Qualified Name |
| ---------- | ----------------------- |
| 1st | `Example.DoSomethingRequest, 5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |
| 1st | `Example.DosomethingResponse, 5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |
| 2nd | `Example.DoSomethingRequest, 4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |
| 2nd | `Example.DosomethingResponse, 4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` |

For those who are not proficient with .NET, the type name is not enough to identity it in the runtime. Instead the full name of the assembly is also used to produce what you would consider a unique identifier of a type also known as **Assembly Qualified Name**.
{: .notice--tip}

`$proxy1` expects as a parameter an instance of type `Example.DoSomethingRequest, 5wx5pbrx, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null` and `$proxy2` expects `Example.DoSomethingRequest, 4qodbdsw, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`. The problem is that when code like `[Example.DoSomethingRequest]::new()` is executed then it is unclear which exact type was instantiated because we didn't specify the assembly qualified name and this is a scenario that is not typical within the .NET runtime. This can lead to situations where types from the `5wx5pbrx` assembly would be used with types from the `4qodbdsw` assembly, which leads to errors that don't make sense.

The [SOAPProxy][2] PowerShell module helps address this problem.

# SOAPProxy

| Cmdlet | Function |
| ------ | -------- | 
| `Initialize-SOAPProxy` | Initializes a proxy from a URI. A proxy can be marked as Default and be acquired without explicit mention of the URI. |
| `Get-SOAPProxy` | Finds and returns a proxy initialized by `Initialize-SOAPProxy`. When the URI is not specified, then the default one is returned. |
| `Get-SOAPProxyInfo` | List each operation available on the SOAP proxy with the expected *Request* types and returned *Response* type. |
| `New-SOAPProxyRequest` | Instantiates a request object expected by an operation in a proxy. |
| `Trace-SOAPProxy` | Helps with understanding how the request and response types of an operation are structured. This is effectively a redirection to `Trace-JSONPath` from the [JSONPath][6] module. |

The module's main functionality is to wrap the `New-WebServiceProxy` and make sure to invoke it only when necessary. The functionality is supported by `Initialize-SOAPProxy` and internally, the cmdlet maintains a sort of proxy cache within the global variables. When the parameters of `Initialize-SOAPProxy` match an existing proxy, then `New-WebServiceProxy` is not executed and the one from memory is used. The condition to match an existing proxy is the `-Uri`. The `-Uri` is also used by the `Get-SOAPProxy` cmdlet to retrieve a proxy for a given URI.

The `Initialize-SOAPProxy` can also leverage the global variables to track a default proxy. When a proxy is created as default, then all cmdlets in the module can be used without specifying the `-URI` parameter. This is a very common case because usually a script targets one endpoint and can be marked as default. Then, the rest of the script assumes that there is a default proxy which makes the code cleaner and easier to read. It increases also its maintainability.

For example, imagine a script that targets the SOAP endpoints specified in the variables `$uri1` and `$uri2`. In the following example, the proxy for `$uri1` is marked as the default one.

```powershell
# Initialize for $uri1
Initialize-SOAPProxy -URI $uri1 -AsDefault

# Initialize for $uri2
Initialize-SOAPProxy -URI $uri2
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

For the rest of the examples, we will assume that a default proxy is created like this `Initialize-SOAPProxy -URI $uri -AsDefault`.

# PowerShell Core not supported

With the [SOAPProxy][2] module depending on [New-WebServiceProxy][1], it means that PowerShell versions higher than 5.1 are not supported. Unfortunately, [New-WebServiceProxy][1] is ASMX era technology and is powered by the `System.Web.Services` assembly. Therefore, porting to PowerShell Core seems very unlucky because the assembly needs to be ported to .NET Core and that will probably never happen. ASMX is old technology and the successor is WCF but even that is not 100% supported by the .NET Core and the team has no plans to do better based on their most recent [roadmap][5] announcement. Regardless, I've created another issue on the PowerShell repository [requesting SOAP support from PowerShell][3]. In my [comment][4], it becomes clear why there is much uncertainty if we will ever get cross-platform support for SOAP in PowerShell.

# Working with the soap proxy

The [SOAPProxy][2] module offers some additional cmdlets to help with using the proxy in a scripting environment. All examples assume that a default proxy is initialized with `Initialize-SOAPProxy`. 

`Get-SOAPProxyInfo` will list the operation along with their input and output types. For example

```powershell
C:> Get-SOAPProxyInfo

Operation   RequestType                ResponseType
---------   -----------                ------------
DoSomething Example.DoSomethingRequest Example.DoSomethingResponse
```

To create an instance of `Example.DoSomethingRequest` the module offers the `New-SOAPProxyRequest` that will create one from a proxy by specifying the operation name. 

```powershell
New-SOAPProxyRequest -Operation DoSomething
```

Both `Get-SOAPProxyInfo` and `New-SOAPProxyRequest` can also work with soap proxies created outside the preview of the [SOAPProxy][2] module. This is possible because [SOAPProxy][2] manages proxies created by [New-WebServiceProxy][1]. All following code is valid and utilizes a proxy created normally

```powershell
$proxy=New-WebServiceProxy -Uri $uri
Get-SOAPProxyInfo -Proxy $proxy
$proxy|Get-SOAPProxyInfo

New-SOAPProxyRequest -Operation DoSomething -Proxy $proxy
$proxy|New-SOAPProxyRequest -Operation DoSomething
```

# Debugging and tracing the proxies

Often the request and response objects are very complicated, referencing other composite types and arrays. Without code visibility, typical with a scripting environment, this becomes very difficult and requires lots of trial and error. [JSONPath][6] offers functionality which will be discussed in another post dedicated to [JSONPath][6]. [SOAPProxy][2] offers the `Trace-SOAPProxy` with two options:
- A list of [JSONPath][7] like expressions that help understand the data contract relationships.
- A code that sets a value to every possible [JSONPath][7] expression. As a developer, you can then keep the lines that you are interested to form a valid request.

For the following examples, we'll use the `PNR_Retrieve` operation from the [Amadeus API][8]

`Trace-SOAPProxy -Proxy $doSomethingProxy -Operation PNR_Retrieve -Request` outputs 

```text
settings.printer.identifierDetail.name="String"
settings.printer.identifierDetail.network="String"
settings.printer.office="String"
settings.printer.teletypeAddress="String"
settings.options[0]="String"
retrievalFacts.retrieve.type="String"
retrievalFacts.retrieve.service="String"
retrievalFacts.retrieve.tattoo="String"
retrievalFacts.retrieve.office="String"
retrievalFacts.retrieve.targetSystem="String"
retrievalFacts.retrieve.option1="String"
retrievalFacts.retrieve.option2="String"
retrievalFacts.reservationOrProfileIdentifier[0].companyId="String"
retrievalFacts.reservationOrProfileIdentifier[0].controlNumber="String"
retrievalFacts.reservationOrProfileIdentifier[0].controlType="String"
retrievalFacts.personalFacts.travellerInformation.traveller.surname="String"
retrievalFacts.personalFacts.travellerInformation.passenger.firstName="String"
retrievalFacts.personalFacts.productInformation.product.depDate="String"
retrievalFacts.personalFacts.productInformation.product.depTime="String"
retrievalFacts.personalFacts.productInformation.product.arrDate="String"
retrievalFacts.personalFacts.productInformation.boardpointDetail.cityCode="String"
retrievalFacts.personalFacts.productInformation.offpointDetail.cityCode="String"
retrievalFacts.personalFacts.productInformation.company.code="String"
retrievalFacts.personalFacts.productInformation.productDetails.identification="String"
retrievalFacts.personalFacts.productInformation.productDetails.subtype="String"
retrievalFacts.personalFacts.ticket.airline="String"
retrievalFacts.personalFacts.ticket.ticketNumber="String"
retrievalFacts.frequentFlyer.frequentTraveller.companyId="String"
retrievalFacts.frequentFlyer.frequentTraveller.membershipNumber="String"
retrievalFacts.accounting.account.number="String"
```

`Trace-SOAPProxy -Proxy $doSomethingProxy -Operation PNR_Retrieve -Request -RenderCode` outputs 

```powershell
$requestPNR_Retrieve=New-SOAPProxyRequest -Operation "PNR_Retrieve"
$requestPNR_Retrieve=$requestPNR_Retrieve | Set-JSONPath -Path "settings.printer.identifierDetail.name" -Value "String" -PassThru |
    Set-JSONPath -Path "settings.printer.identifierDetail.network" -Value "String" -PassThru |
    Set-JSONPath -Path "settings.printer.office" -Value "String" -PassThru |
    Set-JSONPath -Path "settings.printer.teletypeAddress" -Value "String" -PassThru |
    Set-JSONPath -Path "settings.options[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.type" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.service" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.tattoo" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.office" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.targetSystem" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.option1" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.retrieve.option2" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.reservationOrProfileIdentifier[0].companyId" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.reservationOrProfileIdentifier[0].controlNumber" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.reservationOrProfileIdentifier[0].controlType" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.travellerInformation.traveller.surname" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.travellerInformation.passenger.firstName" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.product.depDate" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.product.depTime" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.product.arrDate" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.boardpointDetail.cityCode" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.offpointDetail.cityCode" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.company.code" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.productDetails.identification" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.productInformation.productDetails.subtype" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.ticket.airline" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.personalFacts.ticket.ticketNumber" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.frequentFlyer.frequentTraveller.companyId" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.frequentFlyer.frequentTraveller.membershipNumber" -Value "String" -PassThru |
    Set-JSONPath -Path "retrievalFacts.accounting.account.number" -Value "String"

```
[1]: https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-webserviceproxy?view=powershell-5.1
[2]: https://github.com/Sarafian/1ASOAP/tree/master/Source/SOAPProxy
[3]: https://github.com/PowerShell/PowerShell/issues/9838
[4]: https://github.com/PowerShell/PowerShell/issues/9838#issuecomment-504886982
[5]: https://devblogs.microsoft.com/dotnet/introducing-net-5/
[6]: https://github.com/Sarafian/1ASOAP/tree/master/Source/JSONPath
[7]: https://goessner.net/articles/JsonPath/
[8]: https://developers.amadeus.com/enterprise
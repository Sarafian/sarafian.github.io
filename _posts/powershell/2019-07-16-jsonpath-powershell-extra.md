---
title: JSONPath for composite types in PowerShell with extra tools for debugging 
excerpt: JSONPath is like XPath for XML. It is a great way to navigate and query complex data types. JSONPath is a powershell module that enables JSONPath resolutions but also initializing objects and visualizing their data structure.
tags:
- PowerShell
- JSONPath
---

Although JSONPath is a stand-alone solution to implement the [JSONPath][1] concept in PowerShell, it was developed as part of a greater problem that is discussed in [Introduction to posts about JSONPath, SOAP and Amadeus with PowerShell][4].
{: .notice--info}

# Introduction

The PowerShell module [JSONPath][2] is an implementation of Stefan's Goessner JSONPath concept as described [here][1]. The module works best with typed objects. The purpose of the module is to allow `Find` and `Set` operations on objects using expressions that are based on JSONPath. When *finding* the module is null safe. When *setting* the module will create the elements when encountering `null` values. The `set` operation currently works with typed and not dynamic or `PSObject` types. When working with composite non-dynamic types, things can get complicated and difficult and this where the module shines the most.  

The module's cmdlets are:
- `Find-JSONPath`
- `Set-JSONPath`
- `Trace-JSONPath`

# Find operation

The `Find` operation allows querying objects and returns only those that satisfy a condition for a given JSONPath. Imagine you have a list of 10 objects of the same or different types with complex data that we want to filter based on conditions that go very deep into the structure of each object. Instead of writing complicated code structures, it would be so much easier if we could define a condition for the values resolved by a JSONPath while the operation compensates for intermediate `null` vallues or arrays. We could code this with multiple layers of loops and conditions or we can use the `Find-JSONPath` cmdlet. During its operation, the cmdlet makes sure that a path can be resolved for each object and then apply the condition. If during the resolution of the path, null values are encountered then the condition is treated as `false`. If during the resolution of the path, arrays are encountered then the cmdlet will iterate over each. 

It is important to understand the `Find-JSONPath` cmdlet doesn't resolve the values into an output and neither reveal which in-between array satisfied the condition. Instead, it just filters out all objects that don't satisfy the condition as part of any possible path.

Let's assume there is a `$retrieveFacts` variable which is an array of 2 items visualized with the following XML

```xml
<retrievalFacts>
    <retrieve>
        <type>2</type>
    </retrieve>
</retrievalFacts>
<retrievalFacts>
    <retrieve>
        <type>3</type>
    </retrieve>
</retrievalFacts>
```

The following PowerShell command would return only the second `element` in the variable `$retrieveFacts`.

```powershell
$retrieveFacts|Find-JSONPath -Path "retrieve.type" -EQ 3
```

If the object was like the following xml, it would still return the 2nd `element`.

```xml
<retrievalFacts>
    <retrieve>
        <type>2</type>
    </retrieve>
</retrievalFacts>
<retrievalFacts>
    <retrieve>
        <type>3</type>
    </retrieve>
    <retrieve>
        <type>2</type>
    </retrieve>
</retrievalFacts>
```

If the path is `retrieve[1].type` or `invalid.type` then the command would return `null`.

The above command is much simpler and cleaner than its equivalent with loops and conditions. On top of the cleaner code, it is also reusable.

There are more conditions on the `Find-JSONPath` and the intention is to match them with the `Where-Object` because internally, the last filtering is achieved with the `Where-Object` cmdlet.

# Set operation

This functionality is particularly useful when working with composite types. In a scripting language, we would not like to initialize every property when it is `null` nor initialize arrays. For example, if a property is an array of composite types, then one should first initialize the array and then each item in the array. This would require explicit knowledge of the types involved and checking on each step if the value is null. This is expected from a language like C# but in a scripting environment like PowerShell we would rather do set the value on an expression like `retrievalFacts.retrieve.type=3`.

To create the object that is represented with the above XML fragment, execute the following PowerShell script.

```powershell
$obj | Set-JSONPath -Path "retrievalFacts[1].retrieve.type" -Value 2
$obj | Set-JSONPath -Path "retrievalFacts[0].retrieve.type" -Value 3
```

The above commands are much simpler and cleaner than their equivalents with loops, conditions and implicit knowledge of types. On top of the cleaner code, it is also reusable because the types are discovered by the `Set-JSONPath` cmdlet using reflection. If applicable, then the cmdlet works also with a list of objects of different types, for example `@($objOfType1,$objOfType2) | | Set-JSONPath -Path "expression" -Value $value`

**Note** that when the JSONPath goes over array properties the following need to be considered:
- When the JSONPath refers to an array without explicitly specifying an index, a warning will be raised but still, the array will be initialized with 1 item. For example `retrievalFacts.retrieve.type` is the same as with `retrievalFacts[0].retrieve.type`.
- When an array property is involved, it will be initialized once when null. The size of the array will match the index specified. For example `retrievalFacts[0]` or `retrievalFacts` will create an array of 1 item but `retrievalFacts[10]` will create 10. All following statements need to be constraint by the maximum length, otherwise, there will be an out of bounds error. For example `retrievalFacts[20]` will not work. Therefore it is always best to start with the maximum size like in the example above.

# Trace operation

When working with a composite type structure in PowerShell, it is anything but simple. Assuming one has access to the C# code of the composite types, things would be much easier but there are two problems with this. Not everyone is comfortable with .NET and PowerShell is not .NET although it is built on top of it. Also, the source code of composite types is not always available. Often types are a by-product of previous instruction (e.g. `New-WebServiceProxy`) that injects in the session new types that we need to use. As already mentioned, the module was developed in the context of SOAP automation and specifically automation over [Amadeus API][3] which delivers very complicated and nested types and in this case, the source code is not available but generated in memory. If you want to read more about `New-WebServiceProxy`, please refer to [Improved SOAP proxies management in PowerShell][16]. Though `Find-JSONPath` and `Set-JSONPath` offer a simplification with JSONPaths, we still need to how the types are connected to construct the JSONPaths. For this reason, the `Trace` functionality was implemented to help accelerate and make easy the development experience. 

Depending on whether we want to initialize an object or process it, we need to visualize either the types or instances respectively:
- When `setting`, we need all possible JSONPaths.
- When `finding`, we only need to see the JSONPaths that lead to set values.

When `setting`, there are two possible outputs with `Trace-JSONPath`:
- All valid JSONPaths. We use this to understand the structure.
- Code that sets a random value for all valid JSONPaths. We can use this to directly copy-paste code, keep the lines we need and finally adjust the values we need.

As an example of relative complex types, let's use the composite types defined in the [JSONPath][2] module for testing purposes

```
using System;

namespace JSONPath.Pester
{
    public class Root
    {
        public string StringSingle{get;set;}
        public string[] StringArray{get;set;}

        public int IntSingle{get;set;}
        public int[] IntArray{get;set;}

        public Type1 Type1Single{get;set;}
        public Type1[] Type1Array{get;set;}

    }
    public class Type1
    {
        public string StringSingle{get;set;}
        public string[] StringArray{get;set;}

        public int IntSingle{get;set;}
        public int[] IntArray{get;set;}

        public Type2 Type2Single{get;set;}
        public Type2[] Type2Array{get;set;}
    }
    public class Type2
    {
        public string StringSingle{get;set;}
        public string[] StringArray{get;set;}
        
        public int IntSingle{get;set;}
        public int[] IntArray{get;set;}
    }    
}
```

## Tracing on types to help compose JSONPaths

The `Trace-JSONPath -Type ("JSONPath.Pester.Root" -as [type])` command outputs the following permutations:

```text
IntArray[0]=0
IntSingle=0
StringArray[0]="String"
StringSingle="String"
Type1Array[0].IntArray[0]=0
Type1Array[0].IntSingle=0
Type1Array[0].StringArray[0]="String"
Type1Array[0].StringSingle="String"
Type1Array[0].Type2Array[0].IntArray[0]=0
Type1Array[0].Type2Array[0].IntSingle=0
Type1Array[0].Type2Array[0].StringArray[0]="String"
Type1Array[0].Type2Array[0].StringSingle="String"
Type1Array[0].Type2Single.IntArray[0]=0
Type1Array[0].Type2Single.IntSingle=0
Type1Array[0].Type2Single.StringArray[0]="String"
Type1Array[0].Type2Single.StringSingle="String"
Type1Single.IntArray[0]=0
Type1Single.IntSingle=0
Type1Single.StringArray[0]="String"
Type1Single.StringSingle="String"
Type1Single.Type2Array[0].IntArray[0]=0
Type1Single.Type2Array[0].IntSingle=0
Type1Single.Type2Array[0].StringArray[0]="String"
Type1Single.Type2Array[0].StringSingle="String"
Type1Single.Type2Single.IntArray[0]=0
Type1Single.Type2Single.IntSingle=0
Type1Single.Type2Single.StringArray[0]="String"
Type1Single.Type2Single.StringSingle="String"
```

The same output can be used to render a code fragment that sets random values with the `Set-JSONPath`. The following `Trace-JSONPath -Type ("JSONPath.Pester.Root" -as [type]) -RenderCode` outputs the following PowerShell fragment

```powershell
$obj=New-Object -TypeName "JSONPath.Pester.Root"
$obj=$obj|Set-JSONPath -Path "Type1Single.Type2Single.StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.IntArray[0]" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[0].StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[0].StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[0].IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[0].IntArray[0]" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Single.StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Single.StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Single.IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Single.IntArray[0]" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Single.StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Single.StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Single.IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Single.IntArray[0]" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Array[0].StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Array[0].StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Array[0].IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Array[0].Type2Array[0].IntArray[0]" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Array[0].StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Array[0].StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "Type1Array[0].IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "Type1Array[0].IntArray[0]" -Value 0 -PassThru |
    Set-JSONPath -Path "StringSingle" -Value "String" -PassThru |
    Set-JSONPath -Path "StringArray[0]" -Value "String" -PassThru |
    Set-JSONPath -Path "IntSingle" -Value 0 -PassThru |
    Set-JSONPath -Path "IntArray[0]" -Value 0
```

Take the above code, keep only the lines that match the values you want to set, copy-paste sections for more items in an array (don't forget to place the higher index first), then adapt the values themselves and you have the code that initializes a composite object. Much easier and cleaner than loops and conditions and we don't need to know that `Type1Array` is of type `Pester.JSONPath.Type1`.

## Tracing on object instance to extract JSONPaths with values

Let's assume we initialize an object like this

```powershell
$obj=$obj |    Set-JSONPath -Path "Type1Array[1].Type2Single.StringSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Single.IntSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Single.StringArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Single.IntArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Array[1].StringSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Array[1].IntSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Array[1].StringArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Array[1].Type2Array[1].IntArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.StringSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.IntSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.StringArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Single.IntArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[1].StringSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[1].IntSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[1].StringArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "Type1Single.Type2Array[1].IntArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "StringSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "IntSingle" -Value 1 -PassThru |
    Set-JSONPath -Path "StringArray[1]" -Value 2 -PassThru |
    Set-JSONPath -Path "IntArray[1]" -Value 2 -PassThru
```

On the `$obj` we just initialized we execute `Trace-JSONPath -InputObject $obj` to receive

```text
IntArray[0]=0
IntArray[1]=2
IntSingle=1
StringArray[0]=""(null)
StringArray[1]="2"
StringSingle="1"
Type1Array[1].Type2Array[1].IntArray[0]=0
Type1Array[1].Type2Array[1].IntArray[1]=2
Type1Array[1].Type2Array[1].IntSingle=1
Type1Array[1].Type2Array[1].StringArray[0]=""(null)
Type1Array[1].Type2Array[1].StringArray[1]="2"
Type1Array[1].Type2Array[1].StringSingle="1"
Type1Array[1].Type2Single.IntArray[0]=0
Type1Array[1].Type2Single.IntArray[1]=2
Type1Array[1].Type2Single.IntSingle=1
Type1Array[1].Type2Single.StringArray[0]=""(null)
Type1Array[1].Type2Single.StringArray[1]="2"
Type1Array[1].Type2Single.StringSingle="1"
Type1Single.Type2Array[1].IntArray[0]=0
Type1Single.Type2Array[1].IntArray[1]=2
Type1Single.Type2Array[1].IntSingle=1
Type1Single.Type2Array[1].StringArray[0]=""(null)
Type1Single.Type2Array[1].StringArray[1]="2"
Type1Single.Type2Array[1].StringSingle="1"
Type1Single.Type2Single.IntArray[0]=0
Type1Single.Type2Single.IntArray[1]=2
Type1Single.Type2Single.IntSingle=1
Type1Single.Type2Single.StringArray[0]=""(null)
Type1Single.Type2Single.StringArray[1]="2"
Type1Single.Type2Single.StringSingle="1"
```

In the output the following concepts are embedded. When a path ends to a property
- that is a primitive it will be wrapped in quotes (`"`) when `string`.
- that is not primitive and is `null`, then it won't show.
- that is an empty or `null` string , then it will render like `=""(null)` because for PowerShell strings that are `$null` or empty are considered the same.
- that is an int that was not defined, then it will still render like `=0` because `0` is the default value.
- that is a bool then it will render like `=true` or `=false`.


# Why not Get operation?

Initially, the module was created with a `Get-JSONPath` cmdlet but the issue soon surfaced of how to provide a meaningful output when the JSONPath includes arrays. An idea is to output a recordset with properties of the JSONPath and Value, similar to the `Trace-JSONPath`.

# Future ideas

It would be great if the JSONPaths would become complex. For example, when encountering a property that is an array, we would first like to filter them with a nested JSONPath condition and then continue with the original JSONPath. This would be like specifying the index in the array which proactively filters the array and keeps only the one referenced by the index. Here is an example

`$obj|Find-JSONPath -Path "Type1Array[Type2Single.IntSingle -EQ 0].Type2Array[0].IntSingle" -NE 0`

With this example, we would be filtering for all objects that
- have an element in the `Type1Array` property the relative value resolved by the JSONPath `Type2Single.IntSingle` would be `0`.
- have an element in the `Type1Array` property the relative value resolved by the JSONPath `Type2Array[0].IntSingle` would not be `0`.

[1]: https://goessner.net/articles/JsonPath/
[2]: https://github.com/Sarafian/1ASOAP/tree/master/Source/JSONPath
[3]: https://developers.amadeus.com/enterprise
[4]: {% link _posts/2019-06-24-powershell-jsonpath-soap-proxy-amadeus.md %}
[5]: {% link _posts/powershell/2019-06-28-powershell-soap-proxy-new-webserviceproxy.md %}

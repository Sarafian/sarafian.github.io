---
title: Add semantic versioning capabilities to PowerShell
excerpt: The SemVerPS PowerShell module offers the ability to work with Semantic Version in PowerShell
tags:
- PowerShell
- SemVerPS
---

The [SemVerPS] PowerShell module offers the ability to work with [Semantic Version] utilizing the .net implementation from [Max Hauser's SemVer] repository. With this module, it is possible to:
- Work with [Semantic Version] as typed object leveraging or comparison operators
- Enhance object with a `NoteProperty` that contains a [Semantic Version].
- Filter and/or test above enhanced list of objects. 
  - This could be very useful if for example a list of items is retrieved where part of the name is a semantic version and we want to filter the list.

## Basic usage with ConvertTo-SemVer

Use the `ConvertTo-SemVer` to convert strings to semantic version objects. For example:

```powershell
ConvertTo-SemVer -Version "1.0.0"
```
```text
Major      : 1
Minor      : 0
Patch      : 0
Prerelease : 
Build      : 
```
```powershell
ConvertTo-SemVer -Version "1.0.1-alpha+1" -Strict
```
```text
Major      : 1
Minor      : 0
Patch      : 1
Prerelease : alpha
Build      : 1
```


Use the `Test-SemVer` to test if a string or a semantic version is stable or prerelease. For example:

```powershell
Test-SemVer -InputObject "1.0.0"
#True

Test-SemVer -InputObject "1.0.0" -Stable
#True

Test-SemVer -InputObject "1.0.0-alpha+1" -PreRelease alpha
#True
```

## Add semantic versioning member to existing objects

Often there is a need to attach a semantic version notation to an object or a list of objects. Use the `Add-SemVerMember` to add a member where the semantic version value is derived either from an expression or a script block. By default the semantic version is add to a property named `SemVer` but this can be changed by defining the `Property` parameter. For example:

```powershell
[pscustomobject]@{Name="example-1.0.0"}|Add-SemVerMember -Expression 'Name.Replace("example-","")' -PassThru
[pscustomobject]@{Name="example-1.0.0"}|Add-SemVerMember -ScriptBlock {$_.Name.Replace("example-","")} -PassThru -Name "AnotherProperty"
```
```text
Name          SemVer
----          ------
example-1.0.0 1.0.0


Name          AnotherProperty
----          ---------------
example-1.0.0 1.0.0


```

As an extension of the `Test-SemVer`, use the `Limit-SemVer` to filter objects based on semantic version conditions. Each object can be in the form of a string, or a semantic version instance or an object that has been enhanced with `Add-SemVerMember`. For example:

```powershell
$versions=@(
    "1.0.0"
    "1.0.0-alpha+1"
)
$versions|Limit-SemVer -Stable
$versions|Limit-SemVer -Prerelease alpha
```
```text
1.0.0

1.0.0-alpha+1
```

```powershell
$objectsWithVersions=@(
    "example-1.0.0"
    "example-1.0.0-alpha+1"
)|Foreach-Object {
    [pscustomobject]@{Name=$_}| Add-SemVerMember -Expression 'Name.Replace("example-","")' -PassThru  
}
$objectsWithVersions
$objectsWithVersions|Limit-SemVer -Stable
$objectsWithVersions|Limit-SemVer -Prerelease alpha
```
```text
Name          SemVer
----          ------
example-1.0.0 1.0.0

Name                  SemVer
----                  ------
example-1.0.0-alpha+1 1.0.0-alpha+1
```

[Semantic Version]: http://semver.org/
[Max Hauser's SemVer]: maxhauser/semver
[SemVerPS]: https://www.powershellgallery.com/packages/SemVerPS/
---
title: Upgrade from Pester v4 to v5
excerpt: Upgrading Pester from version 4 to 5 is not easy. Many things are broken and this post is about discussing what to expect and what to do.
tags:
- PowerShell
- Pester
---

## Background

Recently somebody submitted a pull request in my [SemVerPS] which failed with the [Appveyor]'s CI pipeline. I've not been hands on for a while and it took me some time to figure out that the tests were failing and why. It turns out that there has been a new version (v5) of Pester which [broke compatibility][breaking changes] compared with the previous version (v4). Soon, it became clear to me that that I would have to make significant changes which meant that I had to work on my [PowerShellTemplate] repository, where advanced Pester test cases are also covered. 

All the changes are available in this [pull request] and in the following sections I'll focus on the major adaptations required. To help showcase the requires changes, I'll provide the relevant extracts from the V4 and V5. Keep in mind that the extracts are relevant to the [PowerShelltTemplate] repository.

## Invoke-Pester adaptations

The [Invoke-Pester] cmdlet has many changes when used in an "advanced" mode. Basic execution still works but when the cmdlet is integrated in a CI pipeline like I do in the [PowerShelltTemplate] repository with the `CI\Invoke-Pester.ps1`, then you probably need to use some of the advanced functionality. The main difference is that with version 5, the cmdlet is driven by a configuration variable that is initialized by the [New-PesterConfiguration] cmdlet. The cmdlet's documentation page provides an explanation for all options but for some it is not clear what they really do. 

**V4 extracts**

```powershell
$splat=@{
    Script=$srcPath
    PassThru=$true
    OutputFormat="NUnitXml"
    OutputFile=$outputFile
    ExcludeTag=$ExcludeTag
    Tag=$Tag
}

# Activate code coverage
if($CodeCoverage)
{
    $codeCoveragePath=$outputFile.Replace(".xml",".codecoverage.xml")
    $splat+=@{
        CodeCoverage=Get-ChildItem -Path $srcPath -Exclude @("*.Tests.ps1","*.NotReady.ps1","Src\Tests\**") -Filter "*.ps1" -Recurse|Select-Object -ExpandProperty FullName
        CodeCoverageOutputFile=$codeCoveragePath        
    }
}

$pesterResult=Invoke-Pester @splat

```

**V5 extracts**

```powershell
# https://pester-docs.netlify.app/docs/commands/New-PesterConfiguration
$pesterConfiguration=New-PesterConfiguration
$pesterConfiguration.Run.Path=$srcPath
$pesterConfiguration.Run.PassThru=$true
$pesterConfiguration.Run.Path=$srcPath
$pesterConfiguration.Filter.Tag=$Tag
$pesterConfiguration.Filter.ExcludeTag=$ExcludeTag

$pesterConfiguration.TestResult.Enabled=$true
$pesterConfiguration.TestResult.OutputFormat="NUnitXml"
$pesterConfiguration.TestResult.OutputPath=$outputFile

$pesterConfiguration.Output.Verbosity="Detailed"
# If you want full log uncomment
#$pesterConfiguration.Output.Verbosity="Diagnostic"

# Activate code coverage
if($CodeCoverage)
{
    $pesterConfiguration.CodeCoverage.Enabled=$true
#    $pesterConfiguration.CodeCoverage.OutputFormat="JaCoCo"
    $pesterConfiguration.CodeCoverage.OutputPath=$outputFile.Replace(".xml",".codecoverage.xml")
}

$pesterResult=Invoke-Pester -Configuration $pesterConfiguration
```

Notice that most features were moved into the configuration variable but there are also some new welcoming options especially with regards to diagnostics. I chose `Detailed` for `$pesterConfiguration.Output.Verbosity` because I found it to be the right balance between knowing what Pester is doing and logging. In the same [pull request] you will also notice that I moved the following block from a global script check to specifically within the AppVeyors block. This is because the new version reacts differently when there are test errors. There are configuration variables for better control and maybe I missed something but I found this to be the easiest adaptation. You need to first upload the test results to AppVeyor and therefore you can't instruct Pester to exit the script from within Pester. This is also documented in AppVeyor's [running test] page.

```powershell
if ($pesterResult.FailedCount -gt 0) { 
     throw "$($pesterResult.FailedCount) tests failed."
}
```

## Test file adaptations

Big differences in the test files `*Tests.ps1` are:

- Everything has to run in a block defined in a Pester function like `BeforeAll` or `BeforeEach`. They also adjusted the `New-Fixture` cmdlet.
- All `Should` expressions need to use `-` with the `Be`, `Throw` etc assertions.
- `Throw` statements used to match text without wildcards. Now they are matched by comparison unless wildcards `*` are provided similar to `-Like` statements.

**V4 extracts**

```powershell
$here = Split-Path -Parent $MyInvocation.MyCommand.Path
$sut = (Split-Path -Leaf $MyInvocation.MyCommand.Path) -replace '\.Tests\.', '.'
. "$here\$sut"

It "Just invoke" {
        Get-M1|Should BeExactly "M1"
}

It "Error" {
        {throw "error"}|Should Throw "error"
}

```

**V5 extracts**

```powershell
BeforeAll {
    . $PSCommandPath.Replace('.Tests.ps1', '.ps1')
}

It "Just invoke" {
        Get-M1|Should BeExactly "M1"
}

It "Error" {
        {throw "error"}|Should -Throw "*error*"
}
```

## Mocking and InModuleScope adaptations

The hardest part was with mocking modules, leveraging common functionality (like a random generator) and doing all this within an `InModuleScope` block. In V4 there were a lot of gaps but the implementations were less verbose. The new strictness is welcome but there is now more code required especially. This is because the `InModuleScope` is really a new scope (hence the name) and won't recognize local variables from other blocks, like e.g. a random string.

To better understand the following differences, I need to first explain that in my code, I use a common function to generate random strings and numbers. I do this, to make sure that all unit tests work with clean values and nothing is overlooked. This is important, because the scripting nature of PowerShell allows for left over variables from within the same block or a previous execution which sometimes don't raise errors. My random generator is implemented in the `Get-RandomValue` cmdlet and is found in `Src/Tests/Cmdlets-Helpers/Get-RandomValue.ps1`. 

The `Get-RandomValue.ps1` got also adjusted because now it is not necessary to define it as global function. In the past the function would be declared as `function global:Get-RandomValue` but now it can be a normal one and be declared as `function Get-RandomValue`.

The goal is to test and mock the private cmdlet `Get-M1Private` which is called by the public `Get-M1` cmdlet. Notice that when mocking the `Get-M1Private`, I always use a fresh random string using `Get-RandomValue`

**V4 file**

```powershell

& $PSScriptRoot\..\..\..\Modules\Import-M1.ps1
# Dot sourcing
. $PSScriptRoot\..\..\Cmdlets-Helpers\Get-RandomValue.ps1

Describe -Tag @("M1","Module","InModuleScope") "InModuleScope M1" {
    InModuleScope M1 {
        It "Get-M1Private" {
            Get-M1Private| Should BeExactly "M1 Private"
        }
        It "Get-M1" {
            Get-M1| Should BeExactly "M1"
        }
    }
}

Describe -Tag @("M1","Module","InModuleScope","MockPrivate") "InModuleScope M1 Mock private" {
    InModuleScope M1 {
        $mockedValue=Get-RandomValue -String
        Mock Get-M1Private {
            $mockedValue
        }
        It "Get-M1Private Mocked" {
            Get-M1Private| Should BeExactly $mockedValue
        }
        It "Get-M1" {
            Get-M1| Should BeExactly $mockedValue
        }
    }
}

Describe -Tag @("M1","Module") "M1" {
	Describe -Tag @("M1","Module") "M1" {
        Get-M1| Should -BeExactly "M1"
    }
}

Remove-Module -Name M1 -Force
```



A couple of things that require special attention with the V5 adaptations:

- The `InModuleScope` block needs to move inside each `It` block. This is until version `5.3.0` as mentioned in this [issue] raised on Pester's gitub repository. 
- The variable `mockedValue` which is assigned in the `BeforeEach` block must now be provided to the `InModuleScope` block as a function parameter because without the parameter the `Get-M1Private| Should -BeExactly $mockedValue` will fail because the expected value is actually `$null`.
- As the module `M1` is imported inside a `BeforeAll` block, it must also be removed inside an `AfterAll` block.

**V5 file**

```powershell
BeforeAll {
    & $PSScriptRoot\..\..\..\Modules\Import-M1.ps1
    . $PSScriptRoot\..\..\Cmdlets-Helpers\Get-RandomValue.ps1
}

Describe -Tag @("M1","Module","InModuleScope") "InModuleScope M1" {
    It "Get-M1Private" {
        InModuleScope M1 {
            Get-M1Private| Should -BeExactly "M1 Private"
        }
    }
    It "Get-M1" {
        Get-M1| Should -BeExactly "M1"
    }
}

Describe -Tag @("M1","Module","InModuleScope","MockPrivate") "InModuleScope M1 Mock private" {
    BeforeEach {
        $mockedValue=Get-RandomValue -String
        Mock -ModuleName M1 Get-M1Private {
            $mockedValue
        }
        $inModuleScopeParameters = @{
            mockedValue = $mockedValue
        }

    }
    It "Get-M1Private Mocked" {
        InModuleScope M1 -Parameters $inModuleScopeParameters {
            param($mockedValue)
            Get-M1Private| Should -BeExactly $mockedValue
        }
    }
    It "Get-M1" {
        Get-M1| Should -BeExactly $mockedValue
    }
}

Describe -Tag @("M1","Module") "M1" {
	Describe -Tag @("M1","Module") "M1" {
        Get-M1| Should -BeExactly "M1"
    }
}
AfterAll {
    Remove-Module -Name M1 -Force
}
```

## Final thoughts

What I didn't do is adapt all `Describe` and `It` blocks and declare the name properly using the `-Name` parameter. I expect that in the next version I'll get a similar error with the `This whole Legacy-parameter set is deprecated` that `Invoke-Pester throws when not using the configuration variable with legacy options.

Overall, I feel that Pester has improved and that this was a step to the right direction but it needs getting used to it. From my experience, the `InModuleScope` adaptations were the hardest ones to figure out. Some extra quirks had to be addressed in my other repositories [MarkdownPS] and [SemVerPS].

If you are an owner of a PowerShell module with full unit tests and code coverage, then I hope I hope you found this post useful.

[MarkdownPS]: https://www.powershellgallery.com/packages/MarkdownPS/
[SemVerPS]: https://www.powershellgallery.com/packages/SemVerPS/
[AppVeyor]: https://www.appveyor.com/
[breaking changes]: https://pester-docs.netlify.app/docs/migrations/breaking-changes-in-v5
[PowerShellTemplate]: https://github.com/Sarafian/PowerShellTemplate
[Invoke-Pester]: https://pester-docs.netlify.app/docs/commands/Invoke-Pester
[New-PesterConfiguration]: https://pester-docs.netlify.app/docs/commands/New-PesterConfiguration
[pull request]: https://github.com/Sarafian/PowerShellTemplate/pull/6
[running tests]: https://www.appveyor.com/docs/running-tests/
[issue]: https://github.com/pester/Pester/issues/2009
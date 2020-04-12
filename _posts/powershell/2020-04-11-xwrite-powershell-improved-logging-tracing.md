---
title: Enhance PowerShell tracing and logging for the console and transcriptions
excerpt: PowerShell tracing capabilities are very much lacking. XWrite offers the means to enrich the output of each line with tokens such as the invoker's type, invoker's name and timestamp.
tags:
- PowerShell
- XWrite
- Transcription
---

## Background

[XWrite] was originally created for PowerShell `4` but it works flawlessly with the latest and greatest of PowerShell (`7.0.0`). It is a 100% script-powered module and explicit effort was made to keep it as less intrusive as possible to PowerShell given the sensitive nature of the affected cmdlets.

The module was created because back then I was building a very complicated CI/CD pipeline for deploying [SDL Knowledge Center] on [Amazon Web Services]. As part of this effort and as I matured into the technology I realized some of the shortcomings of PowerShell and thought of some enhancements I would like for myself.

- PowerShell's default tracing to the console or transcription files is lacking. To give an example, I would love to see, which cmdlet or script wrote something as debug, verbose, information, warning or even an error. The script developer needs to always add this information in the body of the output-ed entry which makes tracing annoying for the developer and the code-base dirty.  
- PowerShell is missing a seamless redirection of the progress into the console and transcription. Because of this, the developer of the script needs to code both lines for nice interactive user experience but also a solid trace when the script executes in non-interactive mode. I also don't like the rendering of the `Write-Progress`'s output which often interferes with the console and is different per host (e.g. PowerShell, PowerShell ISE, PWSH, VSCode etc). I like the concept though and I would like to have the option to redirect it to the console.
- PowerShell is missing a simple and fast way to enable full tracing to the console. I don't want to modify multiple preference variables e.g. `$DebugPreference` without autocomplete. There has to be something simpler and faster.

For these reasons and to improve my experience with PowerShell as a developer and the transcription analysis, I had developed back then (2007) the [XWrite] module. It turns out that it works so well, that I started using it with every PowerShell session by loading the module through my profile.

Here is a glimpse of the normal and enhanced outputs

| PowerShell normal output | PowerShell output with XWrite enabled |
| :----------------------- | -------------------------------------:|
| ![PowerShell normal](https://raw.githubusercontent.com/Sarafian/XWrite/master/Images/PS-Normal.png)  | ![XWriteEnabled](https://raw.githubusercontent.com/Sarafian/XWrite/master/Images/XWrite-Enabled.png) |

## XWrite features

The module can:

- Enable full tracing for all preference variables and rollback to the original settings.
- Enhance the output of the following core (`Microsoft.PowerShell.Utility`) cmdlets:
    - `Write-Host`
    - `Write-Debug`
    - `Write-Verbose`
    - `Write-Information`
    - `Write-Warning`
- Add inline additional output for the native `Write-Progress` cmdlet using: 
    - `Write-Host`
    - `Write-Debug`
    - `Write-Verbose`
    - `Write-Information`
- Seamless support with PowerShell transcriptions. 

The output of each of the `Write-*` cmdlets can be enhanced with the following extra information:

- The **Source** that is the source script or module name that executed the `Write-` cmdlet.
- The **Caller** that is the name of the script or module that executed the `Write-` cmdlet.
- The **Date** that is the date part of the current date.
- The **Time** that is the time part of the current date.

The above functionality is enabled or disabled by use of the following groups of cmdlets:
- To enable/disable different levels of preference variables:
  - `Set-XGlobalTrace`
  - `Undo-XGlobalTrace`
- To enable/disable the enhanced output
  - `Enable-XWrite`
  - `Disable-XWrite`
- To enable/disable progress output redirection
  - `Enable-XWriteProgress`
  - `Disable-XWriteProgress`

## Setting up for the demo

To demonstrate let's first create a sample function `Test-MyXWrite` that just writes the preference value per `Debug`,`Verbose`,`Information` and `Warning` using the respected cmdlets `Write-Debug`, `Write-Verbose`,`Write-Information` and `Write-Warning`. The same is written to the host using the `Write-Host` cmdlet.

```powershell
# Sample function that uses Write-* commands without any changes
function Test-MyXWrite
{
    param(
    )

    $message=@(
        "DebugPreference=$DebugPreference"
        "VerbosePreference=$VerbosePreference"
        "InformationPreference=$InformationPreference"
        "WarningPreference=$WarningPreference"
    )
    
    $message|ForEach-Object {
        Write-Host $_
    }
    
    Write-Debug "DebugPreference=$DebugPreference"
    Write-Verbose "VerbosePreference=$VerbosePreference"
    Write-Information "InformationPreference=$InformationPreference"
    Write-Warning "WarningPreference=$WarningPreference"
}
```

Then we first enable all preference variables using `Set-XGlobalTrace`, invoke the `Test-MyXWrite` and undo the changes of `Set-XGlobalTrace` using the `Undo-XGlobalTrace`.

```powershell
# Enable full trace 
Set-XGlobalTrace -ForAll

# Invoke test function to execute the Write-* commands
Test-MyXWrite

# Disable full trace. Rollback to original settings
Undo-XGlobalTrace
```

This is how it looks:

![PowerShell normal](https://raw.githubusercontent.com/Sarafian/XWrite/master/Images/PS-Normal.png)

## Demo of XWrite enhanced output

To showcase the enhanced output, we just need to enable it using `Enable-XWrite` and the rest of the demo is the same as before.

```powershell
# Enable XWrite default output enhancement
Enable-XWrite -ForAll

# Enable full trace 
Set-XGlobalTrace -ForAll

# Invoke test function to execute the Write-* commands
Test-MyXWrite

# Disable full trace. Rollback to original settings
Undo-XGlobalTrace
```

This is how it looks:

![XWriteEnabled](https://raw.githubusercontent.com/Sarafian/XWrite/master/Images/XWrite-Enabled.png)

Notice that the name of the invoker, that is `Test-MyXWrite` shows up as well.

## Demo of XWrite advanced enhanced output

The enhanced output could be configured to output more information with additional tokens such as the invoker's type and the timestamp using additional parameters of `Enable-XWrite`.

```powershell
# Enable XWrite advanced output enhancement
Enable-XWrite -ForAll -Source -Date -Time

# Enable full trace 
Set-XGlobalTrace -ForAll

# Invoke test function to execute the Write-* commands
Test-MyXWrite

# Disable full trace. Rollback to original settings
Undo-XGlobalTrace
```

This is how it looks:

![XWriteEnabled](https://raw.githubusercontent.com/Sarafian/XWrite/master/Images/XWrite-Enabled-Full.png)

Notice that the source `Function` and a timestamp are added extra to the name `Test-MyXWrite.

## Formatting of advanced output

The module will add a prefix to the output with the following logic:

- The invoker's name `Test-MyXWrite` is always present.
- When the `-Source` parameter is used then the source of the invoker is added. For example
  - `Function` when the invocation originates from a function declared in-script as above.
  - `ModuleName` when the invocation originates from a function that is part of a module.
  - `FileName` when the invocation originates from a script file.
  - `<SCRIPT>` when the invocation originates from a script block.
- When `-Date` or `-Time` is used, then their respected values are added.
- When the `-Separator` parameter is used, then the default `:` separator is adapted.
- When the `-Format` parameter is specified then the provided custom format is used. The valid tokens are:
  - `%source%`
  - `%caller%`
  - `%date%`
  - `%time%`.

This is a consolidated matrix"

| Token in `-Format` parameter | Parameter | Example value | Remarks |
|:-------- | --------- | ------------- | ------- |
| `%source%` | `-Source` | `Function` | The invoker's type |
| `%caller%` | No parameter. Always active | Test-MyXWrite | The invoker's name |
| `%date%` | `-Date` | 20170804 | the date stamp formatted as `yyyyMMdd` |
| `%time%` | `-Time` | 10:57:27.858 | the time stamp formatted as `hh:mm:ss.fff` |

## Considerations

The module works by placing a global implementation overwrite of the respected cmdlet over the core one from the `Microsoft.PowerShell.Utility` namespace. This effectively overwrites the default behavior of each `Write-*` cmdlet. With this understanding, the following need to be always considered:

- When the invocation is like `Microsoft.PowerShell.Utility/Write-Host` then the global overwrite is bypassed. 
- Binary modules, target through compiling the namespaced `Write-*` functions and because of that [XWrite] will not enhance their output.

## Impact on PowerShell's core functionality

The impact is minimum because [XWrite] utilizes the above considerations and lets PowerShell's default `Write-*` functionality do the heavy lifting. This means that the module doesn't interfere with PowerShell and just modifies the payload of the used parameters.

## Best practices

Here are some best practices I've picked up over the years:

- Use `Enable-XWrite` and `Enable-XWriteProgress` before `Set-XGlobalTrace` because they write to `DEBUG` the body of the overwrites and that can clutter the output.
- If you want to sneak pick in the implementation of each of the overwriting functions then use `Set-XGlobalTrace` first and then invoke `Enable-XWrite` and `Enable-XWriteProgress`
- You don't have to do anything about the transcription output. This is managed by PowerShell by redirecting the output to a file. Since `XWrite` used the PowerShell's own `Write-*` functionality it just works.
- Try to invoke the `Enable-XWrite` and `Enable-XWriteProgress` as early as possible in your session, so any transcriptions are enhanced from the beginning.
  - For your interactive sessions, just add in your profile script.
  - For non-interactive flows, this could be part of the pipeline.

[XWrite]: https://www.powershellgallery.com/packages/XWrite/
[XWrite Github]: https://github.com/Sarafian/XWrite/
[Amazon Web Services]: https://aws.amazon.com/ 
[SDL Knowledge Center]: http://sdl.github.io/#dita
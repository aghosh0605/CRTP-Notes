# PowerShell Basics

### Useful Symbols

```powershell
%	Foreach-Object
?	Where-Object
$_      The variable for the current value in the pipe line

Examples
1,2,3 | %{ write-host $_ } will print 1,2,3
1,2,3 | ?{$_ -gt 1} will print 2,3
```

Limit results of a Powershell command by using `Select-Object`

```powershell
Get-DomainObjectAcl | Select-Object -First 2
```

### Language Mode

Read more about different language modes from [here](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_language_modes?view=powershell-7.5). &#x20;

{% hint style="info" %}
_FullLanguage_ mode is what attackers prefer most. _Constrained Language_(introduced in PowerShell 3.0) mode in PowerShell is the language mode that allows only signed binaries to run.
{% endhint %}

```powershell
$ExecutionContext.SessionState.LanguageMode
```

### Execution Policy

Several ways to bypass

```batch
powershell -ExecutionPolicy bypass
powershell -c <cmd>
powershell -encodedcommand $env:PSExecutionPolicyPreference="bypass" 
```

### Load PowerShell script

```powershell
. C:\AD\Tools\PowerView.ps1
```

### Import a module

```powershell
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

### List available module commands

```powershell
Get-Command -Module <module_name>
Get-Help <module_name>
```

# PowerShell Basics

Interact with PowerShell using the **\[ADSI]**, **.NET Classes System.DirectoryServices.Active Directory**, **Native Executable**, **WMI using PowerShell**, **Active Directory Module**.&#x20;

PowerShell Detections after Windows PowerShell 5.1(after 2016) - **System-wide Transcription** (Logging in Files), **Script Block Logging** (Event ID 4103 & 4104), **Antimalware Scan Interface** (AMSI), **Constrained Language Mode** (CLM) - **Integrated with AppLocker and WDAC** (Windows Defender Application Control)

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

### Get username & Computername

```powershell
# Get all environment variables
ls env:
# set computername in winrs
$env:COMPUTERNAME
# set username in winrs
$env:username
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

### Powershell State of Settings

```powershell
Get-PSReadLineOption
```

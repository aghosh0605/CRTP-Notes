# General

### Tasklist

```batch
tasklist /svc
```

### ENV variables

```batch
set
ls env:
$env:computername
$env:username
```

### Detailed OS Information

```batch
systeminfo
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" 
```

### Patches and Updates

{% tabs %}
{% tab title="Cmd" %}
```batch
wmic qfe get Caption,Description,HotFixID,InstalledOn 
```
{% endtab %}

{% tab title="PowerShell" %}
```powershell
Get-HotFix | ft -AutoSize
```
{% endtab %}
{% endtabs %}

### Understand Powershell or CMD

Run `asdfgh` or any arbitary string that is not a command.

* In CMD, you'll get something like:\
  `'asdfgh' is not recognized as an internal or external command...`
* In PowerShell, you'll get:\
  `The term 'asdfgh' is not recognized as...`

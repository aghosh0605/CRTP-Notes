# Gnereral

### Required Tools

[PowerView](https://github.com/ZeroDayLab/PowerSploit/blob/master/Recon/PowerView.ps1)

```powershell
. C:\AD\Tools\PowerView.ps1
```

[AD module](https://github.com/samratashok/ADModule) - MS singed

```powershell
Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

### Domain

{% tabs %}
{% tab title="PowerView" %}
Get Current domain

```powershell
Get-Domain
```

Get Object of another domain

```powershell
Get-Domain -Domain moneycorp.local
```

Get domain SID for the current domain

```powershell
Get-DomainSID
```

Get domain policy for the current domain

```powershell
Get-DomainPolicyData
(Get-DomainPolicyData).systemaccess
```

Get domain policy for another domain

```powershell
(Get-DomainPolicyData -domain moneycorp.local).systemaccess
```
{% endtab %}

{% tab title="AD module" %}
Get Current domain

```powershell
Get-ADDomain
```

Get Object of another domain

```powershell
Get-ADDomain -Identity moneycorp.local
```

Get domain SID for the current domain

```powershell
(Get-ADDomain).DomainSID
```
{% endtab %}
{% endtabs %}

### Domain controller

{% tabs %}
{% tab title="PowerView" %}
Get domain controllers for the current domain

```powershell
Get-DomainController
```

Get domain controllers for another domain

```powershell
Get-DomainController -Domain moneycorp.local
```
{% endtab %}

{% tab title="AD module" %}
Get domain controllers for the current domain

```powershell
Get-ADDomainController
```

Get domain controllers for another domain

```powershell
Get-ADDomainController -DomainName moneycorp.local -Discover
```
{% endtab %}
{% endtabs %}

### Domain users

{% tabs %}
{% tab title="PowerView" %}
Get a list of users in the current domain

```powershell
Get-DomainUser
Get-DomainUser -Identity student1
```

Get list of all properties for users in the current domain

```powershell
Get-DomainUser -Identity student1 -Properties * 
Get-DomainUser -Properties samaccountname,logonCount
```

Search for a particular string in a user's attributes

```powershell
Get-DomainUser -LDAPFilter "Description=*built*" | Select name,Description
```

Get actively logged users on a computer (requires local admin privileges)

```powershell
Get-NetLoggedon -ComputerName dcorp-adminsrv

```

Get locally logged users on a computer (requires remote registry)

```powershell
Get-LoggedonLocal -ComputerName dcorp-adminsrv
```

Get the last logged user on a computer (requires admin privileges and remote registry)

```powershell
Get-LastLoggedOn -ComputerName dcorp-adminsrv
```
{% endtab %}

{% tab title="AD module" %}
Get a list of users in the current domain

```powershell
Get-ADUser -Filter * -Properties *
Get-ADUser -Identity student1 -Properties *
```

Get list of all properties for users in the current domain

```powershell
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name
Get-ADUser -Filter * -Properties * | select name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}}
```

Search for a particular string in a user's attributes

```powershell
Get-ADUser -Filter 'Description -like "*built*"' -Properties Description | select name,Desc
```
{% endtab %}
{% endtabs %}

### Domain Computers

{% tabs %}
{% tab title="PowerView" %}
Get a list of computers in the current domain

```powershell
Get-DomainComputer | select Name
Get-DomainComputer -OperatingSystem "*Server 2022*"
Get-DomainComputer -Ping
```
{% endtab %}

{% tab title="AD Module" %}
Get a list of computers in the current domain

```powershell
Get-ADComputer -Filter * | select Name
Get-ADComputer -Filter * -Properties *
Get-ADComputer -Filter 'OperatingSystem -like "*Server 2022*"' -Properties OperatingSystem | select Name,OperatingSystem
Get-ADComputer -Filter * -Properties DNSHostName | %{TestConnection -Count 1 -ComputerName $_.DNSHostName}
```
{% endtab %}
{% endtabs %}

### Domain Groups

{% tabs %}
{% tab title="PowerView" %}
Get all the groups

```powershell
Get-DomainGroup | select Name
Get-DomainGroup -Domain <targetdomain>
```

Get all groups containing the word "admin" in group name

```powershell
Get-DomainGroup *admin*
```

Get all the members of the Domain Admins group

```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

Get the group membership for a user

```powershell
Get-DomainGroup -UserName "student1"
```
{% endtab %}

{% tab title="AD Module" %}
Get all the groups

```powershell
Get-ADGroup -Filter * | select Name 
Get-ADGroup -Filter * -Properties *
```

Get all groups containing the word "admin" in group name

```powershell
Get-ADGroup -Filter 'Name -like "*admin*"' | select Name
```

Get all the members of the Domain Admins group

```powershell
Get-ADGroupMember -Identity "Domain Admins" -Recursive
```

Get the group membership for a user

```powershell
Get-ADPrincipalGroupMembership -Identity student1
```
{% endtab %}
{% endtabs %}

### Local Groups

{% tabs %}
{% tab title="PowerView" %}
List all the local groups on a machine (requires admin privileges)

```powershell
Get-NetLocalGroup -ComputerName dcorp-dc
```

Get members of the local group "Administrators" on a machine (requires admin privileges)

```powershell
Get-NetLocalGroupMember -ComputerName dcorp-dc -GroupName Administrators
```
{% endtab %}
{% endtabs %}

### Shares

{% tabs %}
{% tab title="PowerView" %}
Find shares on hosts in current domain.

```powershell
Invoke-ShareFinder -Verbose

```

Find sensitive files on computers in the domain

```powershell
Invoke-FileFinder -Verbose
```

Get all file servers of the domain

```powershell
Get-NetFileServer
```
{% endtab %}
{% endtabs %}

# User Hunting



{% tabs %}
{% tab title="PowerView" %}
Find Local group members of RDP or WinRM of specific machine

```powershell
Get-NetLocalGroupMember -ComputerName COMPUTER_NAME -GroupName "Remote Desktop Users"
Get-NetLocalGroupMember -ComputerName COMPUTER_NAME -GroupName "Remote Management Users"
```

Find all machines on the current domain where the current user has local admin access

```powershell
# Very noisy
Find-LocalAdminAccess -Verbose

# Very noisy
# When SMB and RPC are blocked

# Load
. C:\AD\Tools\Find-WMILocalAdminAccess.ps1
# execute
Find-WMILocalAdminAccess

#Load
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
# execute
Find-PSRemotingLocalAdminAccess.ps1


```

Find machines where a domain admin has sessions

```powershell
# Very noisy
Find-DomainUserLocation -Verbose
Find-DomainUserLocation -UserGroupIdentity "RDPUsers"
Find-DomainUserLocation -CheckAccess 
Find-DomainUserLocation -Stealth # less noisy, targeting file servers

```

List sessions on remote machines ([source](https://github.com/Leo4j/Invoke-SessionHunter))

```powershell
# Doesn’t need admin access on remote machines. 
# Uses Remote Registry and queries HKEY_USERS hive.
Invoke-SessionHunter -FailSafe

# Opsec friendly
Invoke-SessionHunter -NoPortScan -Targets C:\AD\Tools\servers.txt
```
{% endtab %}
{% endtabs %}

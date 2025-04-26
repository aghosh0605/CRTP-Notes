# BloodHound & SOAPHound

### BloodHound

BloodHound Versions:

* [BloodHound Legacy](https://github.com/BloodHoundAD/BloodHound)
* [BloodHound](https://github.com/SpecterOps/BloodHound)

```bash
# start db server
sudo neo4j console

# run bloodhound
bloodhound 
```

SharpHound collector:

* [SharpHound PowerShell](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.ps1)
* [SharpHound executable](https://github.com/BloodHoundAD/BloodHound/blob/master/Collectors/SharpHound.exe)

```powershell
# PowerShell
. SharpHound.ps1
Invoke-BloodHound -CollectionMethod All 
Invoke-BloodHound –Steatlh # Remove noisy collections like RDP,DCOM,PSRemote and Local Admin
Invoke-BloodHound -ExcludeDCs # Avoid MDI

# executable
SharpHound.exe
SharpHound.exe –-steatlh # Remove noisy collections like RDP,DCOM,PSRemote and Local Admin
```

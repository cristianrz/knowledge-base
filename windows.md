# Windows

## Import and export firewall rules

```cmd
netsh advfirewall export "C:\firewall-rules.wfw"
netsh advfirewall import "C:\firewall-rules.wfw"
```

## Registered antivirus

On regedit

```txt
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Security Center\Provider\Av
```

## Port forwarding

```cmd
netsh interface portproxy add v4tov4 listenport=8080 connectaddress=192.168.0.100 connectport=8080 protocol=tcp
```

Show forwards:

```cmd
netsh interface portproxy  show v4tov4
```

## Set connection profile to private

```powershell
Get-NetConnectionProfile
Set-NetConnectionProfile -InterfaceIndex 25 -NetworkCategory Private
```



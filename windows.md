# Windows

## Import and export firewall rules

```term
C:\> netsh advfirewall export "C:\firewall-rules.wfw"
C:\> netsh advfirewall import "C:\firewall-rules.wfw"
```

## Registered antivirus

On regedit

```txt
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Security Center\Provider\Av
```
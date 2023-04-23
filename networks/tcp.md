# TCP

## Three way handshake

- If the port is open

```
----- syn --->
<-- syn/ack --
----- ack --->
```

- If the port is closed

```
----- syn --->
<---- rst ----
```

- If the port is Filtered

```
----- syn --->
...nothing....	
```
## Local port forwarding

Bypass port block

From gate:

```shell
ssh -L [gate port]:[target ip]:[target port] [user]@[target ip]
```

## Dynamic port forwarding

Used as a web proxy (socks).

From gate:

```shell
ssh -D [gate port] [user]@[target ip]
```

## Remote port forwarding

From target:

 ```shell
 ssh -R [gate port]:localhost:[target port] [user]@[gate port]
 ```
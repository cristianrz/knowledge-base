# sudo

## sudoers syntax

```
User Host = (Runas) Command
```

## NOPASSWD

```
user ALL=(ALL) NOPASSWD: ALL
````

## Command execeptions

```
user ALL=(ALL) NOPASSWD: ALL, !/bin/bash, !/bin/sh
````


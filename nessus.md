# Nessus

## SSH key not working

<mark>First of all, check what plugin failed. There are two, a normal one and a V2 one. If the normal one failed but V2 succeeded everything is OK and you can ignore the below.</mark>

Otherwise:

- `ssh-rsa` needs to be enabled in `/etc/crypto-policies/back-ends/opensshserver.config`:

```
HostKeyAlgorithms [...],ssh-rsa
PubkeyAcceptedAlgorithms [...],ssh-rsa
```

**But disable after becuase it's unsafe**

- pubkey needs to be in `pem` format:

```bash
ssh-keygen -t rsa -b 1024 -m pem
```


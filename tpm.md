# TPM

## Use TPM + PIN to lock a key

Setup:

```bash
umask 077

# less than 32M is not recommended as the headers already take 16M
truncate -s 32M /var/secret.enc

# set a random password when asked, we will remove this later
cryptsetup luksFormat secret.enc

cryptsetup luksOpen secret.enc secret

dd if=/dev/zero of=/dev/mapper/secret bs=64K

cat /dev/random | head -c 40 | base64 > /dev/mapper/secret

cryptsetup luksClose secret

# copy this key and put in a safe place, to be used for emergencies
systemd-cryptenroll secrets.enc --recovery-key

# this will remove the random password we set before and leave
# only the recovery key
systemd-cryptenroll --wipe-slot=0 /var/secret.enc

# this will lock with PCR7 (secure boot) and a PIN
systemd-cryptenroll --tpm2-pcrs=7 --tpm2-with-pin=true \
	--tpm2-device=auto /var/secret.enc
```

Unlocking:

```bash
# this can then be passed to processes through stdin
/lib/systemd/systemd-cryptsetup attach secret /var/secret.enc - tpm2-device=auto &&
	cat /dev/mapper/secret &&
	cryptsetup luksClose secret
```


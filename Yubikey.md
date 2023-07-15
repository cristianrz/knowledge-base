# Yubikey

## List all FIDO

Initial protocol was U2F, then FIDO2. U2F is meant to be username + password + key. FIDO2 is passwordless. They are both currently in use. Main players:

- Google: U2F
- Microsoft: FIDO2
- Apple: U2F and FIDO2
- AWS: U2F and FIDO2
- Facebook: U2F and FIDO2

U2F aim is to be used as a second factor. FIDO2 aim was to not require passwords but some websites require passwords anyway. The website decides if they want the user to put the key PIN before authenticating.

- Non-resident keys: When signing up private keys are generated and encrypted with the yubikey internal key and then passed to the server. Then for auth the server sends you your key for you to decrypt. No keys are stored on the yubikey.
- Resident keys: stored on the yubikey, this allows single-factor passwordless auth.

U2F generally does not have a PIN (this is, yubikey PIN). FIDO2 depends on the website, they can decide if they want to require PIN or not.

Can be used for SSH keys as well.

```bash
ykman fido credentials list
```

Not sure what the list shows, mine is empty even though I used my key for many websites.

## List all OTP

This is just TOTP/HOTP

```bash
$ ykman oath accounts list
Amazon
Facebook
Google
```

## Keyboard-mode (OTP or password)

This functionality allows:

- Yubico OTP: OTP that an app can use calling yubi servers
- Challenge-response: app will send a challenge and yubikey will respond. Seems to be used more to decrypt stuff (e.g. KeePass)
- Static password: will type a password
- OATH-HOTP: like TOTP but using counters, likely to lose sync at some point. KeePass can do this too (but not KeePassXC)

There are 2 slots for this.

```
$ ykman otp info
Slot 1: empty
Slot 2: empty
```

## CCID

Acts like a smart card. Two modes:

- PIV: US government standard
- OpenPGP: acts as a GPGP Smartcard

This can be used to store SSH keys. OpenPGP is more complicated but allows signing stuff (like git commits).



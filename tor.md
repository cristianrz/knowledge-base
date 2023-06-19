# Tor

## SSH

1. Add to `torrc`:

```conf
SocksPort                     0
HiddenServiceDir              /var/lib/tor/site
HiddenServiceVersion          3
HiddenServicePort             80 127.0.0.1:80
# makes it faster at the expense of anonimity
HiddenServiceSingleHopMode    1
HiddenServiceNonAnonymousMode 1
```

2. Generate keys

```bash
sudo -u debian-tor /bin/bash -l

mkdir -p /var/lib/tor/site/authorized_clients/

openssl genpkey -algorithm x25519 -out /tmp/k1.prv.pem

cat /tmp/k1.prv.pem | grep -v " PRIVATE KEY" | base64pem -d | tail --bytes=32 | base32 | sed 's/=//g' > /tmp/k1.prv.key

openssl pkey -in /tmp/k1.prv.pem -pubout | grep -v " PUBLIC KEY" | base64pem -d | tail --bytes=32 | base32 | sed 's/=//g' > /tmp/k1.pub.key

cat << EOF > /var/lib/tor/site/authorized_clients/alice.auth
descriptor:x25519:<public key>
EOF
```

3. Restart `tor`

```bash
sudo systemctl reload tor
```

4. Get address from `/var/lib/tor/site/hostname` 
5. On client side, add to `torrc`

```
# torrc
ClientOnionAuthDir /var/lib/tor/onion_auth
```

```bash
mkdir -p  /var/lib/tor/onion_auth

cat << EOF > /var/lib/tor/onion_auth/bob_onion.auth_private
<56-char-onion-addr-without-.onion-part>:descriptor:x25519:<x25519 private key in base32>
EOF
```

6. Restart `tor`

```bash
sudo systemctl reload tor
```

7. Use `ssh` over `tor`

```bash
torsocks ssh host
```

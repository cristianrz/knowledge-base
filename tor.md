# Tor

## SSH

1. Add to `torrc`:

```ini
# torrc

HiddenServiceDir              /var/lib/tor/site
HiddenServiceVersion          3
HiddenServicePort             80 127.0.0.1:80

# makes it faster at the expense of anonimity
HiddenServiceSingleHopMode    1
HiddenServiceNonAnonymousMode 1
```

2. Generate keys

```bash
mkdir -p /var/lib/tor/site/authorized_clients/

openssl genpkey -algorithm x25519 -out /tmp/k1.prv.pem

cat /tmp/k1.prv.pem | grep -v " PRIVATE KEY" | base64pem -d | tail --bytes=32 | base32 | sed 's/=//g' > /tmp/k1.prv.key

openssl pkey -in /tmp/k1.prv.pem -pubout | grep -v " PUBLIC KEY" | base64pem -d | tail --bytes=32 | base32 | sed 's/=//g' > /tmp/k1.pub.key

touch /var/lib/tor/site/authorized_clients/alice.auth
```

3. `alice.auth` should contain

```
descriptor:x25519:<public key>
```

4. Restart `tor`

```bash
sudo systemctl reload tor
```

5. Get address from `/var/lib/tor/site/hostname` 

6. On client side, add to `torrc`

```
# torrc
ClientOnionAuthDir /var/lib/tor/onion_auth
```

```
cd /var/lib/tor/onion_auth
touch bob_onion.auth_private

echo '<56-char-onion-addr-without-.onion-part>:descriptor:x25519:<x25519 private key in base32>' > bob_onion.auth_private
```

7. Restart `tor`

```bash
sudo systemctl reload tor
```

8. Use `ssh` over `tor`

```bash
torsocks ssh host
```
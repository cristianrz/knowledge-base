# OpenSSL
## Make a certificate self signed

```bash
#!/bin/sh      

set -eux
  
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
	-keyout privkey.pem -out cert.pem \
	-addext "subjectAltName = IP:192.168.1.9" \
	-addext "extendedKeyUsage = serverAuth"

openssl pkcs12 -export -out key.pfx -inkey privkey.pem -in cert.pem -passout pass:

chown root:group key.pfx
chmod g+r key.pfx
```

## Generate CSR and key from template

```sh
openssl.exe req -newkey rsa:2048 -keyout private.key -out my.csr -config config.conf
```

## Generate CSR and key with prompts

```sh
openssl.exe req -newkey rsa:2048 -keyout private.key -out my.csr 
```

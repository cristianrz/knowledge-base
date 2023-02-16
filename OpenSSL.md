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

## Generate key

```sh
# target host
HOST=hostname
CA=rootCA

# generate private key
openssl genrsa -out "$HOST".key 2048

# generate CSR
openssl req -new -key "$HOST".key -out "$HOST".csr -sha256

# generate certificate
openssl x509 -req -in "$HOST".csr -CA "$CA".pem -CAkey "$CA"-key.pem -set_serial 100 -days 365 -outform PEM -out "$HOST".pem -sha256
```

## Generate CSR and key from template

```sh
openssl.exe req -newkey rsa:2048 -keyout private.key -out my.csr -config config.conf
```

## Generate CSR and key with prompts

```sh
openssl.exe req -newkey rsa:2048 -keyout private.key -out my.csr 
```

## Generate pfx

```sh
openssl pkcs12 -export -out client.pfx -inkey client.key -in client.pem -certfile root-ca.crt
```

## Full script

```sh
#!/bin/sh

set -eu

case "$#" in
	0)
		echo 'usage: makecert [fqdn]' >&2
		exit 1
		;;
esac

client="$1"
CA_KEY=rootCA-key.pem
CA=rootCA.pem
DAYS=365

printf 'password: '
read -r pass

# Create private key
if [ ! -f "${client}.key" ]; then
	openssl genrsa -out ${client}.key 2048
else
	echo Key already exists, skipping
fi

# create a config file
cat << EOF > "${client}.conf"
[req]
distinguished_name = req_distinguished_name
req_extensions = v3_req
prompt = no

[req_distinguished_name]
CN = ${client}

[v3_req]
keyUsage = digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names
[alt_names]
DNS.1 = ${client}
EOF

# Create a CSR
if [ ! -f "${client}.csr" ]; then
	yes '' | openssl req -new \
	    -config "${client}.conf" \
	    -key ${client}.key \
	    -out ${client}.csr
else
	echo CSR already exists, skipping
fi

# Sign it
openssl x509 -req \
    -extfile "${client}.conf" \
    -extensions v3_req \
    -days "$DAYS" \
    -in ${client}.csr \
    -CA ${CA} \
    -CAkey ${CA_KEY} \
    -out ${client}.pem \
    -CAcreateserial \
    -sha256

# Convert to pkcs12
openssl pkcs12 \
	-export \
	-in ${client}.pem \
	-inkey ${client}.key \
	-passout pass:"$pass" \
	-out ${client}.pfx

#cat "${client}.key" "${client}.pem" "$ca" > "${client}-fullchain.pem"

rm ${client}.csr
#rm ${client}.pem
```

## Check certificate contents

```bash
openssl x509 -in cert.pem -text
```

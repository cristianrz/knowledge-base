# OpenSSL

## Make a certificate self signed for an IP

```bash
#!/bin/sh      

set -eux
  
openssl req -x509 -nodes -days 182 -newkey rsa:2048 \
	-keyout privkey.pem -out cert.pem \
	-addext "subjectAltName = IP:192.168.0.1" \
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

## Make Firefox trust only your own CA's

- Firefox uses cert9.db inside your profile to decide what certs to trust.
- By default it only uses the certs embedded in libnss3.so
- You can mark them as not trusted manually from the certs window in the settings but this can take too long
- These certificates are also in /usr/share/ca-certificates/mozilla/
- We can add these certificates to a db

1. Copy the current \*.db files to a directory
2. Add all to the db as untrusted

```bash
#!/bin/bash

set -eu

# Set the path to the NSS database
NSSDB="<the directory>"

# Loop through each .crt file in /usr/share/ca-certificates/mozilla/
for crt_file in /usr/share/ca-certificates/mozilla/*.crt; do
        # Extract the filename without the extension
        filename="$(basename "$crt_file" .crt | sed 's/_/ /g')"

        # Add the certificate to the NSS database
        if certutil -A -n "$filename" -t "c,," -d "$NSSDB" -i "$crt_file"; then
                echo "Added $filename.crt to the NSS database."
        else
                echo "Failed to add $filename.crt to the NSS database."
        fi
done
```

3. Create a new Firefox profile, but don't launch it!
4. Copy the \*.db files inside the profile
5. Launch Firefox now
6. If you go to any website with HTTPS it will say the certificate is not trusted

Now you can add your own CA's:

1. Check the certificate for the site
2. Go on the tab associated with the CA
3. Download the PEM
4. Add it through the certificates window in settings

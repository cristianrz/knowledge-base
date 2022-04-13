# E-mail

## Send e-mails from command line
1. Install `msmtp` and `s-nail`
2. Configure `~/.msmtprc`

```txt
# Set default values for all following accounts.
defaults
auth           on
tls            on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile        ~/.msmtp.log

# Gmail
account        gmail
host           smtp.gmail.com
port           587
from           myuser@gmail.com
user           myuser
password       mypass

# Set a default account
account default : gmail
```

3. Send mail

```term
$ echo "Message content" | mail -s "Subject" destination@gmail.com
```

## Send attachments 

```term
$ mail -s "Subject" -a file.tar.gz destination@gmail.com < /dev/null
```

## Send HTML
```term
$ curl https://example.com | mail -M "text/html" -s "Subject" destination@gmail.com
```
# GPG

## Revoke ID

1. Edit your key with `gpg --edit-key <KEY_ID>`
1. Select the sub-key to revoke with `uid <ID>`
1. Revoke it with `revuid`
1. Save your changes with `save`
1. Publish your updated key with `gpg --send-keys <KEY_ID>`

## Retrieve ID

```shell
$ gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 843938DF228D22F7B3742BC0D94AA3F0EFE21092
```

## Import public key

```shell
gpg --import-key rms-pubkey.asc
```

## Add ID

```shell
gpg --edit-key <ID>
adduid
```

## Export public key

```shell
gpg --armor --export 4891B39B3AA3244E01DB2485F1C91E514AF8DE5F
```

## Export secret key

```shell
gpg --armor --export-secret-key 4891B39B3AA3244E01DB2485F1C91E514AF8DE5F
```

## Symmetric encrypt

```shell
gpg --output doc.gpg --symmetric doc
```

## Symmetric decrypt

```shell
gpg --output doc --decrypt doc.gpg
```

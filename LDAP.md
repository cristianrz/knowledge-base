# Ldapsearch

* Authenticate as user `-D uid=user,ou=People,dc=example,dc=com -W`
* Specify server: `-H ldap://ldap.example.com`
* Where to search: `-b ou=People,dc=example,dc=com`
* Search recursively: `-s sub`
* Use simple authentication: `-x`
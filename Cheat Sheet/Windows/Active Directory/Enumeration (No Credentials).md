### LDAP Anonymous Bind
```
Using LDAPSearch, we can extract information out of the AD

First, we need to get the naming contexts
ldapsearch -H ldap://$ip -x -s base namingcontexts 

If we want to print everything
ldapsearch -H ldap://$ip -x -b "[namingContext]" 

If we want to print all object class with people
ldapsearch -H ldap://$ip -x -b "[namingContext]" '(objectClass=Person)'

If we want to print all usernames to form a wordlist
ldapsearch -H ldap://$ip -x -b "[namingContext]" '(objectClass=Person)' sAMAccountName | grep sAMAccountName | awk '{print $2}'

Then output it into a file and remove all irrelevant accounts (ad accounts,guest accounts etc)
```
### RPC
```
Null authentication with rpc
rpcclient -U '' -N $ip

To view domain users:
enumdomusers 

From there, can create a wordlist with the usernames, can ignore guest and other accounts created by the AD, focus on user accounts
```
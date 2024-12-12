(https://www.netexec.wiki/)
```
netexec smb [ip] -u Guest -p '' --shares
netexec smb [ip] -u [username] -p [password] --shares
netexec winrm [ip] -u [username] -p [password]

For spraying:
netexec smb [ip] -u [file] -p [file] --continue-on-success

Important:
In AD, use --local-auth flag to see if you have access locally since it may not be domain wide access
```
## Password Spraying
First, come up with a list of usernames that is obtained from enumeration, then run the following command for spraying for default credentials
```
netexec smb [ip] -u users -p users --continue-on-success 
```
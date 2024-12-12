If there is anything you cant find, always good to look at HackTricks
https://book.hacktricks.xyz/
## Nmap
At the start, run a basic full port scan with nmap to see what ports are open
```
nmap -p- $ip
```
After getting the ports that are open, run a full service detection and default scripts scan
```
nmap -sC -sV -p [ports] -o [outputfile] $ip
```
Maybe you need a UDP scan
```
sudo nmap -sV -sU -top-ports=100 $ip
```
----
## FTP
If there is FTP, try anonymous login

----
## Exiftool
Let's say you got access into SMB share and you got a file but it contains nothing, can try exiftool and read the metadata to see if there is any information within
```
exiftool -a -u {filename}
```
## Web
For every port with http discovered, run Gobuster to enumerate the directories
```
For directory busting:
gobuster dir -u $ip:$port -w /usr/share/wordlists/dirb/common.txt -t 5

To specify the file extension, use -x
Repeat the gobuster for any subdirectories found, remember that 301 does not mean you cannot access the endpoint
```
---------
```
For subdomains/vhosts busting:
gobuster vhost -u http://url -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt

ffuf -u http://10.10.11.44 -H "Host: FUZZ.alert.htb" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -mc all -ac (change to fit your use case)
```
-------
```
For APIs:
APIs Path Naming Convention
/api_name/v1

In Gobuster: create a file called pattern which contains the following:
{GOBUSTER}/v1
{GOBUSTER}/v2
```
### Enumeration of APIs
```
gobuster dir -u http://$ip:port -w /usr/share/wordlists/dirb/big.txt -p pattern
```
-----
Other wordlists you can use:
Directory:
/usr/share/wordlists/dirb/common.txt 
/usr/share/wordlists/dirb/big.txt 
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

Subdomains/VHosts:
/usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt

----
### Manual Checking
robots.txt -  tells search engines which pages they are and aren't allowed to show on their search engine results or ban specific search engines from crawling the website altogether

Able to find certain sites that should be blocked, allowing us to pentest to access them

sitemap.xml - gives a list of every file the website owner wishes to be listed on a search engine

can find old webpages that are not in use but still working

HTTP headers - contains info such as web server software and programming language used

can find vulnerabilities related to the version
## SMB
https://0xdf.gitlab.io/cheatsheets/smb-enum#
- Enumerate Host
    - `netexec smb [ip]`
- List Shares
    - `netexec smb [host/ip] -u [user] -p [pass] --shares`
    - `netexec smb [host/ip] -u guest -p '' --shares`
    - `smbclient -N -L //[ip]`
- Enumerate Users within Host
	- `netexec smb [ip] -u guest -p '' --rid-brute`
- Enumerate files
	- `smbclient //[ip]/[share] -U [username] [password]`

## SNMP
### Enumerating the Management Information Base (MIB) Tree
```
snmpwalk -c public -v1 $ip 
snmpbulkwalk -Cr1000 -c public -v2c -t 10 $ip > snmp.txt (faster)


-c stands for community string
-v stands for SNMP version number
-t stands for timeout period

To enumerate SNMP MIB values:
snmpwalk -c public -v1 $ip [MIB Value]

Extended Queries
https://book.hacktricks.xyz/network-services-pentesting/pentesting-snmp#snmp-versions

snmpwalk -v X -c public <IP> NET-SNMP-EXTEND-MIB::nsExtendOutputFull
```
### SNMP MIB values

|                        |                  |
| ---------------------- | ---------------- |
| 1.3.6.1.2.1.25.1.6.0   | System Processes |
| 1.3.6.1.2.1.25.4.2.1.2 | Running Programs |
| 1.3.6.1.2.1.25.4.2.1.4 | Processes Path   |
| 1.3.6.1.2.1.25.2.3.1.4 | Storage Units    |
| 1.3.6.1.2.1.25.6.3.1.2 | Software Name    |
| 1.3.6.1.4.1.77.1.2.25  | User Accounts    |
| 1.3.6.1.2.1.6.13.1.3   | TCP Local Ports  |

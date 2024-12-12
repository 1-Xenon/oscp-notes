```
Always run both TCP and UDP scans
Quick Scan of Ports
- nmap -p- $ip
- nmap $ip
- nmap -sC -sV -o output.txt -p [portnumbers] $ip (to narrow down scan and make it faster)
Full Scan of Ports
- nmap -sC -sV -p- -o output.txt -A -T5 $ip
- sudo nmap -sV -sU -top-ports=100 $ip

Host Discovery
- nmap -sn x.x.x.1-254 -vv -oA hosts
```
## Options in nmap
-sC : Scan with default NSE scripts. Considered useful for discovery and safe

-sV : Attempts to determine the version of the service running on port

-o : Remote OS detection using TCP/IP stack fingerprinting

-A : Enables OS detection, version detection, script scanning, and traceroute

-T(0-5) : Timing and Performance, on a scale of 0-5 where 0 is paranoid and 5 is insane

-sn : Disable port scanning. Host discovery only.

-v : Increase the verbosity level (use -vv or more for greater effect)

-oA: Output in the three major formats at once

-oG : send the output in grepable format to its given filename

https://www.stationx.net/nmap-cheat-sheet/ for more options

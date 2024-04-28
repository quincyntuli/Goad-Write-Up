# Goad-Write-Up

This is a setup where GOAD is running on top of Ubuntu. An additional vm running kali was added to simulate a scenario where an internal assessment is conducted and the assessor already has access to the network.
### ifconfig 

```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.106  netmask 255.255.255.0  broadcast 192.168.56.255
        inet6 fe80::2a0a:6d46:1d16:18a1  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:1e:36:4a  txqueuelen 1000  (Ethernet)
        RX packets 35510  bytes 4867717 (4.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 13644  bytes 1536225 (1.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.80  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::1e73:4881:fb34:527c  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:40:fd:53  txqueuelen 1000  (Ethernet)
        RX packets 589440  bytes 43891514 (41.8 MiB)
        RX errors 0  dropped 648  overruns 0  frame 0
        TX packets 51565  bytes 3625982 (3.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 3220  bytes 206532 (201.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3220  bytes 206532 (201.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

The kali box with IP 192.168.56.106 is on the 192.168.56.0/24 network.

### nmap

```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ nmap -T4 192.168.56.0/24 | tee initial_report.txt
```

The results are parsed to look for the line containing the IP addresses.

```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ cat initial_report.txt | grep 'report'
Nmap scan report for 192.168.56.1
Nmap scan report for 192.168.56.10
Nmap scan report for 192.168.56.11
Nmap scan report for 192.168.56.12
Nmap scan report for 192.168.56.22
Nmap scan report for 192.168.56.23
Nmap scan report for 192.168.56.106
```

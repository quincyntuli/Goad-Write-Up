# Goad-Write-Up

This is a setup where GOAD is running on top of Ubuntu. An additional vm running kali was added to simulate a scenario where an internal assessment is conducted and the assessor already has access to the network.


## Subnet Enumeration

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


```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ nmap -T4 192.168.56.0/24 | tee initial_report.txt
```

The results are parsed to look for the line containing the IP addresses.

<div align="center" ><img width='95%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/01-nmap-01.png'><br><ins>IP scan reports</ins></div>


The hosts are enumerated as follows.

### Domain : sevenkingdoms.local
#### `KINGSLANDING` - host (192.168.56.10)

```bash
┌──(qdada㉿Embizweni)-[~/GOAD]
└─$ nmap -sC -sV 192.168.56.10 -oA nmap/nmap-services-192.168.56.10.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-09 12:36 EDT
Nmap scan report for 192.168.56.10
Host is up (0.00049s latency).
Not shown: 987 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-05-09 16:36:33Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-05-04T13:39:10
|_Not valid after:  2025-05-04T13:39:10
|_ssl-date: 2024-05-09T16:37:19+00:00; 0s from scanner time.
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-05-09T16:37:19+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-05-04T13:39:10
|_Not valid after:  2025-05-04T13:39:10
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-05-09T16:37:19+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-05-04T13:39:10
|_Not valid after:  2025-05-04T13:39:10
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-05-09T16:37:19+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-05-04T13:39:10
|_Not valid after:  2025-05-04T13:39:10
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Not valid before: 2024-05-03T13:20:15
|_Not valid after:  2024-11-02T13:20:15
| rdp-ntlm-info:
|   Target_Name: SEVENKINGDOMS
|   NetBIOS_Domain_Name: SEVENKINGDOMS
|   NetBIOS_Computer_Name: KINGSLANDING
|   DNS_Domain_Name: sevenkingdoms.local
|   DNS_Computer_Name: kingslanding.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-05-09T16:37:11+00:00
|_ssl-date: 2024-05-09T16:37:19+00:00; 0s from scanner time.
Service Info: Host: KINGSLANDING; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2024-05-09T16:37:11
|_  start_date: N/A
|_nbstat: NetBIOS name: KINGSLANDING, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:52:f1:1f (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 55.14 seconds
```

### Domain : north.sevenkingdoms.local
#### `WINTERFELL` - host (192.168.56.11)
{north.sevenkingdoms.local}

```bash
nmap -sC -sV 192.168.56.11 -oA nmap/nmap-services-192.168.56.11.txt
```

Results


```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ nmap -sC -sV 192.168.56.11 -oA nmap/nmap-services-192.168.56.11.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-28 02:34 EDT
Nmap scan report for 192.168.56.11
Host is up (0.00056s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-28 06:53:10Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-04-20T23:49:47
|_Not valid after:  2025-04-20T23:49:47
|_ssl-date: 2024-04-28T06:54:00+00:00; +18m08s from scanner time.
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-28T06:54:00+00:00; +18m08s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-04-20T23:49:47
|_Not valid after:  2025-04-20T23:49:47
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-28T06:54:00+00:00; +18m08s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-04-20T23:49:47
|_Not valid after:  2025-04-20T23:49:47
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-28T06:54:00+00:00; +18m08s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-04-20T23:49:47
|_Not valid after:  2025-04-20T23:49:47
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: WINTERFELL
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: winterfell.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-04-28T06:53:49+00:00
|_ssl-date: 2024-04-28T06:54:00+00:00; +18m08s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Not valid before: 2024-04-19T22:44:40
|_Not valid after:  2024-10-19T22:44:40
Service Info: Host: WINTERFELL; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2024-04-28T06:53:49
|_  start_date: N/A
|_nbstat: NetBIOS name: WINTERFELL, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:0a:c8:ae (Oracle VirtualBox virtual NIC)
|_clock-skew: mean: 18m07s, deviation: 0s, median: 18m07s
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.91 seconds
```



#### `CASTELBLACK` - host (192.168.56.22)
{north.sevenkingdoms.local}

```bash
nmap -sC -sV 192.168.56.22 -oA nmap/nmap-services-192.168.56.22.txt
```

Results

```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ nmap -sC -sV 192.168.56.22 -oA nmap/nmap-services-192.168.56.22.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-28 03:20 EDT
Nmap scan report for 192.168.56.22
Host is up (0.00076s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html).
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-04-23T12:11:09
|_Not valid after:  2054-04-23T12:11:09
|_ssl-date: 2024-04-28T07:40:11+00:00; +18m38s from scanner time.
| ms-sql-ntlm-info:
|   192.168.56.22:1433:
|     Target_Name: NORTH
|     NetBIOS_Domain_Name: NORTH
|     NetBIOS_Computer_Name: CASTELBLACK
|     DNS_Domain_Name: north.sevenkingdoms.local
|     DNS_Computer_Name: castelblack.north.sevenkingdoms.local
|     DNS_Tree_Name: sevenkingdoms.local
|_    Product_Version: 10.0.17763
| ms-sql-info:
|   192.168.56.22:1433:
|     Version:
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2024-04-28T07:40:11+00:00; +18m38s from scanner time.
| rdp-ntlm-info:
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: CASTELBLACK
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: castelblack.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-04-28T07:40:06+00:00
| ssl-cert: Subject: commonName=castelblack.north.sevenkingdoms.local
| Not valid before: 2024-04-19T22:59:25
|_Not valid after:  2024-10-19T22:59:25
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: CASTELBLACK, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:ca:d7:e0 (Oracle VirtualBox virtual NIC)
| smb2-time:
|   date: 2024-04-28T07:40:06
|_  start_date: N/A
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled but not required
|_clock-skew: mean: 18m38s, deviation: 0s, median: 18m37s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.90 seconds
```







### Domain : essos.local

#### `MEEREEN` - host (192.168.56.12)
{essos.local}

```bash
nmap -sC -sV 192.168.56.12 -oA nmap/nmap-services-192.168.56.12.txt
```

Results


```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ nmap -sC -sV 192.168.56.12 -oA nmap/nmap-services-192.168.56.12.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-28 03:08 EDT
Nmap scan report for 192.168.56.12
Host is up (0.00077s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-28 07:27:02Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-04-20T23:06:20
|_Not valid after:  2025-04-20T23:06:20
|_ssl-date: 2024-04-28T07:27:46+00:00; +18m32s from scanner time.
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds (workgroup: ESSOS)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-04-20T23:06:20
|_Not valid after:  2025-04-20T23:06:20
|_ssl-date: 2024-04-28T07:27:46+00:00; +18m33s from scanner time.
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-04-20T23:06:20
|_Not valid after:  2025-04-20T23:06:20
|_ssl-date: 2024-04-28T07:27:46+00:00; +18m32s from scanner time.
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-04-20T23:06:20
|_Not valid after:  2025-04-20T23:06:20
|_ssl-date: 2024-04-28T07:27:46+00:00; +18m33s from scanner time.
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: MEEREEN
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: meereen.essos.local
|   DNS_Tree_Name: essos.local
|   Product_Version: 10.0.14393
|_  System_Time: 2024-04-28T07:27:40+00:00
|_ssl-date: 2024-04-28T07:27:46+00:00; +18m33s from scanner time.
| ssl-cert: Subject: commonName=meereen.essos.local
| Not valid before: 2024-04-19T22:33:34
|_Not valid after:  2024-10-19T22:33:34
Service Info: Host: MEEREEN; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: meereen
|   NetBIOS computer name: MEEREEN\x00
|   Domain name: essos.local
|   Forest name: essos.local
|   FQDN: meereen.essos.local
|_  System time: 2024-04-28T00:27:40-07:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
|_nbstat: NetBIOS name: MEEREEN, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:76:83:4c (Oracle VirtualBox virtual NIC)
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required
|_clock-skew: mean: 1h05m13s, deviation: 2h20m00s, median: 18m32s
| smb2-time:
|   date: 2024-04-28T07:27:40
|_  start_date: 2024-04-22T22:43:24

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 73.07 seconds
```



#### `BRAAVOS` - host (192.168.56.23)
{essos.local}

```bash
nmap -sC -sV 192.168.56.23 -oA nmap/nmap-services-192.168.56.23.txt
```

Results

```bash
┌──(qdada㉿GOAD-kali)-[~/Desktop/ad]
└─$ nmap -sC -sV 192.168.56.23 -oA nmap/nmap-services-192.168.56.23.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-28 03:30 EDT
Nmap scan report for 192.168.56.23
Host is up (0.00074s latency).
Not shown: 995 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2024-04-28T07:49:53+00:00; +18m34s from scanner time.
| rdp-ntlm-info:
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: BRAAVOS
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: braavos.essos.local
|   DNS_Tree_Name: essos.local
|   Product_Version: 10.0.14393
|_  System_Time: 2024-04-28T07:49:47+00:00
| ssl-cert: Subject: commonName=braavos.essos.local
| Not valid before: 2024-04-19T22:59:21
|_Not valid after:  2024-10-19T22:59:21
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled but not required
|_clock-skew: mean: 1h42m33s, deviation: 3h07m50s, median: 18m32s
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: braavos
|   NetBIOS computer name: BRAAVOS\x00
|   Domain name: essos.local
|   Forest name: essos.local
|   FQDN: braavos.essos.local
|_  System time: 2024-04-28T00:49:47-07:00
|_nbstat: NetBIOS name: BRAAVOS, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:5c:95:28 (Oracle VirtualBox virtual NIC)
| smb2-time:
|   date: 2024-04-28T07:49:47
|_  start_date: 2024-04-22T22:56:10

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.86 seconds
```











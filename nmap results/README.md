## Nmap Results

### Hosts
#### 192.168.56.10

```bash
# Nmap 7.94SVN scan initiated Fri Aug 23 18:31:34 2024 as: nmap -sC -sV -oA nmap/nmap-services-192.168.56.10 192.168.56.10
Nmap scan report for 192.168.56.10
Host is up (0.00015s latency).
Not shown: 987 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-08-23 22:32:01Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-08-20T22:31:11
|_Not valid after:  2025-08-20T22:31:11
|_ssl-date: 2024-08-23T22:32:47+00:00; +2s from scanner time.
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-08-23T22:32:47+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-08-20T22:31:11
|_Not valid after:  2025-08-20T22:31:11
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-08-23T22:32:47+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-08-20T22:31:11
|_Not valid after:  2025-08-20T22:31:11
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-08-23T22:32:47+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2024-08-20T22:31:11
|_Not valid after:  2025-08-20T22:31:11
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: SEVENKINGDOMS
|   NetBIOS_Domain_Name: SEVENKINGDOMS
|   NetBIOS_Computer_Name: KINGSLANDING
|   DNS_Domain_Name: sevenkingdoms.local
|   DNS_Computer_Name: kingslanding.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-08-23T22:32:39+00:00
|_ssl-date: 2024-08-23T22:32:47+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Not valid before: 2024-08-19T22:14:33
|_Not valid after:  2025-02-18T22:14:33
Service Info: Host: KINGSLANDING; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: KINGSLANDING, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:47:64:84 (Oracle VirtualBox virtual NIC)
| smb2-time:
|   date: 2024-08-23T22:32:39
|_  start_date: N/A
|_clock-skew: mean: 1s, deviation: 0s, median: 1s
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Aug 23 18:32:45 2024 -- 1 IP address (1 host up) scanned in 71.33 seconds
```


DNS_Domain_Name: sevenkingdoms.local
DNS_Computer_Name: kingslanding.sevenkingdoms.local


#### 192.168.56.11

```bash
# Nmap 7.94SVN scan initiated Fri Aug 23 18:33:11 2024 as: nmap -sC -sV -oA nmap/nmap-services-192.168.56.11 192.168.56.11
Nmap scan report for 192.168.56.11
Host is up (0.00012s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-08-23 22:33:25Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-08-23T22:34:12+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-08-20T23:04:51
|_Not valid after:  2025-08-20T23:04:51
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2024-08-23T22:34:12+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-08-20T23:04:51
|_Not valid after:  2025-08-20T23:04:51
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-08-20T23:04:51
|_Not valid after:  2025-08-20T23:04:51
|_ssl-date: 2024-08-23T22:34:12+00:00; +2s from scanner time.
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2024-08-20T23:04:51
|_Not valid after:  2025-08-20T23:04:51
|_ssl-date: 2024-08-23T22:34:12+00:00; +2s from scanner time.
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Not valid before: 2024-08-19T22:23:28
|_Not valid after:  2025-02-18T22:23:28
|_ssl-date: 2024-08-23T22:34:12+00:00; +2s from scanner time.
| rdp-ntlm-info:
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: WINTERFELL
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: winterfell.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-08-23T22:34:04+00:00
Service Info: Host: WINTERFELL; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: WINTERFELL, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:a1:9b:90 (Oracle VirtualBox virtual NIC)
| smb2-time:
|   date: 2024-08-23T22:34:04
|_  start_date: N/A
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required
|_clock-skew: mean: 1s, deviation: 0s, median: 1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Aug 23 18:34:10 2024 -- 1 IP address (1 host up) scanned in 59.26 seconds
```

DNS_Domain_Name: north.sevenkingdoms.local
DNS_Computer_Name: winterfell.north.sevenkingdoms.local

#### 192.168.56.12

```bash
# Nmap 7.94SVN scan initiated Fri Aug 23 18:34:23 2024 as: nmap -sC -sV -oA nmap/nmap-services-192.168.56.12 192.168.56.12
Nmap scan report for 192.168.56.12
Host is up (0.00016s latency).
Not shown: 988 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-08-23 22:34:51Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-08-20T22:31:24
|_Not valid after:  2025-08-20T22:31:24
|_ssl-date: 2024-08-23T22:35:38+00:00; +2s from scanner time.
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds (workgroup: ESSOS)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-08-20T22:31:24
|_Not valid after:  2025-08-20T22:31:24
|_ssl-date: 2024-08-23T22:35:38+00:00; +2s from scanner time.
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-08-20T22:31:24
|_Not valid after:  2025-08-20T22:31:24
|_ssl-date: 2024-08-23T22:35:38+00:00; +2s from scanner time.
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
|_ssl-date: 2024-08-23T22:35:38+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:meereen.essos.local
| Not valid before: 2024-08-20T22:31:24
|_Not valid after:  2025-08-20T22:31:24
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: MEEREEN
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: meereen.essos.local
|   DNS_Tree_Name: essos.local
|   Product_Version: 10.0.14393
|_  System_Time: 2024-08-23T22:35:30+00:00
| ssl-cert: Subject: commonName=meereen.essos.local
| Not valid before: 2024-08-19T22:14:32
|_Not valid after:  2025-02-18T22:14:32
|_ssl-date: 2024-08-23T22:35:38+00:00; +2s from scanner time.
Service Info: Host: MEEREEN; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: MEEREEN, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:dd:db:3d (Oracle VirtualBox virtual NIC)
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2024-08-23T22:35:30
|_  start_date: 2024-08-23T22:27:09
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: meereen
|   NetBIOS computer name: MEEREEN\x00
|   Domain name: essos.local
|   Forest name: essos.local
|   FQDN: meereen.essos.local
|_  System time: 2024-08-23T15:35:30-07:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
|_clock-skew: mean: 46m41s, deviation: 2h20m00s, median: 1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Aug 23 18:35:36 2024 -- 1 IP address (1 host up) scanned in 73.35 seconds
```

DNS_Domain_Name: essos.local
DNS_Computer_Name: meereen.essos.local

#### 192.168.56.22

```bash
# Nmap 7.94SVN scan initiated Fri Aug 23 18:39:11 2024 as: nmap -sC -sV -oA nmap/nmap-services-192.168.56.22 192.168.56.22
Nmap scan report for 192.168.56.22
Host is up (0.00021s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-title: Site doesnt have a title (text/html).
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
|_ssl-date: 2024-08-23T22:39:46+00:00; +1s from scanner time.
| ms-sql-ntlm-info:
|   192.168.56.22:1433:
|     Target_Name: NORTH
|     NetBIOS_Domain_Name: NORTH
|     NetBIOS_Computer_Name: CASTELBLACK
|     DNS_Domain_Name: north.sevenkingdoms.local
|     DNS_Computer_Name: castelblack.north.sevenkingdoms.local
|     DNS_Tree_Name: sevenkingdoms.local
|_    Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-08-23T22:27:55
|_Not valid after:  2054-08-23T22:27:55
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
|_ssl-date: 2024-08-23T22:39:46+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=castelblack.north.sevenkingdoms.local
| Not valid before: 2024-08-19T22:30:34
|_Not valid after:  2025-02-18T22:30:34
| rdp-ntlm-info:
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: CASTELBLACK
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: castelblack.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-08-23T22:39:41+00:00
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2024-08-23T22:39:41
|_  start_date: N/A
|_clock-skew: mean: 1s, deviation: 0s, median: 0s
|_nbstat: NetBIOS name: CASTELBLACK, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:12:0c:a6 (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Aug 23 18:39:45 2024 -- 1 IP address (1 host up) scanned in 33.12 seconds
```

DNS_Domain_Name: north.sevenkingdoms.local
DNS_Computer_Name: castelblack.north.sevenkingdoms.local

#### 192.168.56.23

```bash
# Nmap 7.94SVN scan initiated Fri Aug 23 18:40:15 2024 as: nmap -sC -sV -oA nmap/nmap-services-192.168.56.23 192.168.56.23
Nmap scan report for 192.168.56.23
Host is up (0.00017s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-info:
|   192.168.56.23:1433:
|     Version:
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ms-sql-ntlm-info:
|   192.168.56.23:1433:
|     Target_Name: ESSOS
|     NetBIOS_Domain_Name: ESSOS
|     NetBIOS_Computer_Name: BRAAVOS
|     DNS_Domain_Name: essos.local
|     DNS_Computer_Name: braavos.essos.local
|     DNS_Tree_Name: essos.local
|_    Product_Version: 10.0.14393
|_ssl-date: 2024-08-23T22:40:34+00:00; +2s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-08-23T22:28:09
|_Not valid after:  2054-08-23T22:28:09
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=braavos.essos.local
| Not valid before: 2024-08-19T22:30:33
|_Not valid after:  2025-02-18T22:30:33
|_ssl-date: 2024-08-23T22:40:34+00:00; +2s from scanner time.
| rdp-ntlm-info:
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: BRAAVOS
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: braavos.essos.local
|   DNS_Tree_Name: essos.local
|   Product_Version: 10.0.14393
|_  System_Time: 2024-08-23T22:40:29+00:00
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled but not required
|_clock-skew: mean: 1h00m01s, deviation: 2h38m44s, median: 1s
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
|_  System time: 2024-08-23T15:40:29-07:00
| smb2-time:
|   date: 2024-08-23T22:40:29
|_  start_date: 2024-08-23T22:28:05
|_nbstat: NetBIOS name: BRAAVOS, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:96:3f:e7 (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Aug 23 18:40:32 2024 -- 1 IP address (1 host up) scanned in 17.67 seconds
```

DNS_Domain_Name: essos.local
DNS_Computer_Name: braavos.essos.local


### Domains

#### sevenkingdoms.local

- host : kingslanding.sevenkingdoms.local (192.168.56.10)
- notable service(s) :53,80,88,389,445,3389

#### north.sevenkingdoms.local

- host : winterfell.north.sevenkingdoms.local (192.168.56.11)
- notable service(s) :53,88,389,445,3389
  
- host : castelblack.north.sevenkingdoms.local (192.168.56.22)
- notable service(s) : 80,445,1433,3389

#### essos.local

- host : meereen.essos.local (192.168.56.12)
- notable service(s) :53,88,389,445,3389
  
- host : braavos.essos.local (192.168.56.23)
- notable service(s) : 80, 445, 1433,3389



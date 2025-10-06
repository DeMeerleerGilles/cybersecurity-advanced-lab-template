# Verslag labo 1

Nslookup naar het domein van UGent

```powershell
PS C:\Users\gille> nslookup www.ugent.be
Server:  ns1.hogent.be
Address:  193.190.173.1

Non-authoritative answer:
Name:    www.UGent.be
Address:  157.193.43.50
```

Met een opgegeven server

```powershell
PS C:\Users\gille> nslookup gent.be 8.8.8.8
Server:  dns.google
Address:  8.8.8.8

Non-authoritative answer:
Name:    gent.be
Addresses:  2001:67c:1902:1305::138
          212.123.24.4
```

## Recap Wireshark

In cybersecurity and virtualization we got to know Wireshark. Most captures were relatively small in size. In a more realistic setting, a capture file will have a lot of packets, often unrelated to what you are searching for. Open the capture-lab1.pcap file and try to answer the following questions:

1. What layers of the OSI model are captured in this capturefile?

    Laag 2-4 en een beetje laag 7.

2. Take a look at the conversations. What do you notice?
   Er is heel veel cummunicatie tussen 172.30.128.1O en 172.30.42.2, voor de rest is er een beetje communicatie tussen andere adressen.

3. Take a look at the protocol hierarchy. What are the most "interesting" protocols listed here?
   Heel veel SSH verkeer, een paar windows gerelateerde protocollen zoals LDAP, kerberos. De rest is allemaal ARP, TCP en UDP.

4. Can you spot an SSH session that got established between 2 machines? List the 2 machines. Who was the SSH server and who was the client? What ports were used? Are these ports TCP or UDP?
   Client: 172.30.128.10
   Server: 172.30.42.2
   Port: 22, 37700
   Protocol: TCP

5. Some cleartext data was transferred between two machines. Can you spot the data? Can you deduce what happened here?

    Couldn't find any cleartext data.

6. Someone used a specific way to transfer a png on the wire. Is it possible to export this png easily? 
   
   Is it possible to export other HTTP related stuff?
   It is possible to export the png file because it is transferred over HTTP and thus in the packet capture. Other HTTP related stuff can also be exported.

## Company router

```bash
[vagrant@companyrouter ~]$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 08:00:27:e8:a5:e1 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:26:49:5e brd ff:ff:ff:ff:ff:ff
    altname enp0s8
    inet 192.168.62.253/24 brd 192.168.62.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe26:495e/64 scope link
       valid_lft forever preferred_lft forever
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:92:4c:3c brd ff:ff:ff:ff:ff:ff
    altname enp0s9
    inet 172.30.255.254/16 brd 172.30.255.255 scope global noprefixroute eth2
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe92:4c3c/64 scope link
       valid_lft forever preferred_lft forever
[vagrant@companyrouter ~]$ ip route show
default via 192.168.62.254 dev eth1 proto static metric 100
172.30.0.0/16 dev eth2 proto kernel scope link src 172.30.255.254 metric 101
192.168.62.0/24 dev eth1 proto kernel scope link src 192.168.62.253 metric 100
```

Interfaces companyrouter:

eth0: down → geen gebruik.

eth1: 192.168.62.253/24 → default gateway gaat via 192.168.62.254. Dit is dus de WAN/ISP-kant.

eth2: 172.30.255.254/16 → intern netwerk. Hier hangen employee, dns, web op.

What did you have to configure on your red machine to have internet and to properly ping the web machine (is the ping working on IP only or also on hostname)?
De default gateway moest ingesteld worden op 192.168.62.254. Daarnaast had ik ook nog een probleem met de DNS. Dit viel op te loessen door:

```bash
 echo "nameserver 1.1.1.1" | sudo tee /etc/resolv.conf
```

Hierna kon ik alles bereiken.
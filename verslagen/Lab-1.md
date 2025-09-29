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

    What layers of the OSI model are captured in this capturefile?
    from layer 2 (Ethernet) up to layer 5 (Session layer, SSH)

    Take a look at the conversations. What do you notice?
    Er is heel veel cummunicatie tussen 172.30.128.1O en 172.30.42.2, voor de rest is er een beetje communicatie tussen andere adressen.

    Take a look at the protocol hierarchy. What are the most "interesting" protocols listed here?
    Heel veel SSH verkeer, een paar windows gerelateerde protocollen zoals LDAP, kerberos. De rest is allemaal ARP, TCP en UDP.

    Can you spot an SSH session that got established between 2 machines? List the 2 machines. Who was the SSH server and who was the client? What ports were used? Are these ports TCP or UDP?
    

    Some cleartext data was transferred between two machines. Can you spot the data? Can you deduce what happened here?

    Someone used a specific way to transfer a png on the wire. Is it possible to export this png easily? Is it possible to export other HTTP related stuff?


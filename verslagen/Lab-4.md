# Verslag labo 4: Honeypots

## What is a honeypot?

A honeypot is a deliberately vulnerable or attractive system, service or resource deployed to detect, study, and distract attackers. It appears as a real target to an adversary but is instrumented to record attacker actions, techniques and indicators of compromise without providing legitimate business value.



## LABO

Why is companyrouter, in this environment, an interesting device to configure with a SSH honeypot? What could be a good argument to NOT configure the router with a honeypot service?

Het is een centraal netwerkapparaat, dus aanvallers zullen dit vaak scannen of proberen binnen te geraken.

Dat maakt het een realistisch doelwit → de honeypot gaat effectief aanvallen aantrekken.

Het router IP is meestal default gateway, dus makkelijk vindbaar.

Argument tegen:

Een router moet stabiel en veilig blijven. Een honeypot draaien op dezelfde machine verhoogt de risico’s → extra software, extra attack surface.

Als de honeypot crasht of misbruikt wordt, kan dit impact hebben op de routerfunctie.

### SSH verschuiven van poort 22 -> 2222

Op de companyrouter openen we de SSH config. Dit doen we met het commando:

```bash
sudo nano /etc/ssh/sshd_config
```

Nu zoeken we de lijn waarop de poort gedeclareerd wordt. Deze staat in commentaar dus we halen de hash weg en maken er 2222 van. Hierna paste ik deze poort ook nog aan in de ssh config van mijn gebruiker op mijn host zodat het commando ssh companyrouter meteen op de juiste poort probeert te verbinden.

### Installeren van Crowie

Hierna installeerde ik Crowie op de VM, voor het gemak deed ik dit via docker.

Ik moest hiervoor uiteraard eerst docker installeren op de VM, dit deed ik met de volgende commandos:

```bash
sudo dnf install -y dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io
```

```bash
sudo systemctl enable --now docker
```

Gebruiker toevoegen aan de docker groep zodat er geen sudo meer nodig is:

```bash
sudo usermod -aG docker vagrant
```

Nu kunnen we beginnen aan de crowie installatie.

```bash
sudo mkdir /opt/cowrie
sudo mkdir /opt/cowrie/log
sudo mkdir /opt/cowrie/data
sudo mkdir /opt/cowrie/etc
```

```bash
sudo curl -o /opt/cowrie/etc/cowrie.cfg \
    https://raw.githubusercontent.com/cowrie/cowrie/master/cowrie.cfg.dist
```

```bash
sudo docker run -d \
  --name cowrie \
  -p 22:2222 \
  -v /opt/cowrie/log:/cowrie/var/log/cowrie \
  -v /opt/cowrie/data:/cowrie/var/lib/cowrie \
  -v /opt/cowrie/etc/cowrie.cfg:/cowrie/etc/cowrie.cfg \
  cowrie/cowrie:latest
```

Crowie draait nu op de ssh poort. We kunnen verbinding maken met de router.



What credentials work? Do you find credentials that don't work?

Je kunt met eender welk wachtwoord inloggen. Op een paar wachtwoorden na:
root - root werkt bijvoorbeeld niet. Dit is blijkbaar gedaan om het realistisch te laten zijn.

Do you get a shell?

We krijgen een shell, maar dit is geen echte shell, we zien ook een verschil aan de structuur tov de normale alma shell.

Are your commands logged? Is the IP address of the SSH client logged? If this is the case, where?

Ja, deze worden gelogd. Ze zijn terug te vinden in de 

Can an attacker perform malicious things?
Are the actions, in other words, the commands, logged to a file? Which file?

If you are an experienced hacker, how would/can you realize this is not a normal environment?

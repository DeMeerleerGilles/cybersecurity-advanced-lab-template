# Labo 5: backups

Om dit labo te voltooien volgde ik de stappen uit de opdracht:

```bash
[vagrant@web ~]$ mkdir important-files
[vagrant@web ~]$ cd !$
[vagrant@web important-files]$ curl --remote-name-all https://video.blender.org/download/videos/bf1f3fb5-b119-4f9f-9930-8e20e892b898-720.mp4 https://www.gutenberg.org/ebooks/100.txt.utf-8 https://www.gutenberg.org/ebooks/996.txt.utf-8 https://upload.wikimedia.org/wikipedia/commons/4/40/Toreador_song_cleaned.ogg
[vagrant@web important-files]$ mv 100.txt.utf-8 100.txt # Optional
[vagrant@web important-files]$ mv 996.txt.utf-8 996.txt # Optional
[vagrant@web important-files]$ ll
total 109992
-rw-r--r--. 1 vagrant vagrant       300 Nov  4 12:37 100.txt
-rw-r--r--. 1 vagrant vagrant       300 Nov  4 12:37 996.txt
-rw-r--r--. 1 vagrant vagrant   1702187 Nov  4 12:37 Toreador_song_cleaned.ogg
-rw-r--r--. 1 vagrant vagrant 110916740 Nov  4 12:37 bf1f3fb5-b119-4f9f-9930-8e20e892b898-720.mp4
```

Hierna maakte ik op de DB een map backups aan om de backups in op te slaan:

```bash
mkdir backups
```

We controlleerden of borgbackup in de dnf package manager stond en installeerden het indien nodig:

```bash
No matches found.
```

Dit gaf helaas geen matches. We moesten dus eerst de EPEL repository toevoegen:

```bash
sudo dnf install epel-release
sudo dnf update
sudo dnf search borgbackup
```

Nu moesten we de repository initialiseren:

```bash
borg init --encryption=repokey vagrant@172.30.20.15:~/backups
```

Passphrase: test

Om met keys te werken, moesten we eerst een SSH key genereren op de webserver:

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id vagrant@172.30.20.15
```

Nu exporteren we de borg-keyfile:

```bash
borg key export ~/borg-keyfile vagrant@172.30.20.15:~/backups
```


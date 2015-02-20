# Introduksjon til containere
Ciber Docker Tutorial

## Hva er Docker?
Docker lar deg kjøre prosesser eller applikasjoner isolert i en container. For å kjøre en container trenger man en enkel kommando:
```sh
$ docker run <navn på container>
```

## Hello world
La oss prøve et par enkle eksempel:
```sh
$ sudo docker run ubuntu /bin/echo 'Hei hå'
```
Gratulerer du har startet din første kontainer. Ved hjelp av *docker run* kommandoen har du nå startet en container basert på et image med navn ubuntu. (Ved å ikke spesifisere noe versjon av imaget så vil siste versjon bli benyttet.) I containeren kjøres kommandoen */bin/echo 'Hei hå'* og deretter blir kontaineren avsluttet. Dersom imaget ikke finnes lokalt, vil docker lete etter imaget i [docker-registeret](http://registry.hub.docker.com).

```sh
$ sudo docker run hello-world
```
Dette starter en container basert på imaget hello-world. Imaget inneholder instruksjoner om å kjøre en kommando som skriver ut teksten som kommer opp.
##Starte en interaktiv container
La oss starte en ny container
```sh
$ sudo docker run -t -i ubuntu /bin/bash
root@af8bae53bdd3:/#
```
Her startet vi en kontainer basert på samme image som tidligere, men vi spesifiserte to flagg: *-t* og *-i*. *-i* kobler en tty-terminal sltil containeren og *-t* lar oss interagere med containeren ved å ta kontroll over (STDIN) containeren.

Vi har nå en kommandolinje tilgjengelig som vi kan kjøre kommandoer på inn i containeren. La oss kjøre noen kommandoer:
```sh
root@336a680c5757:/# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.0  18152  1956 ?        Ss   13:33   0:00 /bin/bash
root        10  0.0  0.0  15568  1132 ?        R+   13:33   0:00 ps aux
```
Dette viser alle kjørende prosesser. Dersom tilsvarende kommando blir kjørt utenfor en container vil listen være mye lengre. Dette viser at prosessene som kjører i en container er isolert fra hosten som containeren kjører på.
 
## Search/pull
Image 'ubuntu' som du kjørte istad  ble hentet fra et Docker repository. Man kan søke i dette med *docker search 'navn'*. Så for å søke etter ubuntu kjør følgende:
```sh
$ docker search ubuntu
```
Som du ser i resultatet så finner du flere dockercontainere, hvor noen er markert som official. Ønsker du å laste ned image uten å kjøre det kan det gjøres med *docker pull 'navn'*.
```sh
$ docker pull ubuntu
```
## Liste imagene som ligger lokalt
For å liste ut imagene som ligger lokalt på maskinen benytter man kommandoen ``` sudo docker images```
```sh
➜ ~ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
jenkins             1.580.2             41001f44325b        6 weeks ago         661.9 MB
jenkins             latest              41001f44325b        6 weeks ago         661.9 MB
minkontainer        latest              d667b9855dd7        3 months ago        229.4 MB
myapache            latest              c48e219414a7        3 months ago        229.4 MB
myapache2           latest              c1d42ea45f1d        3 months ago        229.4 MB
hello-world         latest              ef872312fe1b        4 months ago        910 B
centos              centos5             504a65221a38        4 months ago        467.1 MB
centos              centos6             68edf809afe7        4 months ago        212.7 MB
centos              centos7             87e5b6b3ccc1        4 months ago        224 MB
centos              latest              87e5b6b3ccc1        4 months ago        224 MB
mongo               latest              66dd8b282889        4 months ago        391.2 MB
ubuntu              latest              826544226fdc        5 months ago        194.2 MB
nginx               latest              61e8f94e1d65        7 months ago        499.1 MB
training/webapp     latest              31fa814ba25a        8 months ago        278.8 MB
```
Som man ser så lister kommandoen ut imagene som vi har brukt tidligere i tutorialen (og eventuelt andre dersom man har lekt med docker tidligere)

Kommandoen lister ut 3 viktige ting om hvert image:
* Hvilket repository de kom fra, for eksempel ubuntu.
* Tagene til hvert image.
* ID til hvert image.

## Lage vår første container
Containere kan lages i hovedsak på to forskjellige måter. 
* Starte opp en interaktiv container basert på et image for så å påføre containeren endringer ved å kjøre ulike kommandoer.
* Lage en Dockerfile. Denne filen er på en måte en mal som docker bruker for å påføre endringer i en container for deg. Fordelen med denne metoden er at man enkelt kan gjenskape en container fra malen.

Vi skal lage en enkel container som kjører som en daemon, på samme måte som de aller fleste applikasjoner som kjøres ved hjelp av docker.

```sh
$ sudo docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
Dette lager en container som kjører et shellscript som looper og printer ut "hello world".
Stopp en halv. Hvorfor ser vi ikke "hello world"? Vi kjørte ```docker run``` med ```-d``` flagget. ```-d``` forteller docker at den skal lage en daemon av prosessen og kjøre den i bakgrunnen. Så hva er da den rare teksten docker skrev til skjermen? Jo det er ID'en til den kjørende docker containeren. Denne ID'en kan vi bruke til å se hva som har skjedd med vår container. 

```sh
➜  ~  docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
3b0ec9ade035        ubuntu:latest       "/bin/sh -c 'while t   21 seconds ago      Up 21 seconds                           hopeful_thompson    
```
I følge ```docker ps``` så kjører containeren vår. La oss inspisere den ved å se hva som har blitt skrevet til stdout. Dette gjøres ved kommandoen ```docker logs```. ```docker ps``` gir oss i dette tilfellet et generert navn for containeren. Navn er som regel enklere å jobbe med enn ID'er. Vi benytter derfor nå navnet for å inspisere containeren videre.

```sh
➜  ~  docker logs hopeful_thompson
hello world
hello world
hello world
hello world
hello world
hello world
hello world
hello world
``` 
Så resultatet ble som forventet. Ved hjelp av kommandoen ```docker logs``` kan vi altså inspisere stdout til en container. Dersom man ønsker å manuelt tilordne et navn til en container så kan man gjøre det ved hjelp av flagget ```-name```.

Hvis vi ikke gjør noe med den kjørende containeren vil den kjøre til vertsmaskinen stopper.
For å stoppe den kjørende containeren bruker vi ```docker stop```.

```sh
➜ ~ docker stop hopeful_thompson
hopeful_thompson
```
En kjapp sjekk på hvilke docker containere som kjører på vertsmaskinen viser at containeren er stoppet.

```sh
➜  ~  docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

# Jobbe med containere
Containerene vi har jobbet med til nå har ikke vært veldig nyttige. Så la oss se hvordan man jobber med docker i forbindelse med mer nyttige ting.
## Kjøre en webapplikasjon i docker
For vår "webapplikasjon" skal vi benytte en standard Tomcat container. La oss starte med ```docker run``` kommandoen.

```sh
➜  ~  docker run -d -P tomcat
Unable to find image 'tomcat' locally
Pulling repository tomcat
b5e4c1b6e097: Download complete 
511136ea3c5a: Download complete 
8771fbfe935c: Download complete 
0e30e84e9513: Download complete 
c90a56bfe7dd: Download complete 
4b976fb59d87: Download complete 
e43216966b96: Download complete 
1d1873aa2b8d: Download complete 
834592b9ae6e: Download complete 
518febcc1732: Download complete 
0ada0f19a068: Download complete 
dd68d8361188: Download complete 
63eb73f7c69c: Download complete 
fd2a9faea586: Download complete 
f3f50ba5de3b: Download complete 
05b935ac2d88: Download complete 
6c76a7473765: Download complete 
fd380d9fb669: Download complete 
a76d5b92b4cd: Download complete 
Status: Downloaded newer image for tomcat:latest
be83b545716b7b83b6ce18fff7e8b6d21bbb48f2c07563a954b53030ee1b18ff
```
La oss ta en titt på hva kommandoen gjorde. Vi har spesifisert to flag: ```-d``` og ```-P```. Vi har allerede sett hva ```-d``` gjør. ```-P``` er ny og forteller docker at interne exponerte porter i containeren skal eksponeres på vertsmaskinen. Uten ```-P``` vil vi ikke kunne nå tomcat på innsiden av containeren. La oss ta en titt på containeren vår med ```docker ps```.

```sh
➜  ~ docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
be83b545716b        tomcat:8            "catalina.sh run"   10 seconds ago      Up 9 seconds        0.0.0.0:49153->8080/tcp   jovial_mayer        
```
I kolonnen PORTS kan vi se at port 49153 på vertsmaskinen har blitt mappet til port 8080 i containeren. Som dere kan se, så har vi brukt flagget ```-l```. Det forteller docker at bare informasjon om sist startet container skal listes ut.

Så la oss peke webbrowseren vår til port 49153 å se om vi får kontakt med webapplikasjonen. 
![](https://github.com/Ciber-Norge/ciber-docker-tutorial/blob/master/bilder/tomcat.png)

##Start, Stop og Restart av containere.
Vi har til nå sett på hvordan vi kan starte en ny instans av en container basert på et image ved hjelp av kommandoen ```docker run```. Vi har også vært innom at man kan stoppe containeren ved hjelp av ```docker stop```. Vi stopper den kjørende tomcat containeren.

```sh
➜  ~  docker stop jovial_mayer
jovial_mayer
```
Containeren er nå stoppet. Det kan evt. verifiseres med ```docker ps```
For å starte den samme instansen av containeren på nytt kan man benytte kommandoen ```docker start```

```sh
➜  ~  docker start jovial_mayer
jovial_mayer
```
Kjør kommandoen ```docker ps -l``` for å verifisere at containeren er oppe og kjører igjen.

>**Merk:** kommandoen ```docker restart``` er også tilgjengelig for å restarte en container. 

##Sletting av containere
Du er ferdig med containeren din og har ikke lenger bruk for den. Da er det på tide å fjerne den fra vertsmaskinen. La oss slette den med ```docker rm```.

```sh
➜  ~  docker rm jovial_mayer
Error response from daemon: You cannot remove a running container. Stop the container before attempting removal or use -f
2015/02/20 12:25:04 Error: failed to remove one or more containers
```
Hva skjedde? Man kan ikke slette en kjørende container. Dette hindrer deg fra utilsiktet sletting av kjørende containere som man har behov for å beholde. Vi prøver på nytt etter å ha stoppet containeren.

```sh
➜  ~  docker stop jovial_mayer
jovial_mayer
➜  ~  docker rm jovial_mayer
jovial_mayer
```
Containeren er nå slettet og kan ikke startes igjen ved hjelp av ```docker start```. Hvis man har behov for en tomcat-container må man fyre opp en ny instans igjen med ```docker run```

>**Merk:** Sletting av en container kan ikke reverseres. 

## Lagre container state
Forsøk å starte ubuntu containeren og si at den skal finne veien til en av google sine dns servere.
```sh
$ sudo docker run ubuntu traceroute 8.8.8.8
```
Den observante seer vil da se at systemet ikke finner traceroute. La oss derfor oppdatere indexen over tilgjengelige pakker og hente/installere traceroute.
```sh
$ sudo docker run ubuntu /bin/sh -c 'sudo apt-get update && sudo apt-get install -y traceroute'
```
I output ser du nå at apt-get oppdaterer index for de forskjellige repositoriene og installerer traceourte.
Nå som du har installert traceroute er det en god ide å lagre endringene i containeren. I docker kalles dette commit, en operasjon som lagrer forskjellene fra det gamle til det nye imaget.
For å commite trenger du iden til containeren du vil lagre. Denne kan du finne ved å kjøre følgende kommando.
```sh
$ sudo docker ps -l
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                      PORTS               NAMES
a6a5ca03fa27        ubuntu:latest       "/bin/sh -c 'apt-get   3 weeks ago         Exited (0) 18 seconds ago                       insane_feynman
```
En unik del av iden må angis for å commite, du trenger som regel ikke angi hele. For å se alle argumenter commit støtter kan du kjøre commit uten argumenter.
```sh
$ docker commit

Usage: docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

Create a new image from a container's changes

  -a, --author=""     Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -m, --message=""    Commit message
  -p, --pause=true    Pause container during commit
```
Vi lagrer imaget med navnet ciber/traceroute
```sh
$ docker commit a6a ciber/traceroute
12111178563f5811fcac42320de24c9690b88e79967d26d337fce9eaa550202b
```
I outout vises IDen til den nye versjonen av imaget. Prøv nå å trace veien til google sin dns server med ditt nye image.
```sh
$ docker run ciber/traceroute traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  172.17.42.1 (172.17.42.1)  0.092 ms  0.020 ms  0.016 ms
 2  10.0.2.2 (10.0.2.2)  5.628 ms  5.539 ms  5.517 ms
 3  * * *
 4  62.109.44.210 (62.109.44.210)  1139.202 ms  1139.190 ms  1139.186 ms
 5  62.109.44.217 (62.109.44.217)  1139.179 ms 62.109.44.213 (62.109.44.213)  1139.170 ms 62.109.44.105 (62.109.44.105)  1139.158 ms
 6  62.109.44.157 (62.109.44.157)  1139.146 ms  38.602 ms  38.597 ms
 7  62.109.44.153 (62.109.44.153)  38.621 ms  13.376 ms  13.280 ms
 8  72.14.219.217 (72.14.219.217)  13.731 ms  14.031 ms  17.247 ms
 9  64.233.175.204 (64.233.175.204)  15.882 ms  22.242 ms 72.14.239.239 (72.14.239.239)  23.975 ms
10  8.8.8.8 (8.8.8.8)  28.199 ms  28.147 ms  29.137 ms
```
Du bør nå ha en forståelse av hvordan man kan endre state i et image og lagre endringen. 

# ciber-docker-tutorial
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
$ sudo docker -t -i ubuntu /bin/bash
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

 
## Søke
Image 'ubuntu' som du kjørte istad  ble hentet fra et Docker repostiory. Man kan søke i dette med *docker search 'navn'*. Så for å søke etter ubuntu kjør følgende:
```sh
docker search ubuntu
```
Som du ser i resultatet så finner du flere dockercontainere, hvor noen er markert som official.

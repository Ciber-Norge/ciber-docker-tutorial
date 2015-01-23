# ciber-docker-tutorial
Ciber Docker Tutorial

## Hva er Docker?
Docker lar deg kjøre prosesser eller applikasjoner isolert i en kontainer. For å kjøre en kontainer trenger man en enkel kommando:
```sh
docker run <navn på kontainer>
```

## Hello world
La oss prøve et par enkle eksempel:
```sh
sudo docker run ubuntu /bin/echo 'Hei hå'
```
Gratulerer du har startet din første kontainer. Ved hjelp av *docker run* kommandoen har du nå startet en kontainer basert på et image med navn ubuntu. (Ved å ikke spesifisere noe versjon av imaget så vil siste versjon bli benyttet.) I kontaineren kjøres kommandoen */bin/echo 'Hei hå'*
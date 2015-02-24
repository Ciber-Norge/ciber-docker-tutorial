#Jenkins-slave-container

## Bygg
```sh
$ docker build -t jenkins_agent <path til docker file>
```

## Kj�r
```sh
$ docker run -d -P -p 31337:22 jenkins_agent
```

## Legge til slave
For � legge til en agent i jenkins, �pne webgrensesnittet i nettleseren din. Og gj�r f�lgende.

1. Velg Manage Jenkins
2. Velg Manage Nodes
3. Velg New Node
4. Velg Dumb Slave og gi den et navn, f.eks UberSlave
5. Klikk p� OK.
6. Legg inn /home/jenkins/ i Remote root directory. Det er dette som ble valgt som home directory for brukeren jenkins i image.
7. Legg inn ipen hvor docker kj�rer i Host. Om du kj�rer linked containers s� er dette navnet du brukte ved linking.
8. Klikk Add siden av Credentials
9. Legg inn username/password jenkins/jenkins i dette tilfellet.
10. Klikk add.
11. Velg Advanced...
12. Endre port til porten som eksponerer ssh mot slave-image i docker.
13. Klikk save.

![bilde av jenkins slave](../bilder/jenkins_slave.PNG)
# Legge til slave
For å legge til en agent i jenkins, åpne webgrensesnittet i nettleseren din. Og gjør følgende.
1. Velg Manage Jenkins
2. Velg Manage Nodes
3. Velg New Node
4. Velg Dumb Slave og gi den et navn, f.eks UberSlave
5. Klikk på OK.
6. Legg inn /home/jenkins/ i Remote root directory. Det er dette som ble valgt som home directory for brukeren jenkins i image.
7. Legg inn ipen hvor docker kjører i Host.
8. Klikk Add siden av Credentials
9. Legg inn username/password jenkins/jenkins i dette tilfellet.
10. Klikk add.
11. Velg Advanced...
12. Endre port til porten som eksponerer ssh mot slave-image i docker.
13. Klikk save.

![bilde av jenkins slave](../bilder/jenkins_slave.PNG)
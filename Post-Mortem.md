# Post Mortem Analyse 

## Summary
Tirsdag d. 14/11-2017 kl 19:37 blev der bygget en ny version af backenden i Jenkins, der resulterede i et crash af serveren.

## Detailed description
En ændring af serverens port i filen server.js resulterede i et crash på serveren, da den specificerede port ikke længere stemte overens med den port, som var angivet i systemets docker-deploy script. 

## Root cause
Systemet var sat til at lytte på port 8083, men docker-deploy scriptet var sat til at mappe port 8083 til 8080 inde i containeren. 

## The fix
Vi besluttede i første omgang at reverte applikationen tilbage til en tidligere version, som tidligere havde virket. Derefter kunne vi identificere og rette fejlen, uden at miste mere dyrebar uptime.

## Lessons learned
Vi er allesammen blevet klogere på Docker og port-mapping konceptet, og vigtigheden i at have 100% styr på opsætning og konfigurering af en server. Fremover er man nødt til at tjekke at ændrede porte stemmer overens, hvis der ændres i disse. 

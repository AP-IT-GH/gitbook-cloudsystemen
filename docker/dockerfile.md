---
description: >-
  Een Dockerfile is een scriptbestand dat de stappen bevat die nodig zijn om een
  Docker image te bouwen.
---

# dockerfile

## Wat is een Dockerfile?

Een Dockerfile is een scriptbestand dat de stappen bevat die nodig zijn om een Docker image te bouwen. Het is een tekstbestand met specifieke opdrachten die door Docker worden uitgevoerd om de Docker image op te bouwen en te configureren.

Een Dockerfile bevat informatie over de basisimage waarop de Docker image gebouwd wordt, de software en bibliotheken die moeten worden geïnstalleerd, de configuratie-instellingen die moeten worden toegepast en de commands die moeten worden uitgevoerd bij het opstarten van een Docker container.

De Dockerfile is essentieel voor het automatiseren van het bouwproces van Docker images en garandeert dat de Docker image altijd op dezelfde manier wordt gebouwd, ongeacht de machine waarop het draait of de ontwikkelomgeving.

### Voorbeeld van een Dockerfile

Stel dat we een eenvoudige Node.js-webapplicatie willen bouwen die "**Hello World**" in de console logt.&#x20;

<figure><img src="../.gitbook/images/docker/image (9).png" alt=""><figcaption><p>Simpele node applicatie</p></figcaption></figure>

#### Hier is een voorbeeld van hoe we een Dockerfile kunnen maken voor dit project:

```docker
# Stap 1: Basis-image kiezen
FROM node:alpine

# Stap 2: Werkdirectory aanmaken en naar deze werkdirectory gaan
WORKDIR /app

# Stap 3: Bestanden kopiëren
# '.' representeert de root directory
COPY . .

# Stap 4: Nodige packages installeren
RUN npm install

# Stap 5: Luisteren op netwerkpoort 80
EXPOSE 80

# Stap 6: Uitvoeren van de applicatie
CMD ["npm", "start"]
```

#### Hier is wat de verschillende regels in het Dockerfile doen:

1. `FROM node:alpine`: Deze regel specificeert welke basis-image gebruikt moet worden voor de container. In dit geval wordt het '[node:alpine](https://hub.docker.com/\_/node)'-image gebruikt, wat een kleine en lichtgewichtte distributie van Node.js is gebaseerd op de Alpine Linux-distributie.
2. `WORKDIR /app`: Deze regel stelt de standaard werkdirectory in voor de container op `/app`. Dit betekent dat elke opdracht die uitgevoerd wordt binnen de container uitgevoerd zal worden vanuit de `/app` directory.
3. `COPY . .`: Deze regel kopieert alle bestanden en mappen uit de huidige directory naar de root directory in de container. Dit betekent dat alle bestanden uit de huidige directory (waarin de Dockerfile zich bevindt) zullen worden gekopieerd naar de root directory in de container.
4. `RUN npm install`:  Deze regel installeert de nodige npm-packages
5. `EXPOSE 80`: Deze regel zorgt dat de container deze netwerkpoort in de gaten houdt. Anders zal hij niet reageren als bijvoorbeeld een browser contact neemt via deze poort.
6. `CMD ["npm", "start"]`: Uiteindelijk specificeren we dat het commando `npm start` uitgevoerd moet worden wanneer de container opgestart wordt.

Nadat je de Dockerfile gemaakt hebben, kan je een Docker-image bouwen voor je Node.js-applicatie door het volgende commando uit te voeren (navigeer in je terminal naar de map waarin je Dockerfile leeft):

```bash
docker build -t my-node-app .
```

Dit zal een Docker-image bouwen met de naam 'my-node-app' op basis van de informatie in het Dockerfile. Nadat de image is gebouwd, kunnen we deze opstarten door het volgende commando uit te voeren:

```bash
docker run my-node-app
```

Dit zal een nieuwe container opstarten en de code van onze Node.js-applicatie uitvoeren.

<figure><img src="../.gitbook/images/docker/image (2).png" alt=""><figcaption></figcaption></figure>

Samengevat, bouwt de Dockerfile een Docker-container die de Node.js-applicatie bevat en deze uitvoert wanneer de container opgestart wordt.

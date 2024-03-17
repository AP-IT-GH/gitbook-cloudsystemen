---
description: De voornaamste docker compose commands.
---

# docker compose commands

Nu we weten hoe we een docker-compose-bestand moeten maken, gaan we de meest voorkomende Docker Compose-opdrachten bekijken die we met onze bestanden kunnen gebruiken. Houd er rekening mee dat we alleen de meest gebruikte commando's zullen bespreken.

### docker-compose

`docker-compose`: Elke Compose-opdracht begint met deze opdracht. Je kan ook `docker-compose --help` gebruiken om aanvullende informatie over argumenten en implementatiedetails te geven.

<figure><img src="../../.gitbook/images/docker/image (5).png" alt=""><figcaption></figcaption></figure>

### docker-compose build

`docker-compose build`: Deze opdracht bouwt images in het bestand `docker-compose.yml`. De taak van de opdracht build is om de images klaar te zetten om daarvan containers te maken, dus als een service de vooraf gebouwde image gebruikt, wordt deze service overgeslagen.

### docker-compose images

`docker-compose images`: met deze opdracht worden de images weergegeven die je hebt gebouwd met behulp van het `docker-compose.yml` bestand.

<figure><img src="../../.gitbook/images/docker/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### docker-compose stop

docker-compose stop: Deze opdracht stopt de lopende containers van de services.

<figure><img src="../../.gitbook/images/docker/image (29).png" alt=""><figcaption></figcaption></figure>

### docker-compose run

`docker-compose run`: Dit is vergelijkbaar met de opdracht `docker run`. Het maakt containers van images die zijn gebouwd voor de services die worden genoemd in de `docker-compose.yml` file.

### docker-compose up

`docker-compose up`: Deze opdracht doet hetzelfde als `docker-compose build` en `docker-compose run`. Het bouwt de images op als ze zich niet lokaal bevinden en start de containers. Als er al images zijn gemaakt, wordt de container direct opgestart.

<figure><img src="../../.gitbook/images/docker/image (20).png" alt=""><figcaption></figcaption></figure>

### docker-compose ps

`docker-compose ps`: De opdracht "docker-compose ps" is een commando in Docker Compose dat de status van de containers in je Docker Compose-applicatie weergeeft. Dit commando toont informatie over de containers, waaronder hun naam, status en de poorten waarmee ze op de hostmachine zijn gemapped. Het geeft ook informatie over de Docker Compose-services, zoals het uitgevoerde commando en de locatie van het bijbehorende Dockerfile. Dit is een nuttige opdracht om de status van je containers te controleren en eventuele problemen tijdens ontwikkeling en implementatie op te lossen.

<figure><img src="../../.gitbook/images/docker/image (4) (2).png" alt=""><figcaption></figcaption></figure>

### docker-compose down

`docker-compose down`: deze opdracht is vergelijkbaar met de opdracht voor het opschonen van het docker-systeem. In Compose stopt het echter alle services en ruimt het de containers, netwerken en images op.

<figure><img src="../../.gitbook/images/docker/image (14).png" alt=""><figcaption></figcaption></figure>

### Bronnen

* [https://www.educative.io/blog/docker-compose-tutorial](https://www.educative.io/blog/docker-compose-tutorial)

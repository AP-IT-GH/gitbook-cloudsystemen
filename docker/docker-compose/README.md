---
description: >-
  Docker Compose is een tool voor het definiëren en uitvoeren van
  multi-container Docker-applicaties.
---

# Docker Compose

Normaal gezien zou Docker Compose reeds op je systeem moeten staan omdat je Docker geïnstalleerd hebt.

## Wat is Docker Compose?

Docker Compose is een tool voor het definiëren en uitvoeren van multi-container Docker-applicaties. Het maakt het mogelijk om een complete **stack** van services te definiëren in een enkel bestand, genaamd `docker-compose.yml`.

Een applicatie kan bestaan uit meerdere containers waarop verschillende services worden uitgevoerd. Het kan vervelend zijn om containers handmatig te starten en te beheren, dus heeft Docker een handige tool gemaakt die het proces versnelt: Docker Compose.

<figure><img src="../../images/docker/image (4) (2) (1).png" alt=""><figcaption></figcaption></figure>

Met Docker Compose kan je bijvoorbeeld een webapplicatie definiëren die bestaat uit een webcontainer en een databasecontainer. Met een enkele opdracht kun je deze containers tegelijk starten, communiceren en beheren. Hierdoor is het eenvoudiger om complexe applicaties op te zetten en uit te voeren, zonder de noodzaak om veel commandoregels of scripts te schrijven.

Docker Compose is een uitstekende oplossing voor ontwikkelaars die een snelle manier willen hebben om hun applicaties te testen en uit te voeren in een productie-achtige omgeving, zonder dat ze zich zorgen hoeven te maken over de configuratie van de infrastructuur.

{% hint style="info" %}
Het YAML-bestand, `docker-compose.yml` configureert de services van de toepassing en bevat regels die specificeren hoe je ze wilt uitvoeren. Met het YAML-bestand kan je alle services starten, stoppen of opnieuw opbouwen met een enkele opdracht. Bovendien kan je de status van een service controleren, logs weergeven en eenmalige opdrachten uitvoeren.
{% endhint %}

### Bronnen

* [https://www.educative.io/blog/docker-compose-tutorial](https://www.educative.io/blog/docker-compose-tutorial)

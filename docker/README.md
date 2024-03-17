---
description: >-
  Docker is een open-source platform voor het automatiseren van de deployment,
  het beheer en de implementatie van applicaties door middel van containers.
---

# docker

## Wat is Docker?

Docker maakt verpakte applicaties die **containers** worden genoemd. Elke container biedt een geïsoleerde omgeving die lijkt op een [virtuele machine](https://www.vmware.com/topics/glossary/content/virtual-machine.html) (VM). In tegenstelling tot VM's hebben Docker-containers geen volledig besturingssysteem. Ze delen de kernel van je computer en virtualiseren op softwareniveau.

{% hint style="info" %}
De kernel van een computer is het hart van het besturingssysteem. Het is de laag tussen de hardware en de software-applicaties. Het is verantwoordelijk voor het beheer van de resources van de computer, zoals geheugen, processors, apparaten en andere hardware.
{% endhint %}

{% hint style="warning" %}
Er is geen simulatie van hardware in Docker. In plaats daarvan maakt het gebruik van de resources van het besturingssysteem van je computer om containers te hosten.
{% endhint %}

### Wat is een container?

Een Docker container is een afgezonderde omgeving waarin een applicatie en zijn afhankelijkheden worden uitgevoerd. Het is een compleet pakket met alle benodigde componenten en bibliotheken, waardoor het onafhankelijk is van de onderliggende infrastructuur.

Een Docker container is gebouwd op basis van een **Docker image**, dat een snapshot van de configuratie bevat en de benodigde bestanden. Wanneer een container is gestart, heeft het zijn eigen unieke runtime-omgeving, waarin de applicatie uitgevoerd wordt.

Deze isolatie garandeert dat de container en de applicatie erin op een consistente manier werken, ongeacht de onderliggende infrastructuur. Bovendien kunnen containers eenvoudig worden gedeployed, beheerd en verplaatst tussen verschillende hosts/computers.

{% hint style="info" %}
Een waardevol kenmerk is de standaardisatie van de computeromgeving die in de container draait. Het zorgt er niet alleen voor dat je applicatie onder identieke omstandigheden werkt, maar het vereenvoudigt ook het delen met andere teamgenoten.
{% endhint %}

Omdat containers autonoom zijn, bieden ze een sterke isolatie, zodat ze andere actieve containers en de server die ze ondersteunt niet onderbreken. Docker beweert dat deze eenheden "de sterkste isolatiemogelijkheden in de branche bieden". Je moet je dus geen zorgen maken over de beveiligen van je computer.

In tegenstelling tot virtuele machines (VM's) waarbij virtualisatie op hardwareniveau plaatsvindt, virtualiseren containers op de app-laag. Ze kunnen één machine gebruiken, de kernel ervan delen en het besturingssysteem virtualiseren om geïsoleerde processen uit te voeren. Dit maakt containers extreem licht van gewicht.

<figure><img src="../images/docker/image (27).png" alt=""><figcaption><p>bron: <a href="https://phoenixnap.com/kb/docker-image-vs-container">https://phoenixnap.com/kb/docker-image-vs-container</a></p></figcaption></figure>

### Wat is een image?

Een Docker-image is een immutable (onveranderbaar) bestand dat de broncode, bibliotheken, afhankelijkheden, tools en andere bestanden bevat die nodig zijn om een applicatie te laten werken.

Vanwege hun `read-only` kwaliteit worden deze images soms snapshots genoemd. Ze vertegenwoordigen een applicatie en zijn virtuele omgeving op een bepaald tijdstip. Deze consistentie is een van de geweldige functies van Docker. Het stelt ontwikkelaars in staat om software te testen en te experimenteren in stabiele, uniforme omstandigheden.

Omdat images in zekere zin slechts sjablonen zijn, kan je ze niet starten of uitvoeren. Wat je wel kunt doen, is het sjabloon gebruiken als basis om een container te bouwen. Een container is uiteindelijk slechts een lopende image. Nadat je een container hebt gemaakt, voegt deze een beschrijfbare laag toe bovenop de onveranderlijke image, wat betekent dat je deze nu kunt wijzigen.

De image-base waarop je een container maakt, bestaat afzonderlijk en kan niet worden gewijzigd. Wanneer je een gecontaineriseerde omgeving uitvoert, maak je in wezen een read-write kopie van dat bestandssysteem (docker-image) in de container. Dit voegt een containerlaag toe die wijzigingen van de volledige kopie van de image mogelijk maakt.

### Voordelen van Docker

1. **Portabiliteit**: Docker stelt ontwikkelaars in staat om hun applicaties te bouwen en uit te voeren op elke machine die Docker ondersteunt, zonder zorgen te maken over compatibiliteitsproblemen tussen omgevingen.
2. **Isolatie**: Docker-containers kunnen op een afgeschermde manier worden uitgevoerd, waardoor ze niet kunnen interfereren met andere processen en diensten op de hostmachine. Dit verhoogt de stabiliteit en betrouwbaarheid van de applicaties.
3. **Gemakkelijk deployment**: Docker stelt ontwikkelaars in staat om containers met hun toepassingen en bijbehorende afhankelijkheden eenvoudig te verplaatsen tussen verschillende omgevingen. Dit betekent dat ontwikkelaars minder tijd hoeven te besteden aan het oplossen van compatibiliteitsproblemen en problemen met de configuratie.
4. **Snelheid**: Docker-containers starten en stoppen snel en zijn licht van gewicht, waardoor ze sneller kunnen worden gedeployed en uitgevoerd dan traditionele virtuele machines.

### Bronnen

* [https://www.howtogeek.com/733522/docker-for-beginners-everything-you-need-to-know/](https://www.howtogeek.com/733522/docker-for-beginners-everything-you-need-to-know/)
* [https://phoenixnap.com/kb/docker-image-vs-container](https://phoenixnap.com/kb/docker-image-vs-container)

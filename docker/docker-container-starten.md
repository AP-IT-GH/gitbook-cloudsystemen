# Docker container starten

## Starten van een docker container

Om een Docker-container uit te voeren, heb je een Docker-image nodig. Dit is het sjabloon dat de broncode, bibliotheken, afhankelijkheden, tools en andere bestanden bevat die nodig zijn om een container te laten werken. Je kan een Docker-image maken met behulp van een [Dockerfile](https://docs.docker.com/engine/reference/builder/) en de [docker build commando](https://docs.docker.com/get-started/02\_our\_app/), of je kan een image uit een register halen, zoals [Docker Hub](https://hub.docker.com/).

In deze zelfstudie zal je gebruik maken van de image "[denising/hallo-student](https://hub.docker.com/repository/docker/denising/hallo-student/)" dat terug te vinden is op Docker Hub.

### Stap 1: download een image van Docker Hub

Het commando `docker images` geeft een lijst weer met de lokaal geïnstalleerde docker images.

<figure><img src="../.gitbook/images/docker/image (5) (4).png" alt=""><figcaption></figcaption></figure>

In dit voorbeeld zal je de image denising/hallo-student binnenhalen (van Docker Hub) met behulp van het commando `docker pull`. Kopieer de code hieronder en plak het in je terminal:

```sh
docker pull denising/hallo-student:latest
```

Als je nu opnieuw `docker images` uitvoert zal je zien dat denising/hallo-student er tussen staat.

<figure><img src="../.gitbook/images/docker/image (8).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Je kan je lokale images ook raadplegen in Docker Desktop -> Images.
{% endhint %}

### Stap 2: Aanmaken en uitvoeren van een container

Je kan een container aanmaken en uitvoeren met behulp van het commando `docker run`. Maak een container aan en voer deze uit door de onderstaande code te kopiëren naar je terminal:

```sh
docker run denising/hallo-student
```

* `docker run` is het commando om een nieuwe container te starten
* `denising/hallo-student` is de naam van de container-image die je wilt opstarten.

<figure><img src="../.gitbook/images/docker/image (15).png" alt=""><figcaption></figcaption></figure>

### Stap 3: verwijderen van een container

Het commando `docker ps -a` geeft een lijst weer van al je containers.

<figure><img src="../.gitbook/images/docker/image (14) (2).png" alt=""><figcaption></figcaption></figure>

Indien je een container wilt verwijderen kan je het commando `docker rm CONTAINER_NAME` of `docker rm CONTAINER_ID` gebruiken.

Verwijder de container dat de image van denising/hallo-student gebruikt. Je kan de naam van je container terug vinden in de laatste kolom.

```sh
docker rm admiring_williamson
```

<figure><img src="../.gitbook/images/docker/image (3) (3).png" alt=""><figcaption></figcaption></figure>

## Docker run shortcut

Als je `docker run` uitvoert vooraleer je een image lokaal hebt binnengehaald met `docker pull`, zal docker automatisch gaan kijken op [Docker Hub](https://hub.docker.com/) indien de image bestaat en deze downloaden.

Start een nieuwe container op voor de image [docker/getting-started](https://hub.docker.com/r/docker/getting-started). Kopieer de onderstaande code naar je terminal:

```sh
docker run -d -p 80:80 docker/getting-started
```

* `-d` is de optie voor detached mode, waarbij de container op de achtergrond wordt uitgevoerd.
* `-p 80:80` is de optie voor het doorgeven van een poort, in dit geval wordt poort 80 op de host doorgestuurd naar poort 80 in de container.

<figure><img src="../.gitbook/images/docker/image (2) (6).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
docker kan de image niet lokaal vinden, dus haalt het in de achtergrond de image op van Docker Hub.
{% endhint %}

Je kan tenslotte navigeren naar [http://localhost:80](http://localhost) en de website raadplegen in je browser. Let erop dat je container in de achtergrond aanstaat!

### **Detached (-d)**

Om een container in ontkoppelde modus te starten, gebruik je de flag `-d=true` of alleen de optie `-d`. Het is de bedoeling dat containers die in detached modus zijn gestart, worden afgesloten wanneer het rootproces dat wordt gebruikt om de container uit te voeren, wordt afgesloten, tenzij je ook de optie `--rm` opgeeft. Als je `-d` gebruikt met `--rm`, wordt de container verwijderd wanneer deze wordt afgesloten of wanneer de daemon wordt afgesloten, afhankelijk van wat zich het eerst voordoet.

Als je de optie `-d` niet meegeeft bij het uitvoeren van het `docker run` commando, wordt de container niet in detached mode uitgevoerd. In plaats daarvan zal de container in de foreground blijven en zal de output van de container direct naar de terminal gaan. Dit betekent dat de terminal bezet is totdat de container stopt of is beëindigd.

{% hint style="info" %}
Het is belangrijk om te weten dat als je de terminal sluit waar de container in de foreground wordt uitgevoerd, de container automatisch wordt gestopt. **Als je de container op de achtergrond wilt laten werken, gebruik je de `-d` optie.**
{% endhint %}

### Doorgeven van een poort (-p)

De `-p` optie bij het `docker run` commando is de optie voor het doorgeven van poorten. Dit staat bekend als "port forwarding".

Met de `-p` optie kun je specificeren welke poorten op de host moeten worden doorgegeven naar de poorten in de container. Dit is handig als je bepaalde services in de container wilt bereiken vanaf de host of vanaf andere systemen op het netwerk.

De syntaxis voor de `-p` optie is `-p <host-poort>:<container-poort>`, waarbij `host-poort` de poort is op de host en `container-poort` de poort is in de container.

Bijvoorbeeld: `docker run -p 8080:80 nginx` zal poort 80 in de `nginx` container doorgeven naar poort 8080 op de host. Hierdoor kun je de nginx webserver in de container bereiken via `http://localhost:8080` in je webbrowser.

### Linken van een directory

De `-v` optie bij het `docker run` commando is de optie voor het "mounten" van directories. Dit houdt in dat je een directory op je hostmachine linkt aan een locatie in de container.

De syntaxis is gelijkaardig aan die voor `-p`. Op de linkerkant plaats je het pad van de directory op je hostmachine, op de rechterkant plaats je het pad van de directory op je container.

Bijvoorbeeld: `docker run -v /home/user/Documents:/home/ubuntu/Documents ubuntu` zal onze eigen folder `/home/user/Documents` leesbaar delen met de container, die hem kan terugvinden onder `/home/ubuntu/Documents`.

{% hint style="info" %}
Bekijk de[ officiële documentatie](https://docs.docker.com/engine/reference/run/) van `docker run` voor meer informatie.
{% endhint %}

### Opkuisen

Als je klaar bent met een container, kan je hem stoppen via `docker container stop`, gevolgd door de naam of het ID van de container die je wil stoppen. Als je hem dan van je systeem wil verwijderen, kan dat via `docker container rm`, opnieuw gevolgd door de naam of het ID.

Om dit niet te vergeten, kan je hem ook opstarten door `--rm` toe te voegen aan het `docker run` commando. Zo vermijd je dat je een groot aantal niet-gebruikte containers bijhoudt op je systeem (en schijfruimte verspilt).

### Bronnen

* [https://tecadmin.net/docker-run/](https://tecadmin.net/docker-run/)
* [https://docs.docker.com/get-started/04\_sharing\_app/](https://docs.docker.com/get-started/04\_sharing\_app/)
* [https://docs.docker.com/engine/reference/run/#detached-vs-foreground](https://docs.docker.com/engine/reference/run/#detached-vs-foreground)

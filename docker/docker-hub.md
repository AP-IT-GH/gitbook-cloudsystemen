---
description: >-
  Docker Hub is een centrale plek waar developers Docker-images kunnen opslaan,
  delen, en ontdekken.
---

# docker hub

## Docker Hub

Docker Hub is een centrale plek waar developers Docker-images kunnen opslaan, delen, en ontdekken. Het is een cloud-gebaseerde service die ontworpen is om het proces van het ontwikkelen, testen en distributie van Docker-applicaties te vereenvoudigen. Met Docker Hub kun je je eigen Docker-images hosten en publiceren, of je kunt bestaande Docker-images van andere ontwikkelaars gebruiken voor jouw eigen projecten.

Met Docker Hub kun je bijvoorbeeld een webapplicatie bouwen die gebaseerd is op een specifiek besturingssysteem en deze vervolgens opslaan als een Docker-image. Je kunt dit Docker-image vervolgens publiceren zodat andere ontwikkelaars het gemakkelijk kunnen vinden en downloaden voor hun eigen projecten.

Docker Hub is handig voor ontwikkelaars omdat het hen helpt om efficiënter en sneller te werken. Je hoeft niet meer steeds dezelfde infrastructuur opnieuw op te zetten voor elk nieuw project. In plaats daarvan kun je Docker-images gebruiken die al op Docker Hub beschikbaar zijn

Je kan images publiceren, delen, en ontdekken op Docker Hub zodra je [een account aanmaakt](https://hub.docker.com/signup).

### Publiceren van een docker image

Je kan een image op Docker Hub publiceren met behulp van de volgende opdrachten:

```shell
docker login
docker push USERNAME/CONTAINER_NAME
```

{% hint style="info" %}
Bekijk de [officiële docker documentatie ](https://docs.docker.com/engine/reference/commandline/push/)voor meer informatie over het commando `docker push`.
{% endhint %}

### Downloaden van een docker image

Je kan een image op Docker Hub downloaden met behulp van de volgende opdracht:

```sh
docker pull IMAGE_NAME
```

{% hint style="info" %}
Bekijk de [officiële docker documentatie](https://docs.docker.com/engine/reference/commandline/pull/) voor meer informatie over het commando `docker pull`.
{% endhint %}

Dat was slechts een overzicht van de basisprincipes van Docker voordat we verder gaan met geavanceerde concepten. Houd er rekening mee dat Docker veel meer te bieden heeft dan wat we hierboven hebben besproken.

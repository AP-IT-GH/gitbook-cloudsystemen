# installation

## Installeren van Docker Compose

Compose maakt gebruik van de Docker Engine, dus je moet de Docker Engine geïnstalleerd hebben op je apparaat. Je kunt Compose op Windows, Mac en 64-bit Linux uitvoeren. Het installeren van Docker Compose is eigenlijk heel eenvoudig.

Op desktopsystemen, zoals Docker Desktop voor Mac en Windows, is Docker Compose al inbegrepen. Er zijn geen aanvullende stappen nodig.&#x20;

#### Op Linux-systemen moet je de volgende stappen uitvoeren:

Installeer de Docker Engine Voer de volgende opdracht uit om Docker Compose te downloaden

```bash
sudo curl -L " -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
```

Geef de binary toestemming, zoals hieronder:&#x20;

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Test de installatie om te controleren of het correct werkt

<pre class="language-bash"><code class="lang-bash"><strong>$ docker-compose --version
</strong>docker-compose version 1.26.2, build 1110ad01
</code></pre>

Ongeacht hoe je het hebt geïnstalleerd, zodra je Docker Compose hebt gedownload en correct werkt, kun je het beginnen te gebruiken met je Dockerfiles.&#x20;

#### Dit proces vereist drie basisstappen:

* Definieer de omgeving van je app met een Dockerfile. Op deze manier kan het worden gereproduceerd.
* Definieer de services voor je app in een `docker-compose.yml`-bestand. Op deze manier kunnen ze in een geïsoleerde omgeving worden uitgevoerd.
* Voer `docker-compose` uit om je app op te starten.

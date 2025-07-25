# Wat is Traefik?

Traefik is een reverse proxy die ontworpen is om goed samen te werken met andere moderne systemen, zoals containers.
De taak van een reverse proxy hebben we [in het onderdeel over proxy's](../proxys) besproken.
Nog eens kort samengevat: hij staat "voor" de applicatie(s) zelf, routeert inkomend verkeer en past eventuele extra stappen toe, zoals datacompressie.
Traefik ondersteunt **heel veel** setups (niet alleen Docker Compose). We geven hier een inleiding. Je zal af en toe *gericht* moeten gaan zoeken in de [Traefik documentatie](https://doc.traefik.io/traefik/) en het zal je helpen dat je al eens een kort overzicht hebt gekregen.

![architectuur Traefik](../images/traefik-architecture.png)

# Kernconcepten

Traefik heeft een aantal kernconcepten die je steeds in het achterhoofd moet houden. We benoemen ze hier al kort.

- soorten configuratie
  - dynamisch: dit kan *on the fly* wijzigen terwijl de Traefik server runt
    - hieronder vallen onder andere zaken zoals: "Welke domeinnaam leidt naar welke applicatie?"
  - statisch: dit wordt vastgelegd zodra Traefik start en kan je niet aanpassen zonder de Traefik server te herstarten
    - hieronder vallen zaken zoals: "Waar haalt Traefik zijn dynamische configuratie? Op welke poorten kan Traefik bereikt worden? Waar kunnen HTTPS-certificaten opgevraagd worden?"
- abstracties
  - providers: waar Traefik zijn dynamische configuratie haalt (door Docker aan te spreken, door een file uit te lezen,...)
  - entrypoints: hoe Traefik bereikt kan worden, in essentie een transportprotocol of applicatieprotocol en een poort
  - routers: systemen om verkeer op een bepaald entrypoint om te leiden naar een achterliggende dienst
  - middleware: systemen om verkeer te inspecteren en/of aan te passen, zoals: door datacompressie toe te passen
  - services: de achterliggende diensten waar we als eindgebruiker contact mee willen opnemen, ongeveer wat ook Docker Compose verstaat onder "services"

# Codevoorbeeld

Dit zou je kunnen terugvinden **in een Docker Compose file**:

{% hint style="info" %}
Probeer zo veel mogelijk onderdelen te herkennen en lees ook de comments!
{% endhint %}

```yaml
services:
  reverse-proxy:
    image: traefik:v3.2
    # dit voegt extra opties toe aan het defaultcommando
    # in de Dockerfile voor Traefik staat "entrypoint"
    # dat deel is vast
    # dus het "command" bevat alleen de extra opstartopties
    # --api.insecure=true is nodig voor het dashboard
    # --providers.docker zegt Traefik info over andere containers te vragen aan Docker
    command: --api.insecure=true --providers.docker
    ports:
      - "80:80"
      - "8080:8080" # web UI, kan door api.insecure=true
    volumes:
      # dit zorgt dat de Traefik instantie die zelf in Docker runt
      # Docker ook effectief kan contacteren
      - /var/run/docker.sock:/var/run/docker.sock
```

Als je dit uitvoert en naar `localhost:8080` surft, zie je het **Traefik dashboard**. Dit geeft inzicht in de werking van Traefik. Het stukje `command` zorgt hier voor de "statische configuratie". Het zegt wat er vastligt wanneer Traefik wordt opgestart (dat we het dashboard aanbieden en informatie over de services van Docker halen).

{% hint style="info" %}
Run dit en bekijk het dashboard!
{% endhint %}

Volgend voorbeeld voegt hier een echte service aan toe:

```yaml
services:
  reverse-proxy:
    image: traefik:v3.2
    command: --api.insecure=true --providers.docker
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
  whoami:
    image: traefik/whoami
    # zie verderop hoe dit label is opgebouwd
    # nu moet je gewoon weten dat het metadata over de whoami container voor Traefik bevat
    labels:
      - "traefik.http.routers.towhoami.rule=Host(`whoamiservice.localhost`)"
```

{% hint style="info" %}
Run dit, bekijk het dashboard en bezoek de "whoami" service!
{% endhint %}


Omdat we de Docker provider gebruiken, zal Traefik de info onder `labels` kunnen opvragen. In dit geval gaat het om een regel die aangeeft dat we de `whoami` service kunnen aanspreken door naar `whoamiservice.localhost` te surfen. **Merk op: dit is niet zo vanzelfsprekend! `whoami` bevat geen onderdeel `ports`!** De container luistert wel op poort 80, maar we surfen er niet rechtstreeks naartoe. We surfen naar de reverse proxy (die poort 80 wel beschikbaar maakt). Die kan contact opnemen met de `whoami` service (omdat die in hetzelfde Docker netwerk staat). Van buitenaf kunnen we niet rechtstreeks met `whoami` communiceren!

# Statische en dynamische configuratie
![statische en dynamische configuratie](../images/static-dynamic-configuration.png)

De statische configuratie (info die bij opstart van de server vastgelegd wordt) van Traefik kan ingesteld worden:

- via command line argumenten (zie Traefik documentatie voor welke)
- via een YAML file (zie Traefik documentatie voor inhoud, zie Docker Hub voor instellen)
- via omgevingsvariabelen (zie Traefik documentatie voor welke)

Deze omvat op zijn minst:

- welke entrypoints er zijn (transportprotocol of applicatieprotocol in combinatie met poort)
- welke providers er zijn (haalt Traefik info over services uit `labels` zoals hierboven, uit een aparte file,...)

De dynamische configuratie geeft al de rest: hoe wordt specifiek verkeer geïdentificeerd, waar moet het naartoe,... Je zal voorbeelden van deze configuratie in verschillende vormen tegenkomen, bijvoorbeeld in YAML:

```yaml
# dit is geen onderdeel van de Docker Compose file!
# dit kan je apart noteren in een file met de dynamische configuratie
# als je die file dan aanpast terwijl de Traefik instantie runt, wijzigt het gedrag van Traefik

http: # deze info is enkel van toepassing op HTTP-verkeer
  routers: # hieronder kan je meerdere routers definiëren
    my-router: # dit is de naam van één zo'n router
      rule: "Host(`whoamiservice.localhost`)" # deze router is van toepassing als we dat domein bezoeken
      service: service-foo # als hij van toepassing is, leidt de router naar deze achterliggende service
```

Maar je kan zoiets even goed uitdrukken via een Docker label:
```yaml
# deze router handelt verkeer af waarbij de domeinnaam whoamiservice.localhost is
# je kan zo meerdere labels achter elkaar zetten
- "traefik.http.routers.my-router.rule=Host(`whoamiservice.localhost`)"
# je kan bijvoorbeeld nog extra verkeer afhandelen via:
# - "traefik.http.routers.my-router.rule=Host(`otherservice.localhost`)"
# dan werkt dezelfde router ook voor een ander domein
```

Je zou hier nog een label kunnen verwachten:

```yaml
- "traefik.http.routers.my-router.service=service-foo"
```

Maar deze wordt niet uitgedrukt via een label omdat labels zelf al bij Docker Compose services genoteerd staan.

Als je info vindt die je kan helpen een probleem op te lossen in één van deze formaten, moet je in staat zijn de oplossing uit te drukken in het formaat dat je reeds aan het gebruiken bent!

{% hint style="warning" %}
Om Docker labels te kunnen gebruiken, delen we het Docker socket via een bind mount. Dat is goed om snel te testen, maar het omvat een veiligheidsrisico. Als iemand Traefik kan hacken, kan hij **heel de hostmachine** hacken. Zet dit dus niet zomaar in productie, maar lees de uitleg hierrond in de Traefik documentatie grondig.
{% endhint %}

![overzicht architectuur Traefik](../images/architecture-overview.png)
# Uitgebreider voorbeeld

Onderstaand voorbeeld is een sjabloon, maar werkt nog niet volledig. Onderzoek hoe je het aan de praat krijgt!

{% hint style="info" %}
Eventueel haal je alles rond middleware weg, zowel uit het onderdeel rond de router als in de definitie van de middleware zelf. Als het zonder middleware lukt, kan je die daarna nog steeds toevoegen.
{% endhint %}

{% hint style="warning" %}
Staar je niet blind op alleen de Traefikconfiguratie. Entrypoints zijn maar bruikbaar als Docker de juiste poorten opent.
{% endhint %}

## Statische configuratie
```yaml
# dit wordt allemaal vastgelegd bij opstart van Traefik
entryPoints:
  # "web" is de naam van een manier om Traefik te bereiken
  web:
    # alle verkeer (TCP of UDP) dat naar poort 8081 gaat valt onder "web"
    address: :8081
# dit is waar Traefik dynamische configuratie haalt
providers:
  # de file provider betekent dat er ergens een file is met routers,...
  # de Docker provider betekent dat die informatie uit Docker labels gelezen wordt
  # er zijn nog providers mogelijk
  file:
    # dit pad kies je zelf, het leidt gewoon naar de file in kwestie
    filename: /path/to/dynamic/conf
```

## Dynamische configuratie
```yaml
# onderstaande informatie is enkel van toepassing voor dit protocol
# je komt op dit niveau ook "tls" tegen (dat omvat ook https), "tcp" en "udp"
http:
  routers: # dus hier kan je routers onder definiëren
    to-whoami: # dit is de naam van een router, mag je kiezen
      # de router is van toepassing op verkeer zoals http://example.localhost/whoami
      # maar niet op ander verkeer zoals example.localhost/whoareyou
      # of example2.localhost/whoami
      rule: "Host(`example.localhost`) && PathPrefix(`/whoami/`)"
      # dit voegt een extra verwerkingsstap toe
      # de definitie van deze middleware staat een paar regels verder
      # test eens uit met middlewares: [] om het verschil te zien
      middlewares:
        - test-user # zie onder, dit is de naam van een middleware
      service: whoami
  # de middleware die hierboven geactiveerd is, moet hier gedefinieerd worden
  middlewares:
    # naam van de middleware
    test-user:
      # het soort middleware
      # je vindt een boel mogelijkheden in de Traefik documentatie
      basicAuth:
        users:
        # dit is niet het wachtwoord, maar de hash
        # run htpasswd -nb usernaamnaarkeuze wachtwoordnaarkeuze
        # dat kan in een httpd container
        - user:hashcode
  services:
    # naam van een service kies je zelf
    whoami:
      # een load balancer verdeelt inkomende requests over containers die hetzelfde programma runnen
      # dit vermijdt overbelasting
      # uit de docs: "each service has a load-balancer, even if there is only one server to forward traffic to"
      loadBalancer:
        servers:
        # dit is een lijst met adressen voor die achterliggende containers
        # dit kunnen IP-adressen zijn, maar in Docker Compose mogen het ook namen zijn
        # HET PROTOCOL MOET VERMELD WORDEN, zelfs als het http is
        - url: http://private/whoami-service
```

## Middleware
Middleware zit "in het midden" tussen HTTP request en response.
Je kan middleware dus gebruiken om extra aanpassingen te doen aan beide.
Je vindt [hier](https://doc.traefik.io/traefik/middlewares/http/overview/) een uitgebreide lijst met HTTP-middleware.
Enkele voorbeelden:

- login vereisen zonder accountsysteem (de `basicAuth` middleware)
- comprimeren van de response (de `Compress` middleware; kost rekentijd, maar leidt tot minder netwerkverkeer)
- weghalen van een deel van het pad (de `StripPrefix` middleware; zo kan `mywebsite.com/a/b/c` bijvoorbeeld herschreven worden zodat `/b/c` zichtbaar is voor de ontvanger maar `/a` niet)
  - dit kan je testen met de `whoami` image, probeer met het eerdere voorbeeld eens `example.localhost/whoami/test`

# Officiële guides
{% hint style="danger" %}
Niet alle versies van Traefik gebruiken dezelfde settings en er komt veel YAML aan te pas. Reken dus niet te veel op LLM's. Probeer liever in kleine stapjes je werk te verbeteren, te controleren en te committen.
{% endhint %}

De documentatie van Traefik is heel volledig en bevat verschillende nuttige stappenplannen specifiek voor Docker Compose.
Doorloop deze in volgorde en observeer zo goed mogelijk (via dashboard) wat er gebeurt:

- [quick start met Docker Compose](https://doc.traefik.io/traefik/getting-started/docker/)
- [setup met Docker Compose](https://doc.traefik.io/traefik/setup/docker/)
- [user guide Docker Compose](https://doc.traefik.io/traefik/user-guides/docker-compose/basic-example/)

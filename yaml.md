# YAML: YAML Ain't Markup Language
YAML is een "dataserialisatieformaat" dat heel populair is in de wereld van cloudsystemen, DevOps, GitOps,... De naam is een "recursief acroniem". Ooit stond "YAML" voor "Yet Another Markup Language", maar eigenlijk is het geen taal voor het opstellen van documenten (in tegenstelling tot bijvoorbeeld HTML). Het is een taal om data en configuratie in bij te houden. YAML ligt dus dichter bij een taal zoals JSON dan bij HTML.

Je zal bijna gegarandeerd nog in aanraking komen met YAML.
Hier zijn enkele bekende toepassingen van het formaat:

- Docker Compose (komt in deze cursus aan bod)
- CI/CD tools zoals Github Actions (komt in deze cursus aan bod)
- Traefik (komt in deze cursus aan bod)
- Ansible (komt hier niet aan bod maar is nauw verwant)
- Kubernetes (komt hier niet aan bod maar is nauw verwant)
- metadata bij Markdown files (bijvoorbeeld in static site generators zoals Jekyll)
- allerlei gebruikersapplicaties zoals Tmuxinator

YAML wordt bijna uitsluitend gebruikt om configuratie in een plain text file te noteren en die configuratie dan later ergens in te laden.

## Basisgebruik

Een YAML file krijgt typisch de extensie `.yml` of `.yaml`. Normaal mag je kiezen.
YAML files mogen comments bevatten. Deze beginnen met `#`. Je mag meerdere "YAML documenten" in één file noteren als je ze van elkaar scheidt met `---`.

De eigenlijke data behoort tot drie grote groepen:

- mappings: deze stemmen ruwweg overeen met JSON records en bevatten key-value paren
- sequences: deze stemmen overeen met arrays/lijsten
- scalars: dit zijn waarden die we als één geheel zien, zoals strings, getallen, booleans en de nullwaarde

Whitespace geeft aan welke zaken op hetzelfde niveau staan.

{% hint style="info" %}
Dit is niet de volledige YAML syntax.
Bovendien bestaan er meerdere versies van de YAML syntax.
Zie [de YAML specificatie](https://github.com/yaml/yaml-spec) indien je hier meer over wil weten.
{% endhint %}

Bekijk dit voorbeeld:

```yaml
company: spacelift
domain:
 - devops
 - devsecops
tutorial:
  - yaml:
      name: "YAML Ain't Markup Language"
      type: awesome
      born: 2001
  - json:
      name: JavaScript Object Notation
      type: great
      born: 2001
  - xml:
      name: Extensible Markup Language
      type: good
      born: 1996
author: omkarbirade
published: true
status: ~
```

Hier staan `company`, `domain`, `tutorial`, `author` en `published` allemaal op ehtzelfde niveau.
Al deze tokens worden gevolgd door een dubbele punt en meer data.
Dat betekent dat het keys zijn in een key-value mapping.
Met andere woorden, in JSON zou dit ongeveer iets zijn van de vorm:

```json
{
  "company": "spacelift",
  "domain": ...,
  "tutorial": ...,
  "author": "omkarbirade",
  "published": true,
  "status": null
}
```

Wat er in YAML "uitziet" als een gewone string zonder spaties wordt een gewone string. Quotes zijn niet verplicht. Dat kan soms verwarrend zijn, maar het maakt het wel wat eenvoudiger de meeste bestanden te lezen en te schrijven. Zo merk je bijvoorbeeld dat de waarde voor de key `"published"` nu `true` is en niet `"true"`. Dat is dus geen string, maar een boolean! De `~` is de YAML-notatie voor `null`.

De onderdelen `"domain"` en `"tutorial"` zijn iets moeilijker te verklaren, omdat de *values* daar geen scalars zijn. In dit geval zijn het sequences (lijsten) omdat de waarde uit meerdere onderdelen bestaat die telkens voorafgegaan worden door `- `.

Dus als we verder omzetten naar JSON, krijgen we:

```json
{
  "company": "spacelift",
  "domain": ["devops", "devsecops"],
  "tutorial": ...,
  "author": "omkarbirade",
  "published": true,
  "status": null
}
```

Elk streepje duidt dus op een lijstelement en dat lezen we op dezelfde manier af als eerder. `devops` en `devsecops` *zien eruit* als strings, dus het worden strings.

Voor het laatste onderdeel, `tutorial`, hebben we terug een lijst, maar elk element is terug een mapping. We krijgen:

```json
{
  "company": "spacelift",
  "domain": ["devops", "devsecops"],
  "tutorial": [
    { "yaml": ... },
    { "json": ... },
    { "xml": ...}
  ],
  "author": "omkarbirade",
  "published": true,
  "status": null
}
```

Opnieuw, alles wat voorafgegaan wordt door een streepje is een element in een lijst.
Voor de laatste ontbrekende delen kan je inzoomen op enkel de lijstelementen.
Bijvoorbeeld:

```yaml
yaml:
  name: "YAML Ain't Markup Language"
  type: awesome
  born: 2001
```

Op zichzelf zou dit `{"yaml": {"name": "YAML Ain't Markup Language", "type": "awesome", "born": 2001}}` worden. Als we dit doen voor de drie onderdelen en invullen in het geheel, rkijgen we uiteindelijk:

```json
{
  "company": "spacelift",
  "domain": [
    "devops",
    "devsecops"
  ],
  "tutorial": [
    {
      "yaml": {
        "name": "YAML Ain't Markup Language",
        "type": "awesome",
        "born": 2001
      }
    },
    {
      "json": {
        "name": "JavaScript Object Notation",
        "type": "great",
        "born": 2001
      }
    },
    {
      "xml": {
        "name": "Extensible Markup Language",
        "type": "good",
        "born": 1996
      }
    }
  ],
  "author": "omkarbirade",
  "published": true,
  "status": null
}
```

## Gebruik van JSON syntax
De meeste JSON syntax werkt ook in YAML. Je kan strings tussen quotes zetten. Je kan lijsten noteren met `[` en `]` en plaats van met streepjes.

Dit is bijvoorbeeld een andere notatie voor hetzelfde document:

```yaml
company: spacelift
# JSON-notatie voor dit onderdeel
domain: ["devops", "devsecops"]
tutorial:
  - yaml:
      name: "YAML Ain't Markup Language"
      type: awesome
      born: 2001
  - json:
      name: JavaScript Object Notation
      type: great
      born: 2001
  - xml:
      name: Extensible Markup Language
      type: good
      born: 1996
author: omkarbirade
published: true
# opnieuw JSON notatie
status: null
```

## YAML versus JSON
JSON is een vrij simpel en expliciet formaat. Dit komt voornamelijk omdat strings altijd expliciet tussen quotes moeten staan en omdat haakjes expliciet zijn. YAML bevat meer features en een complexere syntax (optionele quotes en comments, maar ook zaken die we hier niet behandelen zoals multiline strings, anchors). Het voornaamste voordeel is dat het vlot leest en vaak makkelijk te schrijven is.

Het resultaat is dat JSON lezen en schrijven meer een karwei is, maar dat je in YAML makkelijker subtielere fouten kan maken.

{% hint style="warning" %}
YAML is populair en je zal het gegarandeerd tegenkomen, maar je mag er kritisch naar kijken. [Deze blog post](https://ruudvanasseldonk.com/2023/01/11/the-yaml-document-from-hell) benoemt terecht een aantal tekortkomingen en alternatieven.
{% endhint %}

Een conversietool van en naar JSON (zoals [deze](https://www.bairesdev.com/tools/json2yaml/)) kan je helpen YAML beter te begrijpen en te debuggen.

## Tips
- LLM's zijn niet zo sterk in YAML, omdat YAML er makkelijk goed kan "uitzien" zonder het ook te zijn.
- Je kan in je editor een plugin installeren voor YAML syntax. Dat kan een algemene plugin zijn of een specifieke (bijvoorbeeld de Docker plugin die heel nuttig is voor het schrijven van de YAML files van Docker Compose).
- Indien je moeite hebt met YAML syntax: neem ergens wat JSON (bijvoorbeeld uit de [pokeapi](https://pokeapi.co/) en zet deze **met de hand** om. Controleer dan met de converter. Dit is niet "nuttig" maar het zal wel zorgen dat je het formaat begrijpt. Je kan achteraf ook in de omgekeerde richting oefenen.

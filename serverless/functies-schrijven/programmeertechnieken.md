# Programmeertechnieken

## Omgevingsvariabelen
Als we het gedrag van functies willen wijzigen zonder de code aan te passen, doen we dat vaak via omgevingsvariabelen. Omdat de Dockerfile van de template niet bedoeld is om aan te passen, doen we dat voor OpenFaaS in de stack file (dat is de .yml file met metadata over de functie), met dezelfde syntax als in Docker Compose files.

## Secrets
Geheime informatie zetten we niet in bestanden die in versiebeheer of op image repositories terecht kunnen komen. We signaleren in de stack file wel dat bepaalde secrets nodig zijn, maar we registreren hun waarden rechtstreeks bij de provider. In het geval van OpenFaaS betekent dat: "in een file ver verwijderd van onze functiecode".

Dit ziet er zo uit in de functiedefinitie:

```yaml
functions:
  mijnfunctie:
    lang: node18 # of iets anders natuurlijk
    handler: ./mijnfunctie # verwijst naar de folder met code
    image: mijngebruikersnaam/mijnfunctie:latest # verwijzing op een image repository
    secrets:
      - secret1
      - secret2
```

In de code voor een functie lezen we deze uit het filesysteem:

```javascript
// ingebouwde functionaliteit van NodeJS voor asynchrone file handling
const { readFile } = require('fs').promises;
module.exports = async(event, context) => {
  // misschien nog wat code

  // elk secret wordt opgevraagd via het filesysteem van de container van de functie
  const secret1 = await readFile("/var/openfaas/secrets/secret1");
  const secret2 = await readFile("/var/openfaas/secrets/secret2");

  // rest van de code
  // bovenstaande variabelen zullen van het type Buffer zijn, maar je kan ze omzetten naar strings via .toString()
};
```

Om de waarde voor een secret te registreren, kunnen we de command line gebruiken: `faas secret create secret1 --from-literal DEWAARDEHIER`.

## Binaire data gebruiken
Je kan functies schrijven die werken met binaire data, zoals afbeeldingen of audiobestanden.
Hoewel het mogelijk is dit soort data door te sturen met de header `Content-Type: application/octet-stream`, is dit soms moeilijk om mee te werken.
Binaire kan bijvoorbeeld zorgen voor problemen met de weergave van logbestanden.

Vaak wordt gebruik gemaakt van "base 64" encodering om hier mee om te gaan.
Hierbij worden de nulletjes en eentjes in je binair bestand in groepjes van zes genomen (want 2^6 = 64) en omgezet naar 64 symbolen uit de ASCII karakterset.
De exacte werking is hier minder belangrijk, maar je moet vooral dit onthouden:

1. base 64 geëncodeerde data kan als plain text verstuurd worden
2. je kan in Bash een bestand converteren naar base 64 met `base64 NAAM_VAN_BINAIR_BESTAND` en je kan een base 64 bestand decoderen (en opslaan) met `base64 --decode NAAM_VAN_PLAIN_TEXT_BESTAND > NAAM_VAN_BINAIR_BESTAND` 
3. base 64 geëncodeerde data is iets groter dan de oorspronkelijke binaire data (gemiddeld 1/3 groter)

### Werken met base 64 in JavaScript
In JavaScript kan je geëncodeerde data omzetten naar een `Buffer` met bytes (min of meer een array) via: `Buffer.from('ABC...','base64')`, waarbij je "ABC..." vervangt door de base 64 string.
Je kan terug omzetten via de methode `.toString('base64')`.

## Unit testen
Het is erg omslachtig om telkens een serverless functie te deployen om ze dan uit te testen.
Dit hoeft ook niet.
De faasd-template voor Node 18 voorziet in de bijbehorende Dockerfile, als laatste stap voor de serverless functie wordt opgestart, het commando `RUN npm test`.
Voor andere programmeertalen kan een overeenkomstig commando worden ingevuld (bijvoorbeeld `dotnet test` voor C♯).
De exacte betekenis van `npm test` wordt vastgelegd in package.json dus je kan testen naar keuze laten lopen tijdens de build van je container, voor je functie online wordt gezet.

{% hint style="warning" %}
Niet alle templates voorzien een teststap, maar je kan ook eigen templates maken waarin deze stap wel voorzien is.
{% end hint %}

In JavaScript kan je bijvoorbeeld de Mocha test runner en de Chai assertion library installeren door dit toe te vogen aan package.json:

```json
"dependencies": {
  "chai": "^4.2.0",
  "mocha": "^7.1.0"
}
```

Als je dan je testen in een bestand genaamd test.js plaatst, kan je (ook in package.json) de actie `test` definiëren als `mocha test.js`.
De inhoud van test.js ziet er dan ongeveer zo uit:

```javascript
'use strict'
const expect = require('chai').expect // require syntax vereist iets oudere versie
const handler = require('./handler')

// eventueel nog algemeen beschikbare waarden
describe('typisch één use case hier', async function() {
    it('typisch één pad doorheen de code hier', async function() {
        const function_context = { /* voorzie alles om een context te simuleren */ };
        const event = { /* voorzie alles om een event te simuleren */ };
        await handler(event, function_context);
        expect(function_context.status()).to.equal(200); // of een andere statuscode
        expect(function_context.data).to.equal(/* verwachte output van de functie */);
    });
});
```

### Testomgeving afbakenen
Soms moet je code zich anders gedragen tijdens unit testen, bijvoorbeeld door geen echte HTTP queries uit te voeren maar ingeblikte data te gebruiken. Dat kan je bijvoorbeeld doen door een omgevingsvariabele in te stellen die aangeeft of je in een testomgeving aan het uitvoeren bent. In Bash shellcommando's kan je dit vlak voor de commando's zelf, dus bijvoorbeeld `NODE_ENV=test mocha test.js`. Indien deze variabele al bestond in een bredere omgeving, maskeert de meer specifieke waarde de algemenere waarde.

## Bouwen in een pipeline
Je hoeft je functies niet lokaal te bouwen.
Het is heel gebruikelijk om functies te bouwen in een continuous integration pipeline, bijvoorbeeld wanneer een nieuwe versie op de `main` branch wordt geplaatst.
Dit kan met enkele extra stappen in een Github Actions workflow:

```yaml
# nodig om commando's te kunnen uitvoeren
- name: Download and install faas-cli
  run: curl -sLSf https://cli.openfaas.com | sudo sh
# vereist om Docker images te kunnen bouwen
- name: Set up QEMU
  uses: docker/setup-qemu-action@v2
- name: Set up Docker Buildx
  uses: docker/setup-buildx-action@v2
# effectief bouwen van de functie zoals je dat lokaal doet
- name: Build function only
  run: faas-cli build -f image-processing.yml
```

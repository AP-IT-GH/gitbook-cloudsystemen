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
};
```

Om de waarde voor een secret te registreren, kunnen we de command line gebruiken: `faas secret create secret1 --from-literal DEWAARDEHIER`.

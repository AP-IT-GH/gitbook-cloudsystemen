# Hello, world! als serverless functie

## Structuur
Serverless functies worden verpakt in handlers voor een web framework (in om het even welke programmeertaal). Je hoeft zelf niet vertrouwd te zijn met een web framework in de taal van je functie, maar je moet de omringende code wel genereren via een **template**.

**Voor elke functie** die we schrijven, maken we dan een nieuwe container image gebaseerd op de template. Deze container zal gestart worden wanneer de functie moet lopen.

Je kan beschikbare templates laten zien via `faas template store list`. Deze templates zijn ondersteunde omgevingsuitvoeringen. Ze duiden in de eerste plaats op het soort container image waarin je functie uiteindelijk zal lopen. Templates worden bewaard als tekstbestanden in Git repositories, dus je kan er makkelijk zelf nieuwe aanmaken.

Templates bevatten volgende folders/files **waar de auteur van een functie niets aan hoeft aan te passen**:

- `templatenaam/`
- `templatenaam/template.yml`: metadata over de template (omschrijving enzovoort)
- `templatenaam/Dockerfile`: instructies om een image te maken
- `templatenaam/index.js` of `templatenaam/index.php` of `templatenaam/index.py` of `templatenaam/main.go` of iets gelijkaardigs: een programma dat een web server opstart die het oproepen van onze functie afhandelt
- `templatenaam/function/`: hierin staat de code die je als applicatieprogrammeur zal moeten aanpassen

Voor NodeJS functies vind je bijvoorbeeld volgende files die je normaal wel zal moeten aanpassen:

- `templatenaam/function/handler.js`
- `templatenaam/function/package.json`

Voor andere programmeertalen vind je ook bronbestanden en een lijst dependencies.

## Zelf een functie schrijven
Maak eerst een omgevingsvariabele `OPENFAAS_PREFIX` met als waarde jouw naam op Docker Hub.

Open eerst een shell in een map waarin je de broncode voor je functie wil bewaren. Noem deze map `helloworld-serverless-function`. Navigeer in je shell naar deze map en geef dan in: `faas new --lang node18 helloworld`.

Je zou volgende zaken moeten zien verschijnen:

- `template/` (met info over de template waar je niet aan hoeft te komen)
- `.gitignore` (je code staat niet automatisch onder versiebeheer, maar dit vermijdt het toevoegen van ongewenste bestanden)
- `helloworld/handler.js` (hierin hoort jouw code)
- `helloworld/package.json` (voor je dependencies)
- `helloworld.yml` (instructies rond build en deployment voor deze functie, ook de **stack file** genoemd)
  - hierin kan je onder andere extra packages meegeven die geen deel uitmaken van de Docker image voor de template
  - een referentie rond de stack file vind je [hier](https://docs.openfaas.com/reference/yaml/)

De file `handler.js` zal er ongeveer zo uitzien:

```javascript
module.exports = async (event, context) => {
  // hier kan nog het een en het ander staan
  return context
    .status(200)
    .succeed(result)
}
```

{% hint style="info" %}
De Node 18 template gebruikt op het moment van schrijven het oudere CommonJS systeem. Je moet dus gebruik maken van `module.exports = ...` voor de default export en je moet dependencies importeren via `require` in plaats van via `import`.
{% endhint %}

De `event`-parameter bevat request informatie.
Belangrijke attributen zijn:

- `event.query` (de query string)
- `event.headers` (in het bijzonder belangrijk voor het controleren van toegang)
- `event.body` (de request body; deze is automatisch omgezet naar een object indien `Content-Type` duidt op JSON)

De `context`-parameter bevat response informatie.
Belangrijke methodes zijn:

- `context.status` (om de responscode in te stellen)
- `context.succeed` (om een succes aan te duiden)
- `context.fail` (om een probleem aan te duiden)

Zorg eerst dat de Docker daemon aan staat. Dan kan je de functie deployen door middel van `faas up -f helloworld.yml`.

{% hint type="warning" %}
Als de image juist wordt gebouwd, maar dan niet kan worden afgehaald, moet je naar Docker Hub gaan en de repository public maken. Dat moet maar één keer gebeuren.
{% end hint %}

Je kan de functie oproepen via de web interface.
Je kan dit ook doen via `faas invoke helloworld`.
Indien invoer verwacht wordt en het niet lukt om deze in te typen, gebruik je `echo "" | faas invoke helloworld`.

Je kan de functie ook oproepen via `curl $OPENFAAS_URL/function/helloworld`. Hier wordt impliciet HTTP GET gebruikt, maar kies steeds een HTTP methode die overeenstemt met wat de functie doet. Hoewel de serverless functie de methode niet ziet, kan de method wel een invloed hebben op andere zaken (tussenliggende proxy's enzovoort).

{% hint type="info" %}
Is dit niet gewoon een andere schrijfwijze voor een Express handler? Niet bepaald. Vergeet niet dat serverless functies automatisch kunnen opschalen om aan de vraag te voldoen en dat je geen server hoeft te reserveren.
{% endhint %}

{% hint type="info" %}
Staat de functie in de lijst, maar kan je ze niet oproepen omdat de "invoke"-knop grijs is? Dan is de onderliggende code misschien gecrasht. Controleer via `faas logs helloworld`.
{% endhint %}

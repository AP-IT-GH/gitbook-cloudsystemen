# faasd gebruiken via CLI

## curl
Uiteindelijk worden serverless functies opgeroepen wanneer een HTTP request ontvangen wordt.
Je kan serverless functies dus oproepen via de command line met `curl`.

## aangepaste client
Een iets handigere manier is via de command line client.
Een volledige lijst met commando's krijg je via `faas-cli` of `faas` (op de meeste systemen zijn beide commando's gelijkwaardig).
Belangrijke subcommando's zijn:

- `new` (om een nieuwe functie aan te maken op basis van een template)
- `invoke` (om een serverless functie op te roepen; indien input verwacht wordt en ctrl+d niet werkt, kan je `echo "mijnteksthier" | faas invoke FUNCTIENAAM` gebruiken)
  - via `faas invoke --help` kan je zien hoe je meer detail in je queries kan steken
- `list` (om de gedeployde functies te zien)
- `logs FUNCTIENAAM` (om de terminal output voor een functie te bekijken)
- `secret` (om geheime data te beheren)
- `template store list` (om beschikbare functietemplates te zien)
- `up -f NAAMVANSTACKFILE.yml` (om een functie te deployen)

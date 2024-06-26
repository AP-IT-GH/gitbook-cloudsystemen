# Apple silicon

{% hint style="info" %}
Deze pagina is enkel relevant voor wie een vrij recente Mac heeft.
{% endhint %}

Sinds 2020 bouwt Apple zijn eigen processoren op basis van ARM. Omdat deze processoren een andere instructieset gebruiken dan x86\_64 processoren, moet zowat alle software apart gecompileerd worden voor deze processoren.

## Optie 1 (te verkiezen): buildx

Via de `docker buildx`-tool kan je Docker images en dus ook serverless functies bouwen voor Linux. Er staat een gedeelde faasd online die op Linux draait, dus dan kan je je functies naar daar pushen. Dat zou als volgt moeten gaan:

{% hint style="warning" %}
Ik beschik niet over een eigen Mac om dit uit te proberen, dus laat zo snel mogelijk weten als de instructies niet werken.
{% endhint %}

1. Zorg voor een "builder": `docker buildx create --name mybuilder --use`
2. Start hem op: `docker buildx inspect --bootstrap`
3. Controleer dat x86\_64 ondersteund wordt: `docker buildx inspect --bootstrap` (de architectuur zou hiertussen vermeld moeten worden)
4. In plaats van `faas-cli up`, gebruik je eerst `faas-cli build --shrinkwrap`
5. Navigeer naar de build directory van je functie: `cd build/<functienaam>`
6. Voer uit: `docker buildx build --platform linux/amd64 -t <dockerhubusername>/<functienaam>:latest .`
7. Deploy naar Docker Hub: `docker push <dockerhubusername>/<functienaam>:latest`
8. Doe enkel het deployment gedeelte via de YAML-file: `faas-cli deploy -f functienaam.yml --gateway https://jaws2paws.com` 8.1 Eventueel moet je eerst inloggen: `faas-cli login --gateway https://jaws2paws.com --password <kijk op DigitAP>`

## Optie 2: Github Actions

Indien optie 1 niet lukt, kan je Github Actions gebruiken om je functie te bouwen. Zet dan de gateway en het wachtwoord in een secret en gebruik de instructies op [deze pagina](../functies-schrijven/programmeertechnieken.md#bouwen-in-een-pipeline). Voeg de deploy stap toe zoals bij buildx.

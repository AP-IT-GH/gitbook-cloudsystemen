Er zijn veel manieren om automatisch software te deployen als je beschikt over een CI/CD-systeem. De eerste stap zal tegenwoordig bijna altijd het bouwen van een of meerdere Docker images zijn. Die kunnen dan makkelijk naar Docker Hub of een andere repository gepubliceerd worden. Github Actions stelt bijvoorbeeld meteen een workflow voor om dit te doen als je repository een Dockerfile bevat.

## SSH
Eén manier om je software automatisch te deployen bestaat erin via SSH in te loggen op de productieserver en daar de nodige commando's (bijvoorbeeld `docker compose build` en `docker compose up`) uit te voeren. Je mag uiteraard geen private key in je repository plaatsen, maar je kan bijvoorbeeld wel een wachtwoord bijhouden door middel van een secret. De gewone versie van `ssh` ondersteunt niet dat je via een wachtwoord op de command line verbindt, maar het programma `sshpass` doet dat wel, via de optie `-p`.

Je kan niet eerst `sshpass -p mypassword user@host` gebruiken van uit een pipeline (omdat dat commando blokkeert en verdere commando's niet in de remote shell zullen uitvoeren). Je kan wel gebruik maken van de syntax `ssh -p mypassword user@host 'command'` om in te loggen en op de remote shell één specifiek commando uit te voeren. Als je een reeks commando's wil uitvoeren, kan je dit doen:

```
sshpass -p mypassword user@host <<EOF
echo "Running a script"
mkdir -p /tmp/example
ls -la /tmp/example
EOF
```

Dit is erg makkelijk op te zetten, maar vereist wel dat er een poort open staat voor SSH. De werkwijze die hier gegeven wordt maakt het wachtwoord ook zichtbaar, dus denk eraan `mypassword` te vervangen door een secret.

## Web hook
Een "web hook" is eigenlijk een API call, zoals je die al kent. Het verschil zit hem in het perspectief. Bij een web hook vertel je aan een bestaande dienst (zoals Github Actions) dat je wil dat deze API call plaatsvindt in bepaalde situaties (zoals bij elke push naar een specifieke branch). Je schrijft zelf gewoon een handler (bijvoorbeeld met Express) die bereikbaar is over HTTP(s) voor de bestaande dienst.

Je vindt makkelijk een reeds verpakte action voor het uitvoeren van een web hook via de Github Actions marketplace. Je kan eigenlijk ook gewoon `curl` gebruiken. Alternatief kan je [webhooks zonder Actions](https://docs.github.com/en/webhooks/using-webhooks/creating-webhooks) gebruiken.

Aan de ontvangerskant zet je dan een programma dat de eigenlijke applicatie herstart, bijvoorbeeld een klein Express script dat je buiten je normale applicatiestack runt en dat in staat is de applicatie te herstarten. Eventueel gebruik je hiervoor een library zoals [de NodeJS docker-compose package](https://www.npmjs.com/package/docker-compose).

Deze aanpak houdt niet in dat je SSH mogelijk maakt. Als iemand hier misbruik van kan maken, kan hij de applicatie herstarten en/of onbereikbaar maken, maar heeft hij niet meteen toegang tot het systeem. Het is wel wat meer werk, omdat je de Express applicatie ook zal moeten beveiligen met op zijn minst HTTPS en een wachtwoord.

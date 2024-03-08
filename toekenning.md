# Toekenning van IP-adressen

## Publiek versus privé
Een "publiek" IP-adres is er één dat je als gewone gebruiker kan gebruiken om een host op het Internet te contacteren.
In principe zou je een webpagina mogen verwachten als je dit invult in je browserbalk en als de machine in kwestie een web server aan het runnen is.

Sommige subnetten bestaan niet uit publieke adressen, maar uit privé-adressen.
Deze adressen kan je niet vinden via het Internet, maar ze mogen gebruikt worden voor thuisnetwerken of andere soorten interne netwerken.

Volgende netwerken zijn hier voor gereserveerd:

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

Als je een adres ziet dat binnen deze netwerken valt, kan je het dus niet bereiken, tenzij je zelf in dezelfde privé-adresruimte zit.

Als je op een van je toestellen thuis je IP-adres opvraagt, zal normaal gezien een adres in één van deze adresbereiken zien.
Je echte publieke IP-adres kan je bijvoorbeeld achterhalen via de settingspagina van Telenet of Proximus.

Daarnaast is er nog een adresbereik gereserveerd: `127.0.0.0/8`.
Dit hele bereik geeft "localhost" of "de huidige machine" aan.
Meestal wordt `127.0.0.1` gebruikt, maar je kan de laatste byte variëren.

## Verdeling
Routering is een hiërarchisch gebeuren en het beheer van adressen is dat ook.
Een organisatie (zoals een ISP) koopt bij de overkoepelende beheersorganisatie IANA IP-adressen aan in grote blokken.
De organisatie kan die dan weer in kleinere blokken toewijzen aan haar eigen klanten, enzovoort.
Als particulier heb koop je typisch één publiek IP-adres van je ISP.

Er zijn breed gesproken twee manieren om IPv4-adressen toe te kennen.
De conceptueel eenvoudigste, maar praktisch lastigste, is het gebruik van statische adressen.
Deze houdt in dat in de netwerkbeheerder met de hand een bepaald hardwareadres koppelt aan een IP-adres.
Dit is voorspelbaar, maar weinig flexibel.
Wijzigingen moeten met de hand gedaan worden en een probleem in de configuratie zet zichzelf niet recht.
Zeker in grote netwerken is dit niet werkbaar.
Je kan niet verwachten dat elke werknemer of bezoeker zich elke dag aanmeldt bij de systeembeheerder om een IP-adres te krijgen.

Daarom is er ook automatische configuratie mogelijk.
Hierbij wordt een zogenaamde DHCP-server (dynamic host control protcol) geïnstalleerd.
Deze weet welke IP-adressen toegekend mogen worden binnen het netwerk.
Vereenvoudigd komt het er dan op neer dat een nieuw toestel een broadcast doet om te vragen of er iemand is die een IP-adres kan toekennen. De DHCP-server zal dit dan ook doen. Het volledige proces is wat uitgebreider, maar het volstaat dat je weet dat adressen statisch of dynamisch toegekend kunnen zijn.

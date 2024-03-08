# IPv4

## Structuur
Een IPv4-adres bestaat uit 32 bits oftewel 4 bytes.
Meestal wordt het in een decimale notatie opgeschreven, bijvoorbeeld `192.168.0.222`.
Het kan natuurlijk ook anders geschreven worden, bijvoorbeeld binair `11000000.1010000.00000000.11011110`.

Een IP-adres heeft een andere structuur dan een MAC-adres.
Het dient niet om een machine te identificeren, maar wel om hem terug te vinden.
Het verandert dus ook wanneer de machine van netwerk verandert.

|                       | MAC-adres           | IP-adres      |
|-----------------------|---------------------|---------------|
| sterkte associatie    | vast                | veranderlijk  |
| structuur             | vlak                | hiërarchisch  |
| begin afhankelijk van | fabrikant           | subnet        |
| vergelijk met         | rijksregisternummer | thuisadres    |

{% hint style="info" %}
Je zal misschien ergens iets tegenkomen over "klassen" van IP-adressen, zoals "klasse A", "klasse B",...
Dit is een ouder systeem dat voor programmeurs niet relevant is.
{% end hint %}

## Een machine vinden
Volgende zaken hebben een IP-adres:

- hosts
- netwerken zelf
- routers

### Netmaskers
Een IP-adres bestaat uit een "netwerkgedeelte" en een "hostgedeelte".
Voor adressen in binaire notatie kan je een "streep" trekken tussen deze twee gedeeltes.
Deze plaats waarop je deze "streep" trekt wordt bepaald door het **netmasker**.
Bij elk netwerk hoort een netmasker.

Een netmasker heeft een gelijkaardige notatie aan een IP-adres, maar de waarden zijn beperkter.
Een netmasker bestaat in binaire notatie uit een aaneensluitende reeks van 1'tjes gevolgd door een reeks 0'tjes.

Om te controleren of een IP-adres A behoort tot netwerk B, neem je het netmasker van B en doe je een `AND`-operatie met A. Dit kan als volgt:

- noteer A binair
- noteer het netmasker van B daar onder, ook binair
- noteer in elke kolom het resultaat van een `AND`-operatie (als 0 "false" is en 1 "true")

Hier is een voorbeeld voor het IP-adres `192.168.0.222` en het netmasker voor `192.168.0.0/16` (het netwerk met IP-adres `192.168.0.0` en een netmasker met 16 1'tjes):

```
1100000010100000000000011011110 # het IP-adres
1111111111111111000000000000000 # het netmasker van het netwerk
1100000010100000000000000000000 # het resultaat
```

Als we de onderste rij terug decimaal noteren, krijgen we `192.168.0.0`.
We kunnen dus besluiten dat `192.168.0.222` zich inderdaad op het netwerk in kwestie bevindt.

### Routers
Routers zijn als verkeersagenten of richtingsaanwijzers die de weg naar bepaalde **netwerken** kennen.
Vergelijk het met de wegwijzers die je op een kruispunt ziet.
Deze wijzen bijvoorbeeld naar "Brussel" of "Antwerpen" en veronderstellen dat je met dat soort brede informatie alvast in de juiste richting verder kan.
De "richting" waarin je vertrekt is hier een netwerkinterface van de router.
Beeld je gerust in dat er een aantal kabels aan de router verbonden zijn en dat de richting samenhangt met één kabel.

Het kiezen van de juiste richting gebeurt door middel van een **routeringstabel**.
Dit is een tabel, opgeslagen in de router, voor gekende netwerken zegt welke richting gebruikt moet worden.
Op elke rij van de tabel wordt een netwerk (met bijbehorend netmasker) vermeld.
Verder staat er ook een adres van een andere router op hetzelfde netwerk.
Een binnenkomend pakket wordt dan vergeleken door het netmasker op een rij toe te passen op het uiteindelijke adres.
Als het resultaat het netwerk op die rij is, kan de regel worden toegepast.
Als er meerdere regels van toepassing zijn, wordt de meest specifieke (d.w.z. de regel met netmasker met het meeste 1'tjes) toegepast.
Als er geen gewone regels van toepassing zijn, wordt er een default gekozen.


Routers kunnen ofwel manueel geconfigureerd worden, ofwel automatisch via gespecialiseerde protocols die je als programmeur niet hoeft te kennen.

### Binnen het juiste netwerk
Eens een pakket op het juiste subnet is, is routering niet meer nodig.
Dan kan je in principe gewoon aan iedereen op het netwerk "vragen" wat hun IP-adres is en het pakket naar de juiste ontvanger sturen.
Dit "vragen" gebeurt via een protocol genaamd **ARP** ("address resolution protocol") en het zorgt dat het juiste laag 2-adres gebruikt wordt.

![ARP](./images/ARP.png)

## Toekenning van IP-adressen

### Publiek versus privé
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

### Verdeling
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

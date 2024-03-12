# Network Address Translation (NAT) / Port Address Translation (PAT)
De adresruimte van IPv4 gaat zuiver numeriek van 0.0.0.0 tot 255.255.255.255.
Het maximaal aantal hosts op het publieke Internet is dus 2^32 - 2, of iets meer dan 4 miljard.
Door de opdeling in subnetten kan bovendien niet elk adres benut worden.
Er zijn dus een pak minder publieke IPv4-adressen dan mensen.
Veel mensen hebben bovendien meerdere toestellen die aangesloten zijn op het Internet via IPv4.

Dit kan via netwerkadresvertaling (NAT). Poortadresvertaling (PAT) is hier een specifieke vorm van.
Het idee achter "dynamische" PAT is als volgt:
Als gebruiker krijg je een adres ergens in een privébereik, dus 10.0.0.0/8 of 172.16.0.0/12 of 192.168.0.0/16.
Dit adres kan niet (zomaar) **bereikt** worden van op het Internet, maar dat hoeft niet altijd.
Het belangrijke punt is dat je **aansluiting** wel een publiek IP heeft.

Heel vaak start je als client een interactie met een server.
Daarvoor gebruik je bijna altijd TCP of UDP.
Een toestel zoals je thuisrouter "onthoudt" het als je zo'n verbinding opstart.
Hij slaat bijvoorbeeld tijdelijk op dat privé-adres 192.168.1.2 een TCP-verbinding maakt met externe server 87.3.2.2.
Hij onthoudt daarbij ook de gebruikte poorten.
Dan vervangt hij het IP van de afzender en de poort van de zender door zijn eigen IP en een vrije poort.
Wanneer er data terugkomt op die vrije poort, vertaalt hij het weer naar het oorspronkelijke privé-adres en de oorspronkelijke poort.
Hij vervult dus een gelijkaardige rol aan een proxy.

Het eerdere gebruik van port forwarding wordt ook wel "statische PAT" genoemd:
![settings Telenet](./images/portforwarding.png)

De twee kunnen gecombineerd worden.
De "statische" oplossing is vooral nuttig wanneer je niet eerst als client een verbinding opstart, maar zelf bereikbaar moet zijn.
Dat kan bijvoorbeeld zijn wanneer je zelf een multiplayer server host voor een game.
Dit soort *ad hoc* oplossing is af en toe nodig omdat NAT de protocolstapel niet helemaal respecteert.
Het is eigenlijk een "hack", want een taak van een lagere laag (routering) vereist dat we naar een verantwoordelijkheid van een hogere laag (poorten) kijken.
Er is dan ook aparte code nodig in NAT-routers voor elk protocol dat bovenop IP staat.

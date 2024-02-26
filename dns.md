# DNS (Domain Name System)

## Basisidee
In de vroege dagen van het internet, bekend als ARPANET, was er slechts één master copy van een tekstbestand dat alle IP-adressen en namen van aangesloten computers bevatte. Gebruikers moesten alleen het IP-adres onthouden van de machine die dit bestand hostte om toegang te krijgen tot andere computers op het netwerk. Echter, deze methode was duidelijk niet robuust of schaalbaar. Naarmate het aantal computers op het netwerk toenam, werd het onhoudbaar om op deze manier de netwerkadressen te beheren. Om deze uitdaging aan te pakken en het systeem robuuster en meer gedistribueerd te maken, werd het Domain Name System (DNS) ontwikkeld. DNS transformeerde de manier waarop adressen op het internet worden beheerd door een gedecentraliseerd en hiërarchisch systeem te introduceren dat de vertaling van domeinnamen naar IP-adressen vergemakkelijkt, waardoor gebruikers gemakkelijk toegang hebben tot internetbronnen zonder de noodzaak om complexe IP-adressen te onthouden. Het DNS-systeem gebruikt het DNS-protocol. Dat bevindt zich in het TCP/IP-model op de applicatielaag.

Het idee achter DNS is dat de verantwoordelijkheid over domeinextensies verdeeld wordt:

![opsplitsing domeinextensies](./images/dns-root-server.png)

Een iets meer technisch perspectief krijg je op volgende afbeelding:

![verdeling verantwoordelijkheid in DNS](./images/Domain_name_space.svg)

Hier staat bovenaan maar één "root", maar eigenlijk zijn er momenteel 13 IP-adressen voor root name servers voorzien.
Per IP-adres zijn er bovendien nog een aantal kopieën, dus er zijn eigenlijk een paar honderd "root" name servers.

Volgend filmpje licht het mechanisme toe: [filmpje](https://www.youtube.com/embed/27r4Bzuj5NQ?si=dgP3K7T8dTUtqYqV)

## Werking
Een eindgebruiker stelt in hun netwerkinstellingen een DNS-resolver in, die verschilt van een name server. Het IP-adres van deze resolver wordt teruggevonden in je netwerkinstellingen, en is normaal gesproken automatisch toegewezen wanneer je een IP-adres krijgt van je netwerkaanbieder. Echter, gebruikers hebben de mogelijkheid om dit manueel aan te passen, bijvoorbeeld door de DNS-instellingen te wijzigen naar die van Google DNS in plaats van de standaard DNS-server die door hun Internet Service Provider (ISP) wordt aangeboden. Het is belangrijk om te noteren dat het IP-adres van de resolver wordt gebruikt omdat je het nodig hebt voor opzoekingen; het kan daarom niet simpelweg als een naam worden bijgehouden. Bovendien luistert de resolver standaard op poort 53, wat eveneens geldt voor name servers, hoewel ze een andere verantwoordelijkheid dragen binnen het DNS-ecosysteem.

Een name server houdt allerlei types DNS-records bij. Voor applicatieprogrammeurs zijn de belangrijkste:

- A record
- AAAA record
- CNAME record
- MX record
- NS record

Details van deze recordtypes vind je op volgende pagina: [details](https://www.cloudflare.com/learning/dns/dns-records/)

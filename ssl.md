# HTTPS / TLS / SSL
De plaats van HTTP in de protocolstack was als volgt:

![HTTP in de protocolstack](./images/http_in_stack.svg)


"Tussen" de transportlaag en de applicatielaag zit hier dus nog een (optionele) extra "beveiligingslaag".
Eigenlijk past het algemene TCP/IP-model hier niet zo goed op, maar meestal wordt TLS / SSL daar gesitueerd.

HTTPS is HTTP bovenop die extra laag in plaats van rechtstreeks bovenop het transportlaagprotocol (TCP of QUIC).
In principe kunnen ook andere protocols op dezelfde manier beveiligd worden (bijvoorbeeld WSS voor "web sockets secure").

Dit is een goede zaak, want HTTP wordt op de kabel voorgesteld als de bytes voor volgende ASCII-tekst:

```
Versie spatie Code spatie Phrase\r\n
Header veldnaam:waarde\r\n
Header veldnaam:waarde\r\n
Header veldnaam:waarde\r\n
...
Header veldnaam:waarde\r\n
\r\n
Data Data Data Data Data ...
```

Met andere woorden: bij gewone HTTP zijn de headers en data zomaar leesbaar op alle tussenstops van client naar server.

SSL en TLS zijn twee protocols die **authenticatie** en **encryptie** voorzien.
Anders gezegd: ze zorgen dat de client zeker weet met wie hij communiceert (authenticatie) en dat de verstuurde data niet leesbaar is op tussenstops (encryptie).

Het eerste puntje is misschien niet meteen duidelijk.
Volgende figuur illustreert een risico:

![man in the middle attack](./images/mitm.png)

Bij een "man in the middle" onderschept iemand berichten tussen client en server.
Deze onderschepper overtuigt de client dat hij de server is en kan zo bijvoorbeeld gevoelige informatie zoals loginwachtwoorden ontfutselen.

SSL en TLS voorkomen dit dus.
In de praktijk wordt SSL niet meer gebruikt en is TLS de opvolger, maar in het IT-taalgebruik wordt vaak "SSL" gezegd voor heel deze familie van protocols.

## Werking
TLS vereist dat de client de server eerst contacteert om een "handshake" uit te voeren.
Dit houdt in dat de server een **certificaat** voorlegt.
Dit is een bewijs, ondertekend door een **certificate authority** dat de voorlegger op een of andere manier betrouwbaar is.

Dit bewijs kan niet zomaar gekopieerd worden, want het bevat een **public key**.
Zie dit als een "open kluis" waarmee je informatie kan encrypteren, maar niet decrypteren.
Enkel de eigenaar van het bewijs heeft een **private key** die compatibel is.

Met andere woorden, via het certificaat kan de client een bericht encrypteren en zorgeloos terugsturen naar de server.
Omgekeerd gaat op dit punt nog niet.

Het bericht dat de client naar de server terugstuurt wordt gebruikt (via een nogal complexe wiskundige procedure) om een **session key** te bepalen,
een geheim dat zowel de client als de server kennen. Deze session key wordt uiteindelijk voor de duur van het "gesprek" gebruikt om verkeer in **beide** richtingen te encrypteren.

[Dit filmpje](https://youtu.be/j9QmMEWmcfo) licht de procedure uitvoerig toe. De belangrijkste stappen staan op onderstaande figuur:

![Screenshot uit het filmpje](./images/TCP-handshake.png)

Je hoeft de stappen in het filmpje niet allemaal uit het hoofd te leren, maar je moet wel de informatie in deze tekst onthouden.

## Certificaten
Een **certificaat** (of "SSL-certificaat" of "TLS-certificaat") is een digitaal bewijs dat uitgegeven wordt door een betrouwbare derde partij.
Dit bewijs geeft (minimum) aan dat diegene die het certificaat voorlegt eigenaar is van het domein dat vermeld wordt in het certificaat.
Als je bijvoorbeeld naar `www.ap.be` surft, ontvang je een certificaat voor dat domein.
Dat zou ondertekend kunnen zijn door bijvoorbeeld Microsoft.
Dan weet je dat Microsoft je garandeert dat de site die je op dat moment te zien krijgt ook echt geregistreerd staat onder `www.ap.be`.
Tenzij Microsoft zich vergist, is een "man in the middle" dus niet mogelijk.

Je besturingssysteem (of browser) gaat er vanzelf van uit dat informatie die ondertekend is door Microsoft / Apple / Google / ... (kies maar) betrouwbaar is.
Het systeem van certificaten veronderstelt dat er altijd zo'n **root certificate authority** is, iemand die betrouwbare certificaten aflevert.
Er bestaan ook lagere **certificate authorities** (of **CA's**), dus ondernemingen die certificaten mogen uitgeven maar niet automatisch door je besturingssysteem / browser betrouwd worden.
Zij kunnen bewijzen dat ze betrouwbaar zijn door een certificaat voor te leggen dat ze zelf bij een grotere provider hebben aangeschaft.

Certificaten bestaan in verschillende types:

1. Domain Validated (DV) Certificaten: Dit zijn de eenvoudigste vorm van TLS-certificaten. Ze worden uitgegeven nadat de beheerder van een domein heeft aangetoond dat hij de controle heeft over dat domein, meestal via een e-mailverificatie of door een DNS-record toe te voegen. DV-certificaten controleren niet de identiteit van de organisatie achter het domein en worden vaak gebruikt voor kleinere websites of blogs.
2. Organization Validated (OV) Certificaten: OV-certificaten vereisen een uitgebreider validatieproces dan DV-certificaten. De certificaatautoriteit (CA) controleert de identiteit van de aanvrager en bevestigt dat de organisatie legitiem is. OV-certificaten geven meer vertrouwen aan gebruikers, omdat ze laten zien dat de organisatie achter de website is geverifieerd.
3. Extended Validation (EV) Certificaten: EV-certificaten bieden het hoogste niveau van validatie en vertrouwen. De validatieprocedure is zeer grondig en omvat vaak persoonlijke identificatie van de aanvrager en controle van juridische documenten van de organisatie. Websites met een EV-certificaat worden weergegeven met een groene adresbalk in de browser, wat aangeeft dat de site zeer veilig en betrouwbaar is. EV-certificaten worden vaak gebruikt door grote bedrijven, financiële instellingen en e-commerceplatforms.

Naast deze types zijn er ook speciale certificaten, zoals wildcard-certificaten die geldig zijn voor alle subdomeinen van een bepaald domein, en multi-domein certificaten die meerdere domeinen en subdomeinen kunnen beveiligen met één certificaat.

In het algemeen geldt dat "domain validated" certificaten het goedkoopst zijn.
Ze kunnen ook automatisch gegenereerd worden (bijvoorbeeld via Letsencrypt) zonder menselijke tussenkomst, want ze controleren eigenlijk alleen een database met geregistreerde URL's (het zogenaamde "DNS"-systeem).
Ze kunnen met andere woorden garanderen dat de website die je ziet bijvoorbeeld echt wel `www.ap.be` is, maar niet dat `www.ap.be` ook toebehoort aan de organisatie AP.
De andere types vereisen menselijke tussenkomst en kosten dus een pak meer.

Als een website een certificaat kan voorleggen, zie je een slotje in de browserbalk.
Als dat niet gaat, zie je vaak een waarschuwing.
Eén type om voor op te letten is een **self-signed** certificate.
Dit is een certificaat dat uiteindelijk niet teruggaat tot een root CA.
Iemand heeft het met andere woorden ondertekend, maar er is geen garantie dat die partij betrouwbaar is.
Het kan soms nuttig zijn om mee te testen, maar eigenlijk is het waardeloos.

### Een certificaat aanvragen
Een Certificate Signing Request (CSR) is een stukje gegevens dat wordt gegenereerd door een entiteit die een SSL/TLS-certificaat wil verkrijgen van een CA.
Dit CSR bevat informatie zoals de openbare sleutel van het certificaat, samen met identificatiegegevens zoals de organisatienaam, domeinnaam en andere relevante details.
Het is in essentie een gestandaardiseerd formulier voor wie een certificaat wil aanvragen.

Hierbij komen volgende stappen kijken:

1. De entiteit genereert een CSR met behulp van een hulpprogramma voor SSL-certificaten op hun server. Dit kan bijvoorbeeld gebeuren met behulp van OpenSSL.
2. Tijdens het genereren van de CSR moet de entiteit verschillende details verstrekken, zoals de organisatienaam (of naam van de aanvrager), het domein waarvoor het certificaat wordt aangevraagd, de locatie, enzovoort.
3. Nadat de CSR is gegenereerd, wordt deze naar de CA gestuurd, samen met een verzoek om een SSL/TLS-certificaat voor de opgegeven domeinnaam te ondertekenen.
4. De CA verifieert de ingediende CSR en de identiteit van de aanvrager. Het kan bijvoorbeeld dat de aanvrager een e-mail ontvangt op het domein waarvoor het certificaat wordt aangevraagd en moet reageren om het domeinbezit te verifiëren.
5. Na succesvolle verificatie ondertekent de CA het certificaat met zijn eigen privésleutel, waardoor het SSL/TLS-certificaat wordt gegenereerd.
6. De CA stuurt het ondertekende certificaat terug naar de entiteit.
7. De entiteit installeert het ontvangen SSL/TLS-certificaat op zijn server, waardoor de website of service wordt beveiligd met SSL/TLS-encryptie.

# Routering
Volgende zaken hebben een IP-adres:

- hosts
- netwerken zelf
- routers

## Netmaskers
Een IP-adres bestaat uit een "netwerkgedeelte" en een "hostgedeelte".
Voor adressen in binaire notatie kan je een "streep" trekken tussen deze twee gedeeltes.
Deze plaats waarop je deze "streep" trekt wordt bepaald door het **netmasker**.
Bij elk netwerk hoort een stukje informatie dat we het "netmasker" noemen.

{% hint style="info" %}
Als je Windows gebruikt, run dan eens het commando `ipconfig` in een terminal. Normaal zie je minstens één vermelding van een netmasker (je hebt er waarschijnlijk meerdere omdat je typisch om meerdere al dan niet virtuele netwerken tegelijk bent aangesloten).
{% endhint %}

Een netmasker heeft een gelijkaardige notatie aan een IP-adres, maar de waarden zijn beperkter.
Een netmasker bestaat in binaire notatie uit een aaneensluitende reeks van 1'tjes gevolgd door een reeks 0'tjes. In het decimaal is het dus ook een reeks van vier getallen tussen 0 en 255, maar niet elk getal daar tussenin is mogelijk. Enkel de waarden die je kan schrijven als een doorlopende reeks 1'tjes gevolgd door 0'tjes (bijvoorbeeld **wel** 255 of 240 of 128 maar **niet** 243 of 64). Als je niet ziet waarom deze getallen wel/niet toegelaten zijn, noteer ze dan voor jezelf in het binair.

Om te controleren of een IP-adres A behoort tot netwerk B, neem je het netmasker van B en doe je een `AND`-operatie met A. Dit kan als volgt:

- noteer A binair
- noteer het netmasker van B daar onder, ook binair
- noteer in elke kolom het resultaat van een `AND`-operatie (als 0 "false" is en 1 "true")

Hier is een voorbeeld voor het IP-adres `192.168.0.222` en het netmasker voor `192.168.0.0/16` (het netwerk met IP-adres `192.168.0.0` en een netmasker met 16 1'tjes):

```
11000000 10101000 0000000 11011110 # het IP-adres
11111111 11111111 0000000 00000000 # het netmasker van het netwerk
11000000 10101000 0000000 00000000 # het resultaat
```

Het "resultaat" hier is een netwerk waarop het IP-adres ligt.
Als je voor de tweede regel het netmasker gebruikt dat je van `ipconfig` (of `ifconfig` op Mac/Linux) krijgt, dan krijg je het adres van je lokaal netwerk.
Met andere woorden, het kleinste netwerk waarop je bent aangesloten.

Aangezien het resultaat in het voorbeeld exact het netwerk is waarop we zochten (want `1100000010100000000000000000000` als vier decimale cijers is `192.168.0.0`), ligt het IP-adres inderdaad op het netwerk in kwestie.

Merk dus op: een netwerk heeft ook een IP-adres en een netmasker. Als je dat netmasker op dat IP-adres toepast, zal het resultaat niet veranderen. Bijvoorbeeld:

```
11000000 10101000 00000000 0000000 # het IP-adres van een netwerk, 192.168.0.0
11111111 11111111 00000000 0000000 # het netmasker van het netwerk, /16
11000000 10101000 00000000 0000000 # het resultaat is terug 192.168.0.0
```

**Dit is ook de manier om te checken of een IP-adres een hostadres of een adres van een netwerk is: controleer of er iets verandert wanneer je het netmasker toepast.**

### Extra voorbeelden

#### Ligt `192.169.0.222` ook op het netwerk `192.168.0.0/16`?

**Antwoord niet meteen "ja" of "nee" omdat de eerste cijfers verschillen!**
Hoewel er truukjes zijn waarmee je snel kan beslissen, kan je het niet in alle gevallen makkelijk zeggen door naar de decimale notatie te kijken.

```
11000000 10101001 0000000 11011110 # het IP-adres
11111111 11111111 0000000 00000000 # het netmasker van het netwerk
11000000 10101001 0000000 00000000 # het resultaat
```

We zetten het resultaat terug om en krijgen `192.169.0.0`. Dat is niet hetzelfde als `192.168.0.0`. Het adres ligt **niet** op het netwerk `192.168.0.0/16`.

{% hint style="danger" %}
Het lijkt misschien alsof je gewoon moet kijken of de eerste getallen overeenkomen. Dat is **niet** waar. In dit geval mag je **wel** naar de eerste twee getallen kijken, maar dat komt omdat het netmasker 16 1'tjes telt. 16 is 2 * 8 en elk groepje van 8 bits komt overeen met één decimaal genoteerd getal. Dus bij een `/8` moet je enkel kijken naar het eerste decimaal genoteerde getal, bij `/16` naar de eerste twee en bij `/24` naar de eerste drie. Maar er bestaan ook netmaskers zoals `/12`, `/21`, enzovoort. Dan **moet** je met de binaire voorstelling werken.
{% endhint %}

#### Ligt `192.168.0.222` ook op het netwerk `192.0.0.0/8`?

```
11000000 10101000 0000000 11011110 # het IP-adres
11111111 00000000 0000000 00000000 # het netmasker van het netwerk
11000000 00000000 0000000 00000000 # het resultaat
```

Hier komen we 192.0.0.0 uit als resultaat. Dus het adres ligt inderdaad op netwerk 192.0.0.0/8.

{% hint style="warning" %}
Dit is geen probleem! 192.168.0.222 ligt op **zowel** 192.168.0.0/16 als 192.0.0.0/8, omdat 192.168.0.0/16 zelf een **deelnetwerk** of **subnet** is van 192.0.0.0/8. Vergelijk: "Ellermanstraat 33 ligt in Antwerpen, Antwerpen ligt in België, Ellermanstraat 33 ligt ook in België".
{% endhint %}

#### Is `192.168.0.0/16` een deelnetwerk van `192.0.0.0/8`?

De procedure is hier dezelfde. Alleen nemen we voor de eerste regel geen hostadres, maar het adres van een netwerk. Meerbepaald het **kleinste** netwerk van de twee, dat wil zeggen het netwerk met het **langste** netmasker.

```
11000000 10101000 0000000 00000000 # het IP-adres van het netwerk, dus 192.168.0.0
11111111 00000000 0000000 00000000 # het netmasker van het grootste netwerk, dus het kortste netmasker
11000000 00000000 0000000 00000000 # het resultaat
```

De uitkomst is 192.0.0.0. Dit is terug het adres van het grootste netwerk. Dus het kleinere netwerk ligt inderdaad in het grotere netwerk.

#### Is `192.0.192.0/20` een deelnetwerk van `192.0.128.0/17`?

Hier merk je dat de netmaskers een lengte hebben die geen veelvoud is van 8. Dus probeer niet te vertrekken van de ecimale notatie.

```
11000000 00000000 11000000 0000000 # het IP-adres van het kleinere netwerk, dus 192.168.192.0, zonder eigen netmasker
11111111 11111111 10000000 0000000 # het netmasker van het grootste netwerk, dus het kortste netmasker, 17 1'tjes
11000000 00000000 10000000 0000000 # het resultaat
```

De uitkomst is 192.0.128.0. Dit is het grote netwerk, dus het kleinere netwerk ligt inderdaad op het grotere netwerk.

#### Is `192.0.64.0/20` een deelnetwerk van `192.0.128.0/17`?

```
11000000 00000000 01000000 0000000
11111111 11111111 10000000 0000000
11000000 00000000 00000000 0000000
```

De uitkomst is 192.0.0.0. Dit is iets anders dan 192.0.128.0. Dus het kleinere netwerk ligt **niet** op het grotere netwerk.


## Routers
Routers zijn als verkeersagenten of richtingsaanwijzers die de weg naar bepaalde **netwerken** kennen.
Vergelijk het met de wegwijzers die je op een kruispunt ziet.
Deze wijzen bijvoorbeeld naar "Brussel" of "Antwerpen" en veronderstellen dat je met dat soort brede informatie alvast in de juiste richting verder kan.
De "richting" waarin je vertrekt is hier een netwerkinterface van de router.
Met andere woorden: de aansluiting op een specifiek netwerk.
Beeld je gerust in dat er een aantal kabels aan de router verbonden zijn en dat de richting samenhangt met één kabel.

De eerste router waar je een pakket naartoe stuurt, noemen we de "default gateway".
Deze kan je terug vinden onder je netwerkinstellingen en het zal altijd een IP-adres zijn op **je eigen netwerk**.
Omdat hij op je eigen netwerk zit, hoef je de weg niet eerst te zoeken.
Anders werd dit een "het kip of het ei"-probleem.
Vergelijk de default gateway met je plaatselijke rode brievenbus.
Je levert daar je post af en iemand anders brengt hem wel naar de bestemming.

Eens een pakket bij een router is, wordt de volgende stap bepaald door middel van een **routeringstabel**.
Dit is een tabel, opgeslagen in de router.
Voor gekende netwerken zegt ze welke richting (netwerkinterface) gebruikt moet worden.
Op elke rij van de tabel wordt een netwerk (met bijbehorend netmasker) vermeld.
Verder staat er ook een adres van een andere router **op hetzelfde netwerk**.

Een binnenkomend pakket wordt dan vergeleken door het netmasker op een rij toe te passen op het uiteindelijke adres.
Als het resultaat het netwerk op die rij is, kan de regel worden toegepast.
Als er meerdere regels van toepassing zijn, wordt de meest specifieke (d.w.z. de regel met netmasker met het meeste 1'tjes) toegepast.

Vergelijk dit met een sorteercentrum voor post.
Als je een brief voor "Ellermanstraat 33, 2000 Antwerpen, België" deponeert in een brievenbus in bijvoorbeeld India, zal men vooral kijken naar de laatste adresregel en de brief gewoon in de richting van België sturen.
Dat brengt de brief in elk geval dichter bij de bestemming.
Als je brief zich in een sorteercentrum in Antwerpen bevindt, zal men wel naar de eerste regel kijken en meegeven aan een postbode die exact op dat adres passeert.

{% hint style="info" %}
Als er geen gewone regels van toepassing zijn, wordt er een default gekozen.
{% endhint %}

Onderstaande opstelling toont dit in Packet Tracer.
De getoonde routeringstabel is die voor de centrale router.
De gemarkeerde IP-adressen zijn IP-adressen van andere routers aangesloten op hetzelfde lokale netwerk.

![routeringstabel van de centrale router](./images/statische-routering.png)

Routers kunnen ofwel manueel geconfigureerd worden, ofwel automatisch via gespecialiseerde protocols die je als programmeur niet hoeft te kennen.

## Binnen het juiste netwerk
Eens een pakket op het juiste subnet is, is routering niet meer nodig.
Dan kan je in principe gewoon aan iedereen op het netwerk "vragen" wat hun IP-adres is en het pakket naar de juiste ontvanger sturen.
Dit "vragen" gebeurt via een protocol genaamd **ARP** ("address resolution protocol") en het zorgt dat het juiste laag 2-adres gebruikt wordt.

![ARP](./images/ARP.png)


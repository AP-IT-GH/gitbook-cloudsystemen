# IPv6
IPv6 vertegenwoordigt een langdurige oplossing zonder de beperkingen van Network Address Translation (NAT) door een significante uitbreiding van de adresruimte.
Terwijl het oude IPv4-systeem werkte met een adresruimte van 2^32, schaalt IPv6 dit op naar een bijna onvoorstelbare 2^128.
Beeld je ongeveer in dat we voor elke machine die bereikbaar is op het Internet een nieuw Internet voorzien.
Kwadrateer dat aantal hosts en dan zit je rond het aantal adressen in IPv6.

Wat betreft de adoptie en implementatie van IPv6: deze vordert gestaag maar langzaam en varieert aanzienlijk per regio.
Het is essentieel om bewust te zijn van het bestaan van IPv6, te begrijpen dat het ontworpen is om de beperkingen van NAT te omzeilen, en te realiseren dat het niet eenvoudigweg een uitbreiding van IPv4 met meer bits is.
IPv6 kan parallel met IPv4 gebruikt worden.
Gebruikers hoeven dus niet "over te stappen".
Ze kunnen IPv6 implementeren zonder IPv4 op te geven.

Om deze reden blijft IPv4 voorlopig het referentiepunt.
Cloud providers zoals AWS staan ook toe eigen subnetten,... te configureren met IPv4, dus een diepgaande kennis van IPv6 is voor programmeurs voorlopig niet nodig.
Je moet echter wel weten:

- hoe je een IPv6-adres uitschrijft
- hoe je er naartoe navigeert
- dat het netwerkgedeelte 64 van de 128 bits inneemt
- dat IPv6-adressen goedkoper zijn dan IPv4-adressen omwille van vraag en aanbod
- dat je standaard meerdere IPv6-adressen hebt

Wat je voorlopig niet hoeft te kennen:

- details rond routering
- aanmaken van subnetten
- gebruik in PacketTracer

## Notatie
De 128 bits van een IPv6-adres worden voluit hexadecimaal genoteerd in groepjes van 2 bytes, gescheiden door `:`.
De keuze van hexadecimaal komt omdat IPv6-adressen erg lang zijn.
Zelfs met dit talstelsel zijn IPv6-adressen soms lastig te noteren, dus je kan een aantal regels toepassen:

- groepjes `0000` kan je afkorten tot `0`
- de **langste** reeks aaneengesloten `0`-groepjes mag je er tussenuit laten, zodat er staat `::`
- in de browserbalk moet je een IPv6-adres tussen rechte haken zetten, bijvoorbeeld `http://[2a00:1450:4001:82a::2004]`
- de notatie met `/` wordt gebruikt om aan te geven welk eerste deel van een adres vast ligt in een bepaalde context, bijvoorbeeld `2000::/3` wijst op "alle adressen die met dezelfde 3 bits beginnen als het adres `2000::`".

## Speciale adressen
- `::1/128` (localhost)
- `2000::/3` (publiek Internet)
- `fc00::/7` (binnen zelfde internetaansluiting)
- `fe80::/10` (geen routering vereist)

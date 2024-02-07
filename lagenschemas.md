Netwerken kunnen gigantisch zijn en er moet rekening gehouden worden met enorm veel aspecten (performantie, security, robuustheid,...). Het spreekt dan ook voor zich dat er heel veel verschillende taken moeten worden uitgevoerd om een "goed" packet switchingsysteem op te bouwen. Voeg daar aan toe dat er heel veel verschillende apparaten op een netwerk aanwezig kunnen zijn, mogelijk zelfs van verschillende fabrikanten. Het spreekt helemaal niet voor zich dat dit nog overzichtelijk zal zijn.

De standaardmanier om een overzicht te bewaren is het gebruik van een "lagenmodel". Hierbij wordt gekeken naar verschillende niveaus tussen de gebruikersapplicatie en de "bits op de kabel". Op elk niveau wordt een bepaalde taak afgehandeld. Hoe hoger het niveau, hoe meer abstractie.

![./images/filosofen.png](3-lagenmodel voor communicatie tussen filosofen \(eventueel laag 0: fax\) \(overgenomen uit Tanenbaum, Computer Networks, 5e editie\))

In dit soort model wordt op elke laag een bepaald protocol toegepast. Hierbij kan gerekend worden op ondersteuning van de onderliggende laag: langs de linkerkant om de boodschap "te versturen", langs de rechterkant om de boodschap te "ontvangen".

Het protocol op een laag kan variëren. In de figuur spreken twee filosofen met elkaar via twee tolken die Nederlands spreken en communiceren ze via fax. Het zouden net zo goed juristen kunnen zijn die hun teksten naar het Frans laten vertalen en dan via e-mail laten doorgeven. Dan heeft het model dezelfde lagen, maar worden andere protocols gevolgd.

Bij het zenden wordt op één laag alleen de volgende laag gekozen. Bij het ontvangen wordt alleen naar de lagere laag gekeken, in principe niet naar de data bedoeld voor de hogere laag.

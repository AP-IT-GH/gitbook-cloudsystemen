# Wat is "serverless" computing?
De term "serverless computing" verwijst (verwarrend genoeg) naar een specifiek client-servermodel. In dit model schrijven we functies die uitgevoerd worden op een server, maar houden we ons helemaal niet bezig met de configuratie van deze server. De term is dus misleidend: er is wel een server betrokken, maar deze is zo goed als onzichtbaar.

Dit is wat abstract, dus vergelijk het met een klassieke client-server applicatie. Wanneer we daar iets willen laten uitvoeren door een server, schrijven we typisch een API. Dit doen we (al dan niet rechtstreeks) met behulp van een web framework zoals Express, Flask,... Voor elke functie die we willen aanbieden, schrijven we een handler. Dan zoeken we een hosting service die dat framework ondersteunt of huren we een VPS waarop we alle dependencies van het framework installeren.

In "serverless" computing is er geen (zichtbaar) framework. We kiezen een template met ondersteunde omgevingen (bijvoorbeeld NodeJS), schrijven een op zichzelf staande functie en deployen die ene functie op zo'n manier dat ze via HTTP kan worden opgeroepen. We gebruiken hiervoor een gespecialiseerd platform zoals AWS Lambda, Azure Functions, Google Cloud Functions of OpenFaaS ("functions as a service").

Serverless functions moeten "stateless" zijn, zoals ook het HTTP-protocol dat is. Ook hier is het wel mogelijk data op te slaan door middel van extra mechanismen als databanken,... maar functies mogen bijvoorbeeld geen variabelen aanpassen in een omringende scope.

Achter de schermen wordt onze functie wel verpakt in een web framework, dat uitvoert in een container, maar daar hoeven we niet naar om te kijken.

## Voor- en nadelen
Serverless functies hebben een heel ander model voor kosten en schaalbaarheid dan klassieke web frameworks.

### Voordelen
- ze zijn eindeloos schaalbaar (je provider kan automatisch meer containers opstarten als de functie meer gebruikt wordt)
- er is eigenlijk geen onderhoud vereist (het onderliggend platform is verborgen, de provider zorgt dat dat up-to-date is)
- je betaalt alleen voor de oproepen die plaatsvinden, niet voor ongebruikte capaciteit ("pay-as-you-go")

### Nadelen
- omdat er niet altijd een container actief is, moet er soms een nieuwe gestart worden en dat kan een seconde of twee duren
- je moet je code misschien anders schrijven (ze moet bijvoorbeeld stateless zijn en mag geen platformspecifieke truukjes gebruiken)
- pay-as-you-go kan duurder zijn, soms is het beter een specifieke capaciteit te huren

Serverless functies zijn niet "de nieuwe manier om alles te doen", maar ze spelen wel een grote rol in cloud applicaties en zijn voor sommige toepassingen erg handig.

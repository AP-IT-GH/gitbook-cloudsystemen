# traceroute
Het programma `traceroute` (op sommige systemen "tracert") is een tool die wordt gebruikt om het pad dat pakketten volgen van een bron naar een doelhost te achterhalen en te visualiseren.

Traceroute biedt inzicht in elk punt (hop) langs het pad en helpt bij het identificeren van de locatie van vertragingen of verstoringen in het netwerk. Je kan traceroute zien als een reeks opeenvolgende pings, waarbij steeds één stapje verder gezet wordt. Dit gebeurt door elk pakket een "time-to-live" ("resterende levensduur") te geven. Per tussenstop wordt deze levensduur verminderd. Wanneer het pakket gedropt wordt, wordt de afzender verwittigd waar het verloren gegaan is.

![werking traceroute]("./images/werking traceroute.png")

![werking traceroute]("./images/uitvoer traceroute.png")

## belangrijke punten bij de interpretatie

- Aantal hops: Geeft een indicatie van de afstand en de complexiteit van het pad naar de bestemming.
- Reactietijden: Hoge reactietijden bij een bepaalde hop kunnen wijzen op vertragingen bij dat netwerkapparaat.
- Consistentie van reactietijden: Variabiliteit in reactietijden kan wijzen op netwerkinstabiliteit of wisselende netwerkbelasting.
- Sterren of ontbrekende reacties: Deze kunnen duiden op een hop die geen ICMP-berichten genereert of een netwerk dat ICMP-berichten blokkeert.

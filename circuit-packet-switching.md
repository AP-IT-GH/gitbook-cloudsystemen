# Circuit en packet switching

## Circuit switching
Circuit switching is een type netwerkcommunicatie waarbij een specifiek fysiek pad wordt opgezet en vastgehouden voor de gehele duur van de communicatiesessie tussen twee of meer nodes. Dit pad wordt exclusief aan het gesprek toegewezen, ongeacht of er op een bepaald moment gegevens worden verzonden of niet. Dit is zoals een klassieke telefoonverbinding.

Circuit switching maakt het mogelijk de kwaliteit van een verbinding te garanderen (zo lang de hardware niet stuk gaat). Het voornaamste nadeel is inefficiëntie: Wanneer je iemand belt, hou je de lijn bezet. Dat is ook zo wanneer je niets aan het zeggen bent.

## Packet switching
Packet switching is een netwerkcommunicatietechniek waarbij de te verzenden gegevens worden opgedeeld in pakketten. Elk pakket wordt onafhankelijk verzonden en kan verschillende routes door het netwerk nemen om bij de bestemming te komen. Aan de ontvangende kant worden de pakketten opnieuw samengevoegd in de oorspronkelijke volgorde. Dit kan je vergelijken met een groep mensen die in verschillende auto's naar dezelfde bestemming vertrekt.

## Afweging
Packet switching is algemeen gesproken efficiënter dan circuit switching. Wanneer er data is en het kanaal nog plaats heeft, kan die data meteen doorgestuurd worden. Het is wel complexer: data die verspreid is over meerdere pakketten kan herordend worden, (gedeeltelijk) verloren gaan, niet alle data volgt noodzakelijk dezelfde weg, naar de eindbestemming.

**Globaal gesproken gebruikt het Internet packet switching.** Zelfs toepassingen waarbij sprake is van een "verbinding" (zoals internetbellen of video chat) gebruiken packet switching. Ook als je een inbelverbinding gebruikt, gebruik je packet switching.

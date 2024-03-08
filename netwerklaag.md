# De netwerklaag
De primaire functie van de netwerklaag is **routering**.
Dit is een ander woord voor "de weg bepalen".
In een **lokaal netwerk**, dus wanneer er geen routers gepasseerd moeten worden, is dit eenvoudig.
Lokale netwerken zijn zodanig klein dat het mogelijk is alle adressen van iedereen op het netwerk bij te houden of om berichten naar iedereen op het netwerk te sturen.
Op een internet (met kleine "i") is dit lastiger.
Wanneer het aantal hosts potentieel erg groot is, is er een efficiënt systeem nodig om de weg te vinden.
Het is niet meer realistisch om te onthouden waar elke gebruiker zich bevindt, of om broadcasts over het hele internet te doen om één specifieke ontvanger te vinden.

Anders gesteld: de taak van de netwerklaag is om data van machine A naar machine B te krijgen, ongeacht de grootte van het gehele netwerk.
De essentiële protocols hiervoor zijn IPv4, IPv6 en (minder zichtbaar) ARP.

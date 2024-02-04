# Wireshark
Wireshark is een grafische "packet sniffer". Een packet sniffer geeft pakketten weer die op een netwerkinterface verzonden en ontvangen worden.

Op het eerste gezicht kan Wireshark overkomen als een tool voor hackers, maar Wireshark doet niet aan bijkomende decryptie,... Het maakt het gewoon mogelijk in detail te kijken naar de data die passeert. Als daar gevoelige informatie tussen zit, wijst dit op een onveilige applicatie of een onveilige systeemconfiguratie.

## Installatie
Je kan Wireshark downloaden op [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html). Gebruik de standaard installatie-opties. Je kan achteraf nog altijd aanpassingen doen.

**Run Wireshark niet als admin. Dit is een risico voor je eigen veiligheid.** Als je de foutmelding "NPF driver is not running" krijgt, run je **`cmd`** als admin. In `cmd` typ je het commando: `net start npf`. Daarna zou je Wireshark moeten kunnen uitvoeren om pakketten af te luisteren.

## Gebruik
[Video van collega V. Willemen](https://ap.cloud.panopto.eu/Panopto/Pages/Viewer.aspx?id=79b9cea0-df9c-4aab-bf46-aac700dc2209)

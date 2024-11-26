# Reverse proxying
Een "proxy" is een plaatsvervangende dienst. In de context van netwerken is een "forward proxy" een dienst die uitgaand verkeer (van client naar server) onderschept, inspecteert en eventueel aanpast. Dit kan nuttig zijn om bandbreedte te besparen, verkeer te monitoren,... Een "reverse proxy" onderschept **inkomend** verkeer en zit dus aan de kant van de server.

Zo'n reverse proxy staat toe verschillende taken op een algemeen niveau (in plaats van per applicatie) af te handelen, waaronder:

- Load balancing, d.w.z. dat dezelfde dienst eigenlijk door meerdere machines wordt aangeboden. In dat geval is de reverse proxy een centraal aanspreekpunt. Vergelijk met hoe je soms meteen een nummertje krijgt om daaarna naar één van de loketten te gaan.
- SSL/TLS-terminatie: Als je SSL/TLS gebruikt, doe je dat waarschijnlijk meteen voor al je diensten. Veel applicaties implementeren hier geen rechtstreeks ondersteuning voor. Web servers ondersteunen bijvoorbeeld HTTP, maar niet noodzakelijk HTTPS. De reverse proxy kan dan eerst de beveiliging afhandelen, zodat de web server gewone HTTP kan verwerken.
- Extra beveiliging: als niet elke dienst rechtstreeks bereikbaar is, maar enkel via de reverse proxy, wordt de *attack surface* kleiner.
- Caching (voor herhaalde requests).

Er bestaan verschillende reverse proxy's en verschillende manieren om deze te integreren. Wij zullen hier gebruik maken van Traefik, een heel populaire en heel flexibele. We zullen Traefik ook steeds runnen **in** Docker Compose, samen met onze applicaties, maar dat is in het algemeen geen vereiste.

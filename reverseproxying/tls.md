# Wat en waarom?

Een van de interessantste features van Traefik is hoe makkelijk deze reverse proxy het maakt om HTTPS te implementeren. Hiervoor moet een server in principe beschikken over een private key en een TLS-certificaat. Dat laatste wordt uitgegeven door een vertrouwde derde partij en is tijdelijk geldig.

Vroeger moest je zo'n certificaat regelmatig handmatig vernieuwen. Voor veel websites was dat het niet waard. Nu is dat veel gemakkelijker en is er geen enkel excuus om HTTPS te negeren, zeker omdat er elk jaar meer cyberaanvallen worden uitgevoerd.

# traefik.me

Er is een speciale DNS-server, [traefik.me](https://traefik.me/), die IP-adressen converteert naar domeinnamen. Dit staat toe heel snel en eenvoudig een applicatie op te zetten die ook echt via een domeinnaam bereikt moet kunnen worden.

Op de site staat ook een wildcard certificaat voor alle `traefik.me` subdomeinen. Dat is geen fantastische garantie voor veiligheid (alle subdomeinen gebruiken hetzelfde certificaat), maar het staat je wel toe te experimenteren met HTTPS. Je kan het certificaat en de bijbehorende website downloaden van [traefik.me](https://traefik.me/).

Om de certificaten bekend te maken aan Traefik, raadpleeg je (de basis van) [deze pagina](https://doc.traefik.io/traefik/https/tls/). Daarmee is HTTPS nog niet actief. Je moet ook zorgen dat dat protocol ondersteund wordt. Daarvoor raadpleeg je [dit onderdeel van de documentatie](https://doc.traefik.io/traefik/routing/routers/#tls).

# LetsEncrypt

Als je een eigen domeinnaam hebt, kan je ook zorgen dat er automatisch (en gratis) aan domeinnaamvalidatie gedaan wordt via de dienst LetsEncrypt. 

Raadpleeg het begin van [deze pagina](https://doc.traefik.io/traefik/https/acme/) om aan de slag te gaan (tot en met het onderdeel "Certificate Resolvers"). In het begin moet je de optie `caServer` uit commentaar halen om te vermijden dat je op de rate limit botst voor alles goed werkt. En, zoals er ook staat, moet je niet alleen de certificate resolver(s) definiëren, maar moet je per router aangeven dat een resolver van toepassing is ("Each router that is supposed to use the resolver must reference it. ("Each router that is supposed to use the resolver must reference it.").

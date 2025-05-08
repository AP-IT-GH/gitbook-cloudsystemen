# Wat en waarom?

Een van de interessantste features van Traefik is hoe makkelijk deze reverse proxy het maakt om HTTPS te implementeren. Hiervoor moet een server in principe beschikken over een private key en een TLS-certificaat. Dat laatste wordt uitgegeven door een vertrouwde derde partij en is tijdelijk geldig.

Vroeger moest je zo'n certificaat regelmatig handmatig vernieuwen. Voor veel websites was dat het niet waard. Nu is dat veel gemakkelijker en is er geen enkel excuus om HTTPS te negeren, zeker omdat er elk jaar meer cyberaanvallen worden uitgevoerd.

# traefik.me

Er is een speciale DNS-server, [traefik.me](https://traefik.me/), die IP-adressen converteert naar domeinnamen. Dit staat toe heel snel en eenvoudig een applicatie op te zetten die ook echt via een domeinnaam bereikt moet kunnen worden.

# self-signed certificaten

Een "self-signed" certificaat is een certificaat waarbij je zelf optreedt als "vertrouwde partij". Het is alsof je een document ondertekent waarmee je zegt dat je jezelf wel best betrouwbaar vindt. Niet iets dat je zomaar zou mogen vertrouwen, maar je kan het gebruiken om te experimenteren met het gebruik van HTTPS.

Je kan een self-signed certificaat maken met dit commando: `openssl req -newkey rsa:2048 -keyout domain.key -x509 -days 365 -out domain.crt`.
Dit omvat een paar deelstappen die je [hier](https://www.baeldung.com/openssl-self-signed-cert) kan lezen indien je geïnteresseerd bent. Wanneer gevraagd wordt om een "FQDN" ("fully qualified domain name") vul je je domeinnaam in, zonder `https://` en zonder pad.

Om de gegenereerde bestanden bekend te maken aan Traefik, raadpleeg je (de basis van) [deze pagina](https://doc.traefik.io/traefik/https/tls/). Daarmee is HTTPS nog niet actief. Je moet ook zorgen dat dat protocol ondersteund wordt. Daarvoor raadpleeg je [dit onderdeel van de documentatie](https://doc.traefik.io/traefik/routing/routers/#tls).

**Zelfs als dit werkt, zal je een waarschuwing krijgen wanneer je de website bezoekt.** Je browser zal namelijk merken dat het certificaat self-signed is. Als je weet dat dit klopt, mag je (soms via "geavanceerde opties") kiezen om toch verder te gaan.

# LetsEncrypt

Als je een eigen domeinnaam hebt, kan je ook zorgen dat er automatisch (en gratis) aan domeinnaamvalidatie gedaan wordt via de dienst LetsEncrypt. 

Raadpleeg het begin van [deze pagina](https://doc.traefik.io/traefik/https/acme/) om aan de slag te gaan (tot en met het onderdeel "Certificate Resolvers"). In het begin moet je de optie `caServer` uit commentaar halen om te vermijden dat je op de rate limit botst voor alles goed werkt. En, zoals er ook staat, moet je niet alleen de certificate resolver(s) definiëren, maar moet je per router aangeven dat een resolver van toepassing is ("Each router that is supposed to use the resolver must reference it. ("Each router that is supposed to use the resolver must reference it.").

# Guides
Traefik zelf biedt enkele stappenplannen aan om zelf TLS op te zetten. Je vindt er [hier](https://doc.traefik.io/traefik/user-guides/docker-compose/acme-tls/) een. Enkel de "TLS challenge" of de "HTTP challenge" is genoeg, je hoeft niet beide te gebruiken.

Je doet dit best als volgt:

1. **Zorg alvast dat je een domeinnaam geregistreerd hebt.** Dat mag een gratis domeinnaam zijn. Als je een top-level domain wil, kan dat door een [Github Student Developer Pack](https://education.github.com/pack) aan te vragen. Dan kan je een domeinnaam aankopen en aan de kassa een korting invoeren zodat de totaalprijs 0 euro bedraagt. Om het top-level domain zelf te registreren in DNS, vul je als "subdomein" `@` in.
2. Zorg dat je het "basic example" op je server aan de praat krijgt. Als je dit op een subdomein bereikbaar wil maken, moet je ook een A-record toevoegen voor dat subdomein.
3. Commit je "basic example".
4. Zorg dat je de versie met de TLS challenge of HTTP challenge aan de praat krijgt. **Gebruik eerst de staging server van LetsEncrypt. In het voorbeeld staat die nog in commentaar!**
5. Controleer dat je de HTTPS-versie van je applicatie te zien krijgt met een "self-signed certificate". Dat leidt normaal tot een waarschuwing in je browser, maar dat is wat we hier willen.
6. Commit die versie.
7. Vervang de `whoami` service door je eigen service(s) die je achter Traefik wil plaatsen.
8. Commit wanneer dat werkt.
9. Schakel uiteindelijk de staging server uit door die regel in commentaar te zetten.

{% hint style="warning" %}
Omwille van de werking van DNS kan het even duren voor je domeinnaam overal gekend is. Waarschijnlijk zal je na registratie even (hooguit een volledige dag) moeten wachten tot je een TLS-certificaat kan krijgen voor je domeinnaam.
{% endhint %}

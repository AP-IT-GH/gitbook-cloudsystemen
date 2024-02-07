## Collision domains
Omdat hubs inkomende bits meteen op al hun poorten kopiëren, zullen er botsingen plaatsvinden als twee hosts gelijktijdig zenden. We zeggen dus dat hosts verbonden aan dezelfde hub in hetzelfde collision domain zitten.

Bij switches is dit minder een probleem: een switch kijkt naar het laag 2-adres van de ontvanger, het MAC-adres genaamd en stuurt de boodschap alleen door via de poort die naar dit adres leidt. Er zijn manieren waarop er meerdere MAC-adressen achter één poort kunnen staan (bijvoorbeeld door een switch met een hub of met een andere switch te verbinden), maar dit verkleint normaal wel de collision domains: enkel hosts achter dezelfde poort zitten in hetzelfde collision domain.

## Broadcast domains
Als het MAC-adres `FFFF.FFFF.FFFF` gebruikt wordt, gaat een bericht naar *alle* ontvangers binnen hetzelfde netwerk (niet naar andere netwerken, want routers houden deze berichten tegen). Toestellen verbonden aan dezelfde switch zitten dus in hetzelfde **broadcast domain**.

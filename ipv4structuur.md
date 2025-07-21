# IPv4

## Structuur
Een IPv4-adres bestaat uit 32 bits oftewel 4 bytes.
Meestal wordt het in een decimale notatie opgeschreven, bijvoorbeeld `192.168.0.222`.
Het kan natuurlijk ook anders geschreven worden, bijvoorbeeld binair `11000000.1010000.00000000.11011110`.

{% hint style="warning" %}
Aangezien de grootste waarde die je met één byte kan voorstellen 255 is, zal je in deze reeks van vier getallen nooit 256 of meer tegenkomen.
{% endhint %}

Een IP-adres heeft een andere structuur dan een MAC-adres.
Het dient niet om een machine te identificeren, maar wel om hem terug te vinden.

Vergelijk dit met de structuur van een gewoon adres dat je op een brief zou noteren:

```text
Ellermanstraat 33 # specifieker dan de volgende regel (en "33" is specifieker dan "Ellermanstraat")
2000 Antwerpen    # specifieker dan de volgende regel
BELGIË            # heel algemeen
```

Ruw gesteld kan je de eerste byte ("192" in het voorbeeld van eerder) vergelijken met de regel "BELGIË", de tweede byte ("168") met "2000 Antwerpen", enzovoort. Het grote voordeel van dit soort werkwijze is dat de postdienst van de afzender niet exact moet weten waar "Ellermanstraat 33" is. Hij moet gewoon weten hoe hij de brief tot in België krijgt.

{% hint style="danger" %}
Let op, er staat "ruw gesteld"! Het is niet altijd zo dat elk van de vier bytes een niveau van detail voorstelt, maar het is een goed vertrekpunt om er zo over te denken. Dit wordt duidelijker na de uitleg over netmaskers.
{% endhint %}

Het IP-adres van een machine verandert (onder normale omstandigheden) wanneer die machine van netwerk verandert, net zoals je thuisadres verandert wanneer je ergens anders gaat wonen.

|                       | MAC-adres           | IP-adres      |
|-----------------------|---------------------|---------------|
| sterkte associatie    | vast                | veranderlijk  |
| structuur             | vlak                | hiërarchisch  |
| begin afhankelijk van | fabrikant           | subnet        |
| vergelijk met         | rijksregisternummer | thuisadres    |

{% hint style="info" %}
Je zal misschien ergens iets tegenkomen over "klassen" van IP-adressen, zoals "klasse A", "klasse B",...
Dit is een ouder systeem dat voor programmeurs niet relevant is.
{% end hint %}

# IPv4

## Structuur
Een IPv4-adres bestaat uit 32 bits oftewel 4 bytes.
Meestal wordt het in een decimale notatie opgeschreven, bijvoorbeeld `192.168.0.222`.
Het kan natuurlijk ook anders geschreven worden, bijvoorbeeld binair `11000000.1010000.00000000.11011110`.

Een IP-adres heeft een andere structuur dan een MAC-adres.
Het dient niet om een machine te identificeren, maar wel om hem terug te vinden.
Het verandert dus ook wanneer de machine van netwerk verandert.

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

Github Actions is een "hosted" dienst. Je "runners" voeren uit op de servers van Github. Dat is vaak handig, maar niet als je een private repository hebt (want dan moet je betalen) of als je om een andere reden geen gebruik wil maken van die externe dienst.

[Act](https://github.com/nektos/act) is een tool waarmee je de workflows van Github Actions lokaal kan uitvoeren. In plaats van een virtuele machine, gebruik je dan een Docker container die heel gelijkaardig geconfigureerd is aan de runners van Github. Dit betekent dat de resultaten niet 100% identiek zullen zijn, maar voor veel taken is het wel goed genoeg.

Act uitvoeren is vrij eenvoudig, maar je doet dit best in de **[Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701?rtc=1&hl=fr-CA&gl=BE)** (of toch in een terminal met goede ondersteuning voor emoji's en dergelijke).

{% hint style="warning" %}
De Windows Terminal is niet de terminal die je standaard op elke versie van Windows hebt!
Op Mac en Linux heb je typisch al een goede terminal.
{% endhint %}

Eens je Act hebt, kan je een event van Github Actions nabootsen door het mee te geven als argument. Bijvoorbeeld `act push` om een push naar je repository na te bootsen. Normaal wordt bij het opstarten van Act gevraagd welke image je wil gebruiken om de VM van Github na te bootsen. Als dat niet zo is, moet je dit manueel aangeven met de `-P` vlag, bijvoorbeeld `act push -P ubuntu-latest=ghcr.io/catthehacker/ubuntu:act-22.04`. De beste keuze zal afhangen van de VM die je gebruikt voor Github Actions.

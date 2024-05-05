# Lokale services toevoegen
Het is mogelijk faasd uit te breiden met extra diensten. Dat kunnen diensten zijn die de serverless functies ondersteunen, maar het kunnen ook diensten zijn om data over het gebruik van de functies te verzamelen.

faasd handelt dit af met een `docker-compose.yaml` file, maar **Docker Compose wordt hier niet gebruikt**. faasd gebruikt het **formaat** van Docker Compose, maar je kan dus niet noodzakelijk alles overdragen.

{% hint style="info" %}
Sterker nog, normaal zal `docker` niet eens werken op je VM. De onderliggende runtime wel, maar daar werk je normaal veel minder mee.
{% endhint %}

Indien je serverless functies host bij van een cloud provider, zal je dit niet doen. Dan zal je typisch gebruik maken van een "managed database", d.w.z. een database waarbij de cloud provider zorgt voor het opschalen van capaciteit en dergelijke. Voorbeelden zijn AWS Aurora, Google Cloud SQL of Azure SQL Database.

## Werking MySQL
Als je faasd hebt geïnstalleerd met de gegeven cloud-config.txt, dan vind je (op de VM) in `/var/lib/faasd/docker-compose.yml` een service `db` terug die een MySQL toevoegt. Om technische redenen gebeurt dit iets anders dan elders in de cursus, maar dit zorgt dat onze serverless functies kunnen verbinden met een database op het IP `10.62.0.1`. Dit IP is speciaal voor OpenFaaS.

## Services herladen
Als je de Docker Compose file aanpast, moet je hem herladen via: `sduo systemctl daemon-reload && sudo systemctl restart faasd`.

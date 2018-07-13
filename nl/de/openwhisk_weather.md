---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-27"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Weather-Paket
{: #openwhisk_catalog_weather}

Das Paket `/whisk.system/weather` bietet eine komfortable Methode zum Aufrufen der Weather Company Data für die {{site.data.keyword.Bluemix}}-API.
{: shortdesc}

Das Paket enthält die folgende Aktion.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Paket | username, password | Services der Weather Company Data für die {{site.data.keyword.Bluemix_notm}}-API  |
| `/whisk.system/weather/forecast` | Aktion | latitude, longitude, timePeriod | Vorhersage für angegebenen Zeitraum|

Es wird empfohlen, eine Paketbindung mit den Werten `username` und `password` zu erstellen. Auf diese Weise brauchen Sie die Berechtigungsnachweise nicht jedes Mal anzugeben, wenn Sie die Aktionen im Paket aufrufen.

## Weather-Paket in {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.Bluemix_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.Bluemix_notm}} Weather-Serviceinstanzen erstellt.

1. Erstellen Sie eine Serviceinstanz von Weather Company Data in Ihrem {{site.data.keyword.Bluemix_notm}}-[Dashboard](http://console.bluemix.net).

  Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.Bluemix_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Serviceinstanz von Weather Company Data, die Sie erstellt haben.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Beispielausgabe:
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Listen Sie die Pakete auf, um zu ermitteln, ob die Paketbindung erstellt wurde:
  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Beispielausgabe:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Weather-Paket außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.Bluemix_notm}} verwenden oder wenn Sie den Weather Company Data-Service außerhalb von {{site.data.keyword.Bluemix_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Weather Company Data-Service erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort für den Weather Company Data-Service.

Erstellen Sie eine Paketbindung, die für Ihren Weather-Service konfiguriert ist.
```
ibmcloud wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Wettervorhersage für einen Standort abrufen
{: #openwhisk_catalog_weather_forecast}

Die Aktion `/whisk.system/weather/forecast` gibt eine Wettervorhersage für einen Standort durch einen Aufruf der API für The Weather Company zurück. Die folgenden Parameter sind verfügbar:

- `username`: Der Benutzername für The Weather Company Data für {{site.data.keyword.Bluemix_notm}}, der berechtigt ist, die API für die Vorhersage aufzurufen.
- `password`: Das Kennwort für The Weather Company Data für {{site.data.keyword.Bluemix_notm}}, das berechtigt ist, die API für die Vorhersage aufzurufen.
- `latitude`: Die Breitengradkoordinate des Standorts.
- `longitude`: Die Längengradkoordinate des Standorts.
- `timePeriod`: Der Zeitraum für die Vorhersage. Gültige Optionen sind:
  - `10day` (Standardwert) Gibt eine tägliche 10-Tage-Vorhersage zurück
  - `48hour` - Gibt eine stündliche 2-Tages-Vorhersage zurück
  - `current` - Gibt die aktuellen Wetterbedingungen zurück
  - `timeseries` - Gibt sowohl die aktuellen Beobachtungen als auch vergangene Beobachtungen rückwirkend bis zu 24 Stunden ab dem aktuellen Zeitpunkt (Datum und Uhrzeit) zurück.

Das folgende Beispiel zeigt die Erstellung einer Paketbindung und den anschließenden Abruf einer 10-Tage-Vorhersage.

Rufen Sie die Aktion **forecast** in Ihrer Paketbindung auf, um die Wettervorhersage abzurufen.
```
ibmcloud wsk action invoke myWeather/forecast --result \
--param latitude 43.7 \
--param longitude -79.4
```
{: pre}

Beispielausgabe:
```
{
    "forecasts": [
          {
            "dow": "Wednesday",
              "max_temp": -1,
              "min_temp": -16,
              "narrative": "Chance of a few snow showers. Highs -2 to 0C and lows -17 to -15C.",
            ...
        },
        {
            "class": "fod_long_range_daily",
              "dow": "Thursday",
              "max_temp": -4,
              "min_temp": -8,
              "narrative": "Mostly sunny. Highs -5 to -3C and lows -9 to -7C.",
            ...
        },
        ...
    ],
}
```
{: screen}

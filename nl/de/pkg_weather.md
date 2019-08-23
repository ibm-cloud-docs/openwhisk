---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: weather package, forecast, functions, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Weather
{: #pkg_weather}

Das vorinstallierte Paket `/whisk.system/weather` bietet eine komfortable Methode zum Aufrufen der Weather Company Data für die {{site.data.keyword.cloud}}-API.
{: shortdesc}

Das Paket enthält die folgende Aktion.

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Paket | `username`, `password` | Services der Weather Company Data für die {{site.data.keyword.cloud_notm}}-API. |
| `/whisk.system/weather/forecast` | Aktion | `latitude`, `longitude`, `timePeriod` | Vorhersage für den angegebenen Zeitraum. |

Es wird empfohlen, eine Paketbindung mit den Werten `username` und `password` zu erstellen. Auf diese Weise brauchen Sie die Berechtigungsnachweise nicht jedes Mal anzugeben, wenn Sie die Aktionen im Paket aufrufen.

## Weather-Paket in {{site.data.keyword.cloud_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk}} über {{site.data.keyword.cloud_notm}} verwenden, werden die Paketbindungen automatisch für Ihre {{site.data.keyword.cloud_notm}} Weather-Serviceinstanzen erstellt.

1. Erstellen Sie eine Weather Company Data-Serviceinstanz in Ihrem {{site.data.keyword.cloud_notm}}-[Dashboard](https://cloud.ibm.com){: external}. 

  Stellen Sie sicher, dass Sie sich den Namen der Serviceinstanz sowie der {{site.data.keyword.cloud_notm}}-Organisation und den Bereich, in dem Sie sich befinden, merken.

2. Aktualisieren Sie die Pakete in Ihrem Namensbereich. Die Aktualisierung erstellt automatisch eine Paketbindung für die Serviceinstanz von Weather Company Data, die Sie erstellt haben.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Beispielausgabe**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Listen Sie die Pakete auf, um zu ermitteln, ob die Paketbindung erstellt wurde:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Beispielausgabe**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Weather-Paket außerhalb von {{site.data.keyword.cloud_notm}} einrichten

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht in {{site.data.keyword.cloud_notm}} verwenden oder wenn Sie den Weather Company Data-Service außerhalb von {{site.data.keyword.cloud_notm}} einrichten möchten, müssen Sie manuell eine Paketbindung für Ihren Weather Company Data-Service erstellen. Sie benötigen hierzu den Benutzernamen und das Kennwort für den Weather Company Data-Service.

Erstellen Sie eine Paketbindung, die für Ihren Weather-Service konfiguriert ist.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Wettervorhersage für einen Standort abrufen
{: #pkg_weather_forecast}

Die Aktion `/whisk.system/weather/forecast` gibt eine Wettervorhersage für einen Standort durch einen Aufruf der API für The Weather Company zurück. Die Parameter lauten wie folgt. 

| Parameter | Beschreibung |
| --- | --- |
| `username` | Der Benutzername für Weather Company Data für {{site.data.keyword.cloud_notm}}, der für den Aufruf der Vorhersage-API berechtigt ist. |
| `password` | Das Kennwort des Benutzers von Weather Company Data für {{site.data.keyword.cloud_notm}}, der für den Aufruf der Vorhersage-API berechtigt ist. |
| `latitude` | Die Breitengradkoordinate des Standorts. |
| `longitude` | Die Längengradkoordinate des Standorts. |
| `timePeriod` | Der Zeitraum für die Vorhersage. |
</br>

Die folgende Tabelle enthält die Werte, die Sie mit dem Parameter `-- timePeriod` verwenden können. 
| `--timePeriod`-Werte | Beschreibung |
| --- | --- |
| `10day` | (Standardeinstellung) Gibt eine tägliche 10-Tage-Vorhersage zurück. |
| `48hour` | Gibt eine stündliche 2-Tage-Vorhersage zurück. |
| `current` | Gibt die aktuellen Wetterbedingungen zurück. |
| `timeseries` | Gibt die aktuellen Beobachtungen und die Beobachtungen für maximal die letzten 24 Stunden ab dem aktuellen Zeitpunkt (Datum und Uhrzeit) zurück. |


**Beispiel**
Das folgende Beispiel zeigt die Erstellung einer Paketbindung und den anschließenden Abruf einer 10-Tage-Vorhersage. 

Rufen Sie die Aktion `forecast` in Ihrer Paketbindung auf, um die Wettervorhersage abzurufen.
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**Ausgabe**
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



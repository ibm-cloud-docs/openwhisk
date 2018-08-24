---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-27"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Pacchetto Weather
{: #openwhisk_catalog_weather}

Il pacchetto `/whisk.system/weather` offre una soluzione pratica per richiamare l'API Weather Company Data for the {{site.data.keyword.Bluemix}}.
{: shortdesc}

Il pacchetto include la seguente azione.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/weather` | pacchetto | username, password | Servizi dall'API Weather Company Data for the {{site.data.keyword.Bluemix_notm}}  |
| `/whisk.system/weather/forecast` | azione | latitude, longitude, timePeriod | Previsione per il periodo di tempo specificato|

Si consiglia di effettuare la creazione di un bind di pacchetto con i valori `username` e `password`. In questo modo, non dovrai specificare le credenziali ogni volta che richiami le azioni nel pacchetto.

## Configurazione del pacchetto Weather in {{site.data.keyword.Bluemix_notm}}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.Bluemix_notm}}, vengono creati automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.Bluemix_notm}} Weather.

1. Crea un'istanza del servizio Weather Company Data nel tuo [dashboard](http://console.bluemix.net) {{site.data.keyword.Bluemix_notm}}.

  Assicurati di ricordare il nome dell'istanza del servizio e dell'organizzazione e dello spazio {{site.data.keyword.Bluemix_notm}} in cui ti trovi.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio Weather Company Data da te creata.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Output di esempio:
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Elenca i pacchetti per verificare che il bind di pacchetto sia stato creato:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Configurazione di un pacchetto Weather all'esterno di {{site.data.keyword.Bluemix_notm}}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} o se vuoi configurare Weather Company Data all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio Weather Company Data. Ti servono il nome utente e la password del servizio Weather Company Data.

Crea un bind di pacchetto configurato per il tuo servizio Weather.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Come ottenere una previsione meteo per una località
{: #openwhisk_catalog_weather_forecast}

L'azione `/whisk.system/weather/forecast` restituisce una previsione meteo per una località richiamando un'API da The Weather Company. I parametri sono i seguenti:

- `username`: nome utente per The Weather Company Data for {{site.data.keyword.Bluemix_notm}} che è autorizzato a richiamare l'API di previsione.
- `password`: password per l'utente di The Weather Company Data for {{site.data.keyword.Bluemix_notm}} che è autorizzato a richiamare l'API di previsione.
- `latitude`: la coordinata di latitudine della località.
- `longitude`: la coordinata di longitudine della località.
- `timePeriod`: il periodo di tempo per la previsione. I valori validi sono:
  - `10day` - (predefinito) Restituisce una previsione giornaliera di 10 giorni
  - `48hour` - Restituisce una previsione oraria di 2 giorni
  - `current` - Restituisce le condizioni meteorologiche correnti
  - `timeseries` - Restituisce le rilevazioni in tempo reale e quelle delle ultime 24 ore dalla data/ora corrente.

Il seguente esempio mostra come creare un bind di pacchetto e quindi ottenere una previsione di 10 giorni.

Richiama l'azione **forecast** nel tuo bind di pacchetto per ottenere la previsione meteo.
```
ibmcloud fn action invoke myWeather/forecast --result \
--param latitude 43.7 \
--param longitude -79.4
```
{: pre}

Output di esempio:
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

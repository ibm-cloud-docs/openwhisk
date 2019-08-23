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

Il pacchetto `/whisk.system/weather` preinstallato offre una soluzione pratica per richiamare il servizio Weather Company Data per l'API {{site.data.keyword.cloud}}.
{: shortdesc}

Il pacchetto include la seguente azione.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Pacchetto | `username`, `password` | Servizi di Weather Company Data per l'API {{site.data.keyword.cloud_notm}}.  |
| `/whisk.system/weather/forecast` | Azione | `latitude`, `longitude`, `timePeriod` | Previsione per il periodo di tempo specificato. |

Si consiglia di effettuare la creazione di un bind di pacchetto con i valori `username` e `password`. In questo modo, non dovrai specificare le credenziali ogni volta che richiami le azioni nel pacchetto.

## Configurazione del pacchetto Weather in {{site.data.keyword.cloud_notm}}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.cloud_notm}}, vengono creati automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.cloud_notm}} Weather.

1. Crea un'istanza del servizio Weather Company Data nel tuo [dashboard](https://cloud.ibm.com){: external} {{site.data.keyword.cloud_notm}}.

  Assicurati di ricordare il nome dell'istanza del servizio e dell'organizzazione e dello spazio {{site.data.keyword.cloud_notm}} in cui ti trovi.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio Weather Company Data da te creata.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Output di esempio**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Elenca i pacchetti per verificare che il bind di pacchetto sia stato creato:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Output di esempio**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Configurazione di un pacchetto Weather all'esterno di {{site.data.keyword.cloud_notm}}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.cloud_notm}} o se vuoi configurare il tuo servizio Weather Company Data all'esterno di {{site.data.keyword.cloud_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio Weather Company Data. Ti servono il nome utente e la password del servizio Weather Company Data.

Crea un bind di pacchetto configurato per il tuo servizio Weather.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Come ottenere una previsione meteo per una località
{: #pkg_weather_forecast}

L'azione `/whisk.system/weather/forecast` restituisce una previsione meteo per una località richiamando un'API da The Weather Company. I parametri sono i seguenti.

| Parametro | Descrizione |
| --- | --- |
| `username` | Nome utente per The Weather Company Data for {{site.data.keyword.cloud_notm}} che è autorizzato a richiamare l'API di previsione. |
| `password` | Password per l'utente di The Weather Company Data for {{site.data.keyword.cloud_notm}} che è autorizzato a richiamare l'API di previsione. |
| `latitude` | La coordinata di latitudine della località. |
| `longitude` | La coordinata di longitudine della località. |
| `timePeriod` | Il periodo di tempo per la previsione. |
</br>

La seguente tabella contiene i valori che puoi utilizzare con il parametro `--timePeriod`.
| Valori `--timePeriod` | Descrizione |
| --- | --- |
| `10day` | (Predefinito) Restituisce una previsione giornaliera di 10 giorni. |
| `48hour` | Restituisce una previsione oraria di 2 giorni. |
| `current` | Restituisce le condizioni meteorologiche correnti. |
| `timeseries` | Restituisce le rilevazioni in tempo reale e quelle delle ultime 24 ore dalla data/ora corrente. |


**Esempio**
Il seguente esempio mostra come creare un bind di pacchetto e quindi ottenere una previsione di 10 giorni.

Richiama l'azione `forecast` nel tuo bind di pacchetto per ottenere la previsione meteo.
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**Output**
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



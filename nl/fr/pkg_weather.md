---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: weather package, forecast, functions, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Weather
{: #pkg_weather}

Le package préinstallé `/whisk.system/weather` offre une méthode pratique permettant d'appeler The Weather Company Data pour l'API {{site.data.keyword.Bluemix}}.
{: shortdesc}

Le package inclut l'action ci-dessous.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/weather` | package | username, password | Services de The Weather Company Data pour l'API {{site.data.keyword.Bluemix_notm}}  |
| `/whisk.system/weather/forecast` | action | latitude, longitude, timePeriod | Prévision pour la période spécifiée|

Il est recommandé de créer une liaison de package avec les valeurs `username` et `password`. Ainsi, il n'est pas nécessaire de spécifier les données d'identification à chaque fois que vous appelez les actions du package.

## Configuration du package Weather dans {{site.data.keyword.Bluemix_notm}}

Si vous utilisez {{site.data.keyword.openwhisk}} depuis {{site.data.keyword.Bluemix_notm}}, les liaisons de package sont automatiquement créées pour vos instances de service {{site.data.keyword.Bluemix_notm}} Weather.

1. Créez une instance de service Weather Company Data dans votre [tableau de bord](http://cloud.ibm.com) {{site.data.keyword.Bluemix_notm}}.

  Mémorisez le nom de l'instance de service ainsi que l'organisation et l'espace {{site.data.keyword.Bluemix_notm}} dans lesquels vous vous trouvez.

2. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour l'instance de service Weather Company Data que vous avez créée.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Exemple de sortie :
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Affichez la liste des packages pour vérifier que la liaison de package a bien été créée :
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Configuration d'un package Weather en dehors d'{{site.data.keyword.Bluemix_notm}}

Si vous n'utilisez pas {{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}} ou si vous voulez configurer le service Weather Company Data en dehors d'{{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour votre service Weather Company Data. Vous devez connaître le nom d'utilisateur et le mot de passe du service Weather Company Data.

Créez une liaison de package configurée pour votre service Weather.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Obtention d'une prévision météorologique pour un lieu
{: #pkg_weather_forecast}

L'action `/whisk.system/weather/forecast` renvoie une prévision météorologique pour un emplacement en appelant une API de The Weather Company. Les paramètres sont les suivants :

- `username` : nom d'utilisateur de The Weather Company Data pour {{site.data.keyword.Bluemix_notm}} qui est autorisé à appeler l'API de prévision.
- `password` : mot de passe de The Weather Company Data pour {{site.data.keyword.Bluemix_notm}} qui est autorisé à appeler l'API de prévision.
- `latitude` : coordonnée de latitude du lieu.
- `longitude` : coordonnée de longitude du lieu.
- `timePeriod`: période sur laquelle porte la prévision. Les options valides sont les suivantes :
  - `10day` - (valeur par défaut) Renvoie des prévisions sur 10 jours
  - `48hour` - Renvoie des prévisions sur 2 jours
  - `current` - Revoie les conditions météorologiques actuelles
  - `timeseries` - Renvoie les relevés actuels, ainsi que jusqu'à 24 heures de relevés antérieurs, à compter de la date et heure actuelle.

L'exemple suivant montre comment créer une liaison de package, puis obtenir une prévision sur 10 jours :

Appelez l'action **forecast** dans votre liaison de package pour obtenir la prévision météorologique.
```
ibmcloud fn action invoke myWeather/forecast --result \
--param latitude 43.7 \
--param longitude -79.4
```
{: pre}

Exemple de sortie :
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


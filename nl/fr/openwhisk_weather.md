---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation du package Weather
{: #openwhisk_catalog_weather}

Le package `/whisk.system/weather` offre une méthode pratique permettant d'appeler The Weather Company Data pour l'API {{site.data.keyword.Bluemix}}.
{: shortdesc}

Le package comprend l'action suivante :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Package | username, password | Services de The Weather Company Data pour l'API {{site.data.keyword.Bluemix_notm}} |
| `/whisk.system/weather/forecast` | Action | latitude, longitude, timePeriod | prévision pour la période spécifiée|

Il est recommandé de créer une liaison de package avec les valeurs `username` et `password`. Ainsi, il n'est pas nécessaire de spécifier les données d'identification à chaque fois que vous appelez les actions du package.

## Configuration du package Weather dans {{site.data.keyword.Bluemix_notm}}

Si vous utilisez OpenWhisk depuis {{site.data.keyword.Bluemix_notm}}, OpenWhisk crée automatiquement des liaisons de package pour vos instances de service {{site.data.keyword.Bluemix_notm}} Weather. 

1. Créez une instance de service Weather Company Data dans votre [tableau de bord](http://console.ng.Bluemix.net) {{site.data.keyword.Bluemix_notm}}. 
  
  Mémorisez le nom de l'instance de service ainsi que l'organisation et l'espace {{site.data.keyword.Bluemix_notm}} dans lesquels vous vous
trouvez.
  
2. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour l'instance de service Weather Company Data que vous avez créée.
  
  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  
 
## Configuration d'un package Weather en dehors de {{site.data.keyword.Bluemix_notm}}

Si vous n'utilisez pas OpenWhisk dans {{site.data.keyword.Bluemix_notm}} ou si vous voulez configurer le service Weather Company Data en dehors de {{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour votre service WWeather Company Data. Vous devez connaître le nom d'utilisateur et le mot de passe du service Weather Company Data.

- Créez une liaison de package configurée pour votre service Watson
Translator.

  ```
  wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}


## Obtention d'une prévision météorologique pour un lieu
{: #openwhisk_catalog_weather_forecast}

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

- Appelez l'action `forecast` dans votre liaison de package pour obtenir la prévision météorologique.
  ```
  wsk action invoke myWeather/forecast --result \
  --param latitude 43.7 \
  --param longitude -79.4
  ```
  {: pre}
  
  ```json
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
  

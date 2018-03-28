---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-27"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Weather
{: #openwhisk_catalog_weather}

The `/whisk.system/weather` package offers a convenient way to call the Weather Company Data for the {{site.data.keyword.Bluemix}} API.
{: shortdesc}

The package includes the following Action.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Package | username, password | Services from the Weather Company Data for the {{site.data.keyword.Bluemix_notm}} API  |
| `/whisk.system/weather/forecast` | Action | latitude, longitude, timePeriod | forecast for specified time period|

Creating a package binding with the `username` and `password` values is suggested. This way, you don't need to specify the credentials every time you invoke the Actions in the package.

## Setting up the Weather package in {{site.data.keyword.Bluemix_notm}}

If you're using {{site.data.keyword.openwhisk}} from {{site.data.keyword.Bluemix_notm}}, the package bindings are automatically created for your {{site.data.keyword.Bluemix_notm}} Weather service instances.

1. Create a Weather Company Data service instance in your {{site.data.keyword.Bluemix_notm}} [dashboard](http://console.bluemix.net).
  
  Be sure to remember the name of the service instance and the {{site.data.keyword.Bluemix_notm}} organization and space you're in.
  
2. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Weather Company Data service instance that you created.
  ```
  bx wsk package refresh
  ```
  {: pre}

  **Output:**
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  {: screen}

  List packages to see that the package binding was created:
  ```
  bx wsk package list
  ```
  {: pre}

  **Output:**
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  {: screen}
  
## Setting up a Weather package outside {{site.data.keyword.Bluemix_notm}}

If you're not using {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} or if you want to set up your Weather Company Data service outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your WWeather Company Data service. You need the Weather Company Data service user name, and password.

Create a package binding that is configured for your Watson Translator service.
```
bx wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Getting a weather forecast for a location
{: #openwhisk_catalog_weather_forecast}

The `/whisk.system/weather/forecast` Action returns a weather forecast for a location by calling an API from The Weather Company. The parameters are as follows:

- `username`: Username for The Weather Company Data for {{site.data.keyword.Bluemix_notm}} that is entitled to invoke the forecast API.
- `password`: Password for The Weather Company Data for {{site.data.keyword.Bluemix_notm}} that is entitled to invoke the forecast API.
- `latitude`: The latitude coordinate of the location.
- `longitude`: The longitude coordinate of the location.
- `timePeriod`: Time period for the forecast. Valid options are:
  - `10day` - (default) Returns a daily 10-day forecast
  - `48hour` - Returns an hourly 2-day forecast
  - `current` - Returns the current weather conditions
  - `timeseries` - Returns both the current observations and up to 24 hours of past observations, from the current date and time.

The following example shows how to create a package binding and then getting a 10-day forecast.

Invoke the **forecast** Action in your package binding to get the weather forecast.
```
bx wsk action invoke myWeather/forecast --result \
--param latitude 43.7 \
--param longitude -79.4
```
{: pre}

**Output:**
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

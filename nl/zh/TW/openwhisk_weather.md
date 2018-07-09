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

`/whisk.system/weather` 套件提供一種簡便的方式來呼叫 Weather Company Data for {{site.data.keyword.Bluemix}} API。
{: shortdesc}

該套件包含下列動作。

|實體|類型|參數|說明
|
| --- | --- | --- | --- |
| `/whisk.system/weather` |套件|username、password|Weather Company Data for {{site.data.keyword.Bluemix_notm}} API 的服務 |
|`/whisk.system/weather/forecast` |動作|latitude、longitude、timePeriod|指定時段的預報|

建議使用 `username` 及 `password` 值來建立套件連結。如此，您就不需要每次在呼叫套件中的動作時都指定認證。

## 在 {{site.data.keyword.Bluemix_notm}} 中設定 Weather 套件

如果您是從 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk}}，則會自動為 {{site.data.keyword.Bluemix_notm}} Weather 服務實例建立套件連結。

1. 在 {{site.data.keyword.Bluemix_notm}} [儀表板](http://console.bluemix.net)中，建立 Weather Company Data 服務實例。

  請務必記住服務實例名稱，以及您所在的 {{site.data.keyword.Bluemix_notm}} 組織及空間。

2. 重新整理名稱空間中的套件。重新整理會自動建立您所建立 Weather Company Data 服務實例的套件連結。
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  輸出範例：
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  {: screen}

  列出套件，查看已建立套件連結：
  ```
  ibmcloud wsk package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## 在 {{site.data.keyword.Bluemix_notm}} 外部設定 Weather 套件

如果您不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者要在 {{site.data.keyword.Bluemix_notm}} 外部設定 Weather Company Data 服務，則必須手動建立 WWeather Company Data 服務的套件連結。您需要 Weather Company Data 服務使用者名稱及密碼。

建立針對 Watson Translator 服務所配置的套件連結。
```
ibmcloud wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 取得某個位置的天氣預報
{: #openwhisk_catalog_weather_forecast}

`/whisk.system/weather/forecast` 動作會從 The Weather Company 呼叫 API，以傳回某個位置的天氣預報。參數如下所示：

- `username`：獲授權呼叫預報 API 的 Weather Company Data for {{site.data.keyword.Bluemix_notm}} 的使用者名稱。
- `password`：獲授權呼叫預報 API 的 Weather Company Data for {{site.data.keyword.Bluemix_notm}} 的密碼。
- `latitude`：位置的緯度座標。
- `longitude`：位置的經度座標。
- `timePeriod`：預報的時段。有效的選項如下：
  - `10day` -（預設值）傳回 10 天的每日預報
  - `48hour` - 傳回 2 天的每小時預報
  - `current` - 傳回現行天氣狀況
  - `timeseries` - 傳回現行的觀察，以及從目前日期和時間算起，最多過去 24 小時的觀察。

下列範例顯示如何建立套件連結，然後取得 10 天預測。

在套件連結中呼叫 **forecast** 動作，以取得天氣預報。
```
ibmcloud wsk action invoke myWeather/forecast --result \
--param latitude 43.7 \
--param longitude -79.4
```
{: pre}

輸出範例：
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

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

透過預先安裝的 `/whisk.system/weather` 套件，可以方便地呼叫 {{site.data.keyword.cloud}} API 的 Weather Company Data。
{: shortdesc}

該套件包含下列動作。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/weather` |套件| `username`、`password` |來自 The Weather Company Data for the {{site.data.keyword.cloud_notm}} API 的服務。|
|`/whisk.system/weather/forecast` |動作|`latitude`、`longitude`、`timePeriod`|指定時段的預測。|

建議使用 `username` 及 `password` 值來建立套件連結。如此，您就不需要每次在呼叫套件中的動作時都指定認證。

## 在 {{site.data.keyword.cloud_notm}} 中設定 Weather 套件

如果是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk}}，則將為 {{site.data.keyword.cloud_notm}} Weather 服務實例自動建立套件連結。

1. 在 {{site.data.keyword.cloud_notm}} [儀表板](https://cloud.ibm.com){: external}中，建立 Weather Company Data 服務實例。

  請務必記住服務實例名稱，以及您所在的 {{site.data.keyword.cloud_notm}} 組織及空間。

2. 重新整理名稱空間中的套件。重新整理會自動建立您所建立 Weather Company Data 服務實例的套件連結。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **輸出範例**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  列出套件，查看已建立套件連結：
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **輸出範例**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## 在 {{site.data.keyword.cloud_notm}} 外部設定 Weather 套件

如果不是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.cloud_notm}} 外部設定 Weather Company Data 服務，則必須為 Weather Company Data 服務手動建立套件連結。您需要 Weather Company Data 服務使用者名稱及密碼。

建立針對 Weather 服務所配置的套件連結。
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 取得某個位置的天氣預報
{: #pkg_weather_forecast}

`/whisk.system/weather/forecast` 動作會從 The Weather Company 呼叫 API，以傳回某個位置的天氣預報。參數如下。

| 參數 |說明|
| --- | --- |
| `username` |有權呼叫預報 API 的 The Weather Company Data for {{site.data.keyword.cloud_notm}} 使用者的使用者名稱。|
| `password` |有權呼叫預報 API 的 The Weather Company Data for {{site.data.keyword.cloud_notm}} 使用者的密碼。|
| `latitude` |位置的緯度座標。|
| `longitude` |位置的經度座標。|
| `timePeriod` |預報的時段。|
</br>

下表包含可用於 `--timePeriod` 參數的值。
| `--timePeriod` 值 | 說明 |
| --- | --- |
| `10day` |（預設）傳回 10 天的每日預報。|
| `48hour` |傳回 2 天的依每小時預報。|
| `current` |傳回現行天氣狀況。|
| `timeseries` |傳回現行觀察資料和過去長達 24 小時（從現行日期和時間開始）的觀察資料。|


**範例**
下列範例顯示如何建立套件連結並取得 10 天的預報。

在套件連結中呼叫 `forecast` 動作，以取得天氣預報。
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**輸出**
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



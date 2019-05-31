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

事前インストール済みの `/whisk.system/weather` パッケージを利用して、Weather Company Data for the {{site.data.keyword.Bluemix}} API を簡単に呼び出すことができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/weather` | パッケージ | username、password | Weather Company Data for the {{site.data.keyword.Bluemix_notm}} API からのサービス  |
| `/whisk.system/weather/forecast` | アクション | latitude、longitude、timePeriod | 指定された時間枠の予測|

`username` と `password` の値を使用して、パッケージ・バインディングを作成することをお勧めします。 この方法を使用すると、パッケージ内のアクションを起動するたびに資格情報を指定する必要はありません。

## {{site.data.keyword.Bluemix_notm}} での Weather パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用している場合、パッケージ・バインディングは {{site.data.keyword.Bluemix_notm}} Weather サービス・インスタンス用に自動的に作成されます。

1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](http://cloud.ibm.com)で Weather Company Data のサービス・インスタンスを作成します。

  サービス・インスタンスの名前、および今使用している {{site.data.keyword.Bluemix_notm}} の組織とスペースの名前を忘れないようにしてください。

2. 名前空間でパッケージを最新表示します。 最新表示により、作成した Weather Company Data サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  出力例:
  ```
  created bindings:
  Bluemix_Weather_Company_Data_Credentials-1
  ```
  {: screen}

  パッケージをリストして、パッケージ・バインディングが作成されたことを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Weather Bluemix_Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 外部での Weather パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}} を使用していない場合、または {{site.data.keyword.Bluemix_notm}} の外部で Weather Company Data サービスをセットアップしたい場合は、Weather Company Data サービスのパッケージ・バインディングを手動で作成する必要があります。Weather Company Data サービスのユーザー名とパスワードが必要になります。

Weather サービス用に構成されるパッケージ・バインディングを作成します。
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 場所を指定した天気予報の取得
{: #pkg_weather_forecast}

`/whisk.system/weather/forecast` アクションは、Weather Company の API を呼び出すことによって、特定の場所の天気予報を返します。 パラメーターは次のとおりです。

- `username`: 予報 API を起動する権限を与えられた Weather Company Data for {{site.data.keyword.Bluemix_notm}} のユーザー名。
- `password`: 予報 API を起動する権限を与えられた Weather Company Data for {{site.data.keyword.Bluemix_notm}} のパスワード。
- `latitude`: 場所の経度の座標。
- `longitude`: 場所の緯度の座標。
- `timePeriod`: 予報の時間枠。 有効なオプションは次のとおりです。
  - `10day` - (デフォルト) 毎日の 10 日間予報を返します
  - `48hour` - 毎時の 2 日間予報を返します
  - `current` - 現在の気象状態を返します
  - `timeseries` - 現在の日時から、現在の観測と過去 24 時間までの観測の両方を返します。

以下の例は、パッケージ・バインディングを作成してから、10 日間予報を取得する方法を示しています。

パッケージ・バインディング内の **forecast** アクションを呼び出して、天気予報を取得します。
```
ibmcloud fn action invoke myWeather/forecast --result \
--param latitude 43.7 \
--param longitude -79.4
```
{: pre}

出力例:
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


---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Weather パッケージの使用
{: #openwhisk_catalog_weather}

`/whisk.system/weather` パッケージは、Weather Company Data for IBM Bluemix API を呼び出すのに便利な方法を提供します。

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ  | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/weather` | パッケージ | username、password | Weather Company Data for IBM Bluemix API からのサービス  |
| `/whisk.system/weather/forecast` | アクション | latitude、longitude、timePeriod | 指定された時間枠の予測|

`username` と `password` の値を使用して、パッケージ・バインディングを作成することをお勧めします。この方法を使用すると、パッケージ内のアクションを起動するたびに資格情報を指定する必要はありません。

## Bluemix での Weather パッケージのセットアップ

Bluemix から OpenWhisk を使用している場合、Bluemix Weather サービス・インスタンスのパッケージ・バインディングは OpenWhisk が自動的に作成します。

1. Bluemix [ダッシュボード](http://console.ng.Bluemix.net)で Weather Company Data のサービス・インスタンスを作成します。
  
  サービス・インスタンスの名前、およびユーザーが所属している Bluemix の組織とスペースの名前を忘れないようにしてください。
  
2. 名前空間でパッケージを最新表示します。最新表示により、作成した Weather Company Data サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  
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
  
 
## Bluemix 外部での Weather パッケージのセットアップ

Bluemix で OpenWhisk を使用していない場合、または Bluemix の外部で Weather Company Data サービスをセットアップしたい場合は、Weather Company Data サービスのパッケージ・バインディングを手動で作成する必要があります。Weather Company Data サービスのユーザー名とパスワードが必要になります。

- Watson Translator サービス用に構成されるパッケージ・バインディングを作成します。

  ```
  wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}


## 場所を指定した天気予報の取得
{: #openwhisk_catalog_weather_forecast}

`/whisk.system/weather/forecast` アクション
は、The Weather Company から API を呼び出して、場所を指定した天気予報を返します。パラメーターは次のとおりです。


- `username`: 予測 API を起動する権限を与えられた Weather Company Data for IBM Bluemix のユーザー名。
- `password`: 予測 API を起動する権限を与えられた Weather Company Data for IBM Bluemix のパスワード。
- `latitude`: 場所の経度の座標。
- `longitude`: 場所の緯度の座標。
- `timePeriod`: 予測の時間枠。有効なオプションは次のとおりです。
  - `10day` - (デフォルト) 毎日の 10 日間予測を返します
  - `48hour` - 毎時の 2 日間予測を返します
  - `current` - 現在の気象状態を返します
  - `timeseries` - 現在の日時から、現在の観測と過去 24 時間までの観測の両方を返します。


- 以下は、パッケージ・バインディングを作成してから 10 日間の予測を取得する例です。API キーを使用してパッケージ・バインディングを作成します。
  
  ```
  wsk package bind /whisk.system/weather myWeather --param username MY_USERNAME --param password MY_PASSWORD
  ```
  {: pre}

- パッケージ・バインディングの `forecast` アクションを起動して、天気予報を取得します。
  
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
  

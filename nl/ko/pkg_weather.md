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

사전 설치된 `/whisk.system/weather` 패키지는 {{site.data.keyword.cloud}} API에 대한 Weather Company Data를 호출하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/weather` |패키지 |`username`, `password` |{{site.data.keyword.cloud_notm}} API에 대한 Weather Company Data의 서비스입니다. |
|`/whisk.system/weather/forecast` |액션 |`latitude`, `longitude`, `timePeriod` |지정된 기간에 대한 예보입니다. |

`username` 및 `password` 값으로 패키지 바인딩을 작성하도록 제안합니다. 이 방법을 사용하면 패키지에서 액션을 호출할 때마다 인증 정보를 지정할 필요가 없습니다.

## {{site.data.keyword.cloud_notm}}에서 Weather 패키지 설정

{{site.data.keyword.cloud_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우 패키지 바인딩이 {{site.data.keyword.cloud_notm}} Weather 서비스 인스턴스에 대해 자동으로 작성됩니다.

1. {{site.data.keyword.cloud_notm}} [대시보드](https://cloud.ibm.com){: external}에서 Weather Company Data 서비스 인스턴스를 작성하십시오.

  자신이 속한 {{site.data.keyword.cloud_notm}} 조직과 영역 및 서비스 인스턴스의 이름을 반드시 기억하십시오.

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 Weather Company Data 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **출력 예**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  패키지를 나열하여 패키지 바인딩이 작성되었는지 확인하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **출력 예**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.cloud_notm}} 외부에서 Weather 패키지 설정

{{site.data.keyword.cloud_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.cloud_notm}} 외부에서 Weather Company Data 서비스를 설정하려는 경우 Weather Company Data 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Weather Company Data 서비스 사용자 이름 및 비밀번호가 반드시 필요합니다.

Weather 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 위치에 대한 일기 예보 가져오기
{: #pkg_weather_forecast}

`/whisk.system/weather/forecast` 액션은 The Weather Company에서 API를 호출하여 위치에 대한 날씨 예보를 리턴합니다. 매개변수는 다음과 같습니다.

|매개변수 |설명 |
| --- | --- |
|`username` |예보 API 호출 자격이 있는 {{site.data.keyword.cloud_notm}}의 Weather Company Data에 대한 사용자 이름입니다. |
|`password` |예보 API 호출 자격이 있는 {{site.data.keyword.cloud_notm}}의 Weather Company Data에 대한 비밀번호입니다. |
|`latitude` |위치의 위도 좌표입니다. |
|`longitude` |위치의 경도 좌표입니다. |
|`timePeriod` |예보 기간입니다. |
</br>

다음 표에는 `--timePeriod` 매개변수에서 사용할 수 있는 값이 포함되어 있습니다.
|`--timePeriod` 값 |설명 |
| --- | --- |
|`10day` |(기본값) 일별로 10일간의 예보를 리턴합니다. |
|`48hour` |시간별로 2일간의 예보를 리턴합니다. |
|`current` |현재 기상 상태를 리턴합니다. |
|`timeseries` |현재 관측 및 현재 날짜와 시간에서 최대 24시간까지의 과거 관측을 모두 리턴합니다. |


**예**
다음 예제는 패키지 바인딩을 작성한 후에 10일간의 예보를 가져오는 방법을 보여줍니다.

패키지 바인딩에서 `forecast` 액션을 호출하여 날씨 예보를 가져오십시오.
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**출력**
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



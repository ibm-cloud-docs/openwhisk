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

通过预安装的 `/whisk.system/weather` 包，可以方便地调用 The Weather Company Data for the {{site.data.keyword.cloud}} API。
{: shortdesc}

此包中包含以下操作。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/weather`|包|`username`、`password` |来自 The Weather Company Data for the {{site.data.keyword.cloud_notm}} API 的服务。|
|`/whisk.system/weather/forecast`|操作|`latitude`、`longitude`、`timePeriod`|指定时间段的预报。|

建议使用 `username` 和 `password` 值创建包绑定。这样就无需在每次调用包中的操作时指定这些凭证。

## 在 {{site.data.keyword.cloud_notm}} 中设置 Weather 包

如果是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk}}，那么将为 {{site.data.keyword.cloud_notm}} Weather 服务实例自动创建包绑定。

1. 在 {{site.data.keyword.cloud_notm}} [仪表板](https://cloud.ibm.com){: external}中创建 Weather Company Data 服务实例。

  请务必记住服务实例的名称以及您所在的 {{site.data.keyword.cloud_notm}} 组织和空间的名称。

2. 刷新名称空间中的包。刷新操作将自动为已创建的 Weather Company Data 服务实例创建包绑定。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **示例输出**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  列出包以查看是否已创建包绑定：
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **示例输出**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## 在 {{site.data.keyword.cloud_notm}} 外部设置 Weather 包

如果不是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.cloud_notm}} 外部设置 Weather Company Data 服务，那么必须为 Weather Company Data 服务手动创建包绑定。您需要 Weather Company Data 服务用户名和密码。

创建为 Weather 服务配置的包绑定。
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 获取某个位置的天气预报
{: #pkg_weather_forecast}

`/whisk.system/weather/forecast` 操作通过从 The Weather Company 调用 API，返回某个位置的天气预报。参数如下所示。

|参数|描述|
| --- | --- |
|`username`|有权调用预报 API 的 The Weather Company Data for {{site.data.keyword.cloud_notm}} 用户的用户名。|
|`password`|有权调用预报 API 的 The Weather Company Data for {{site.data.keyword.cloud_notm}} 用户的密码。|
|`latitude`|位置的纬度坐标。|
|`longitude`|位置的经度坐标。|
|`timePeriod`|预报的时间段。|
</br>

下表包含可用于 `--timePeriod` 参数的值。
|`--timePeriod` 值|描述|
| --- | --- |
|`10day`|（缺省值）返回 10 天的每日预报。|
|`48hour`|（缺省值）返回 2 天的每小时预报。|
|`current`|返回当前天气状况。|
|`timeseries`|返回当前观察数据和过去长达 24 小时（从当前日期和时间开始）的观察数据。|


**示例**
以下示例显示如何创建包绑定并获取 10 天的预报。

调用包绑定中的 `forecast` 操作来获取天气预报。
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**输出**
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



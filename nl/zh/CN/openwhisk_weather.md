---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 使用 Weather 包
{: #openwhisk_catalog_weather}

通过 `/whisk.system/weather` 包，可以方便地调用 Weather Company Data for the {{site.data.keyword.Bluemix}} API。
{: shortdesc}

此包中包含以下操作。

| 实体| 类型| 参数| 描述
|
| --- | --- | --- | --- |
| `/whisk.system/weather`| 包| username 和 password| 来自 Weather Company Data for the {{site.data.keyword.Bluemix_notm}} API 的服务|
| `/whisk.system/weather/forecast`| 操作| latitude、longitude、timePeriod| 指定时间段的预报|

建议使用 `username` 和 `password` 值创建包绑定。这样就无需在每次调用包中的操作时指定这些凭证。

## 在 {{site.data.keyword.Bluemix_notm}} 中设置 Weather 包

如果是在 {{site.data.keyword.Bluemix_notm}} 中使用 OpenWhisk，那么 OpenWhisk 将为 {{site.data.keyword.Bluemix_notm}} Weather 服务实例自动创建包绑定。

1. 在 {{site.data.keyword.Bluemix_notm}} [仪表板](http://console.ng.Bluemix.net)中创建 Weather Company Data 服务实例。
  
  请务必记住服务实例的名称以及您所在的 {{site.data.keyword.Bluemix_notm}} 组织和空间的名称。
  
2. 刷新名称空间中的包。刷新操作将自动为已创建的 Weather Company Data 服务实例创建包绑定。
  
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
  
 
## 在 {{site.data.keyword.Bluemix_notm}} 外部设置 Weather 包

如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 OpenWhisk，或者如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 Weather Company Data 服务，那么必须为 Weather Company Data 服务手动创建包绑定。您需要 Weather Company Data 服务用户名和密码。

- 创建为您的 Watson Translator 服务配置的包绑定。

  ```
  wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}


## 获取某个位置的天气预报
{: #openwhisk_catalog_weather_forecast}

`/whisk.system/weather/forecast` 操作通过从 The Weather Company 调用 API，返回某个位置的天气预报。参数如下所示：

- `username`：有权调用预报 API 的 The Weather Company Data for {{site.data.keyword.Bluemix_notm}} 用户的用户名。
- `password`：有权调用预报 API 的 The Weather Company Data for {{site.data.keyword.Bluemix_notm}} 用户的密码。
- `latitude`：位置的纬度坐标。
- `longitude`：位置的经度坐标。
- `timePeriod`：预报的时间段。有效的选项为：
  - `10day` -（缺省值）返回每日 10 天的预测
  - `48hour` - 返回每小时 2 天的预测
  - `current` - 返回当前的天气条件
  - `timeseries` - 返回当前观察数据和过去长达 24 小时（从当前日期和时间开始）的观察数据。


以下示例显示如何创建包绑定并获取 10 天天气预报。

- 调用包绑定中的 `forecast` 操作来获取天气预报。
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
  

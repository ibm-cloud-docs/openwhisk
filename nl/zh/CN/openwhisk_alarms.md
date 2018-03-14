---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 使用 Alarm 包
{: #openwhisk_catalog_alarm}

`/whisk.system/alarms` 包可用于按指定的频率触发触发器。对于设置重复发生的作业或任务（例如，每小时调用一次系统备份操作）来说，警报非常有用。
{: shortdesc}

此包中包含以下订阅源。

| 实体| 类型| 参数| 描述
|
| --- | --- | --- | --- |
| `/whisk.system/alarms`| 包| - | 警报和定期实用程序。|
| `/whisk.system/alarms/interval`| 订阅源| minutes、trigger_payload、startDate 和 stopDate| 根据基于时间间隔的计划触发触发器事件。|
| `/whisk.system/alarms/once`| 订阅源| date 和 trigger_payload| 在特定日期触发一次触发器事件。|
| `/whisk.system/alarms/alarm`| 订阅源| cron、trigger_payload、startDate 和 stopDate| 使用 cron 根据基于时间的计划触发触发器事件。|


## 根据基于时间间隔的计划定期触发触发器事件
{: #openwhisk_catalog_alarm_fire}

`/whisk.system/alarms/interval` 订阅源将警报服务配置为根据基于时间间隔的计划触发触发器事件。参数如下所示：

- `minutes`：表示触发器触发时间间隔长度（以分钟为单位）的整数。

- `trigger_payload`：此参数的值成为每次触发器触发时触发器的内容。

- `startDate`：触发第一个触发器的日期。后续的触发将根据 `minutes` 参数指定的时间间隔长度发生。   

- `stopDate`：将停止运行触发器的日期。达到此日期后，即不再触发触发器。

  **注**：`startDate` 和 `stopDate` 参数支持整数或字符串值。整数值表示自 1970 年 1 月 1 日 00:00:00 UTC 以来的毫秒数，字符串值应该采用 ISO 8601 格式 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)。

以下示例创建每 2 分钟触发一次的触发器。触发器会尽快触发，并且将在 2019 年 1 月 31 日 23:59:00 UTC 停止触发。

  ```
  wsk trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

每个生成的事件都包含参数，这些参数是由 `trigger_payload` 值指定的属性。在本例中，每个触发器事件都包含参数 `name=Odin` 和 `place=Asgard`。

## 触发一次触发器事件  

`/whisk.system/alarms/once` 订阅源将警报服务配置为在指定日期触发触发器事件。参数如下所示：

- `date`：将触发触发器的日期。触发器仅在给定时间触发一次。 

  **注**：`date` 参数支持整数或字符串值。整数值表示自 1970 年 1 月 1 日 00:00:00 UTC 以来的毫秒数，字符串值应该采用 ISO 8601 格式 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)。

- `trigger_payload`：此参数的值成为触发器触发时触发器的内容。 

下面是创建触发器的示例，此触发器将在 2017 年 12 月 25 日 12:30:00 UTC 触发一次。

  ```
  wsk trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2017-12-25T12:30:00.000Z"
  ```
  {: pre}
    
## 使用 cron 根据基于时间的计划触发触发器

`/whisk.system/alarms/alarm` 订阅源将警报服务配置为按指定的频率触发触发器事件。参数如下所示：

- `cron`：字符串，基于 UNIX crontab 语法，用于指示何时触发触发器，时间采用全球标准时间 (UTC)。此字符串是 5 个字段组成的序列，字段之间用空格分隔：`X X X X X`。有关更多信息，请参阅：http://crontab.org。以下字符串是使用不同持续时间频率的示例。

  - `* * * * *`：触发器在每分钟开始时触发。
  - `0 * * * *`：触发器在每小时开始时触发。
  - `0 */2 * * *`：触发器每两小时（即 02:00:00、04:00:00、...）触发一次。
  - `0 9 8 * *`：触发器在每个月第 8 天上午 9:00:00 (UTC) 触发。

  **注**：`cron` 参数仅支持 5 个字段。
    
- `trigger_payload`：此参数的值成为每次触发器触发时触发器的内容。

- `startDate`：将开始运行触发器的日期。触发器将根据 cron 参数指定的计划触发。  

- `stopDate`：将停止运行触发器的日期。达到此日期后，即不再触发触发器。

  **注**：`startDate` 和 `stopDate` 参数支持整数或字符串值。整数值表示自 1970 年 1 月 1 日 00:00:00 UTC 以来的毫秒数，字符串值应该采用 ISO 8601 格式 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)。

下面是通过触发器事件中的 `name` 和 `place` 值创建每 2 分钟触发一次的触发器的示例。触发器直到 2019 年 1 月 1 日 00:00:00 UTC 才会开始触发，并且将在 2019 年 1 月 31 日 23:59:00 UTC 停止触发。

  ```
  wsk trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **注**：不推荐使用 `maxTriggers` 参数，该参数很快会除去。要停止触发器，请使用 `stopDate` 参数。

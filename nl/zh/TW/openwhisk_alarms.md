---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 使用 Alarm 套件
{: #openwhisk_catalog_alarm}

`/whisk.system/alarms` 套件可以用來依指定的頻率發動觸發程式。警示對於設定循環工作或作業很有用，例如每小時呼叫系統備份「動作」。
{: shortdesc}

套件包括下列資訊來源。

| 實體| 類型| 參數| 說明
|
| --- | --- | --- | --- |
| `/whisk.system/alarms` | 套件| - | 警示和定期公用程式。|
| `/whisk.system/alarms/interval` | 資訊來源| minutes, trigger_payload, startDate, stopDate | 根據間隔型排程發動「觸發程式」事件。|
| `/whisk.system/alarms/once` | 資訊來源| date, trigger_payload | 在特定日期發動「觸發程式」事件一次。|
| `/whisk.system/alarms/alarm` | 資訊來源| cron, trigger_payload, startDate, stopDate | 使用 cron 在時間型排程上發動「觸發程式」事件。|


## 根據間隔型排程定期發動觸發程式事件
{: #openwhisk_catalog_alarm_fire}

`/whisk.system/alarms/interval` 資訊來源會配置「警示」服務，以根據間隔型排程發動「觸發程式」事件。參數如下所示：

- `minutes`：此為整數，代表觸發程式發動之間的間隔長度（以分鐘為單位）。

- `trigger_payload`：每次發動「觸發程式」時，此參數的值都會變成「觸發程式」的內容。

- `startDate`：將發動第一個觸發程式的日期。根據 `minutes` 參數所指定的間隔長度，發生後續的發動。   

- `stopDate`：觸發程式停止執行的日期。到達此日期之後，即不再發動觸發程式。

  **附註**：`startDate` 和 `stopDate` 參數支援整數或字串值。整數值代表 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值應該是 ISO 8601 格式 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)。

下列範例會建立每 2 分鐘發動一次的觸發程式。觸發程式會盡快發動，並將在 2019 年 1 月 31 日 23:59:00 UTC 停止發動。

  ```
  wsk trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

每一個產生的事件都包含參數，這些參數是 `trigger_payload` 值所指定的內容。在此情況下，每個「觸發程式」事件都有 `name=Odin` 和 `place=Asgard` 參數。

## 發動觸發程式事件一次  

`/whisk.system/alarms/once` 資訊來源會配置「警示」服務，以在指定的日期發動觸發程式事件。參數如下所示：

- `date`：發動觸發程式的日期。觸發程式在給定的時間只會發動一次。 

  **附註**：`date` 參數支援整數或字串值。整數值代表 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值應該是 ISO 8601 格式 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)。

- `trigger_payload`：發動「觸發程式」時，此參數的值會變成「觸發程式」的內容。 

下列範例說明如何建立一個將在 2017 年 12 月 25 日 12:30:00 UTC 發動一次的觸發程式。

  ```
  wsk trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2017-12-25T12:30:00.000Z"
  ```
  {: pre}
    
## 使用 cron 在時間型排程上發動觸發程式

`/whisk.system/alarms/alarm` 資訊來源會配置「警示」服務，以指定的頻率發動「觸發程式」事件。參數如下所示：

- `cron`：根據 UNIX crontab 語法，指出何時（世界標準時間，UTC）發動「觸發程式」的字串。字串是五個欄位的序列，以空格區隔：`X X X X X`。
如需相關資訊，請參閱：http://crontab.org。下列字串是使用不同頻率期間的範例。

  - `* * * * *`：「觸發程式」在每分鐘整分發動。
  - `0 * * * *`：「觸發程式」在每小時整點發動。
  - `0 */2 * * *`：「觸發程式」每隔 2 小時發動（亦即 02:00:00、04:00:00）。..).
  - `0 9 8 * *`：「觸發程式」在每個月的第八天上午 9:00:00 (UTC) 發動。

  **附註**：參數 `cron` 只支援 5 個欄位。
    
- `trigger_payload`：每次發動「觸發程式」時，此參數的值都會變成「觸發程式」的內容。

- `startDate`：「觸發程式」開始執行的日期。「觸發程式」會根據 cron 參數所指定的排程來發動。  

- `stopDate`：「觸發程式」停止執行的日期。到達此日期之後，便不再發動觸發程式。

  **附註**：`startDate` 和 `stopDate` 參數支援整數或字串值。整數值代表 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值應該是 ISO 8601 格式 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15)。

下列範例說明如何使用觸發程式事件中的 `name` 及 `place` 值來建立每 2 分鐘發動一次的觸發程式。觸發程式在 2019 年 1 月 1 日 00:00:00 UTC 之前都不會開始發動，並將在 2019 年 1 月 31 日 23:59:00 UTC 停止發動。

  ```
  wsk trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **附註**：已淘汰參數 `maxTriggers`，即將予以移除。若要停止「觸發程式」，請使用 `stopDate` 參數。

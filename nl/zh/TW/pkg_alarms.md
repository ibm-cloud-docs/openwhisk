---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: alarms, serverless, triggers, functions

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



# 警示
{: #pkg_alarms}

`/whisk.system/alarms` 套件可以用來依指定的頻率發動觸發程式。對於設定重複發生的工作或作業（例如，每小時呼叫一次系統備份）來說，警示非常有用。
{: shortdesc}

套件包括下列資訊來源。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
|`/whisk.system/alarms` |套件| - |警示和定期公用程式。|
|`/whisk.system/alarms/once` |資訊來源|`date`、`trigger_payload`、`deleteAfterFire`|在特定日期發動觸發程式事件一次。|
|`/whisk.system/alarms/interval` |資訊來源|`minutes`、`trigger_payload`、`startDate`、`stopDate`| 根據間隔型排程發動觸發程式事件。|
|`/whisk.system/alarms/alarm` |資訊來源|`cron`、`timezone`、`trigger_payload`、`startDate`、`stopDate`|使用 cron 根據時間型的計劃發動觸發程式事件。|



## 發動觸發程式事件一次
{: #pkg_alarms_one}

`/whisk.system/alarms/once` 資訊來源會配置「警示」服務，以在指定的日期發動觸發程式事件。若要建立發動一次的警示，請執行下列指令。

```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

<table>
<caption>瞭解 <code>trigger create fireOnce</code> 指令元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>trigger create fireOnce</code> 指令元件</th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>您要建立的警示觸發程式類型。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>fireOnce 資訊來源的警示套件檔案路徑。</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>將 <code>&lt;date&gt;</code> 取代為計劃發動觸發程式的日期。觸發程式在指定的時間觸發一次。`date` 參數支援整數值或字串值。整數值代表自 `1970 年 1 月 1 日 00:00:00` UTC 以來的毫秒數，字串值必須採用 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>（選用）將 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 取代為發動觸發程式時的觸發程式參數。</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>（選用）觸發程式發動之後，是否要刪除觸發程式及任何關聯的規則。將 <code>&lt;delete_option&gt;</code> 取代為下列其中一項。<ul><li><code>false</code> -（預設值）：觸發程式發動之後不採取任何動作。</li><li><code>true</code> - 在觸發程式發動之後，會刪除觸發程式。</li><li><code>rules</code> - 在發動觸發程式之後，會刪除觸發程式及其所有關聯規則。</li></ul></td>
</tr>
</tbody></table>

下列指令是建立觸發程式的範例，此觸發程式將在 2019 年 12 月 25 日 12:30:00 UTC 觸發一次。每一個觸發程式事件都有 `name=Odin` 和 `place=Asgard` 參數。觸發程式觸發後，將刪除該觸發程式和所有關聯的規則。

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}


## 根據間隔型排程定期發動觸發程式事件
{: #pkg_alarms_int}

`/whisk.system/alarms/interval` 資訊來源會配置「警示」服務，以根據間隔型排程發動觸發程式事件。若要建立時間間隔型的警示，請執行下列指令。
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>瞭解 <code>trigger create interval</code> 指令元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>trigger create interval</code> 指令元件</th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>您要建立的警示觸發程式類型。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>interval 資訊來源的警示套件檔案路徑。</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>將 <code>&lt;minutes&gt;</code> 取代為整數，用於代表觸發程式發動的時間間隔長度（以分鐘為單位）。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>（選用）將 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 取代為發動觸發程式時的觸發程式參數。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>（選用）將 <code>&lt;startDate&gt;</code> 取代為計劃觸發第一個觸發程式發動的日期。根據 minutes 參數指定的間隔長度，進行後續的發動。此參數支援整數或字串值。整數值代表自 `1 January 1970 00:00:00` UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>（選用）將 <code>&lt;stopDate&gt;</code> 取代為計劃停止觸發程式的日期。到達此日期之後，就不會發動觸發程式。此參數支援整數或字串值。整數值代表自 `1 January 1970 00:00:00` UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
</tbody></table>

下列範例會建立每 2 分鐘發動一次的觸發程式。觸發程式會盡快發動，並且將在 `2019 年 1 月 31 日 23:59:00` UTC 停止發動。每一個觸發程式事件都有 `name=Odin` 和 `place=Asgard` 參數。

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## 使用 cron 根據時間型的排程發動觸發程式
{: #pkg_alarms_cron}

`/whisk.system/alarms/alarm` 資訊來源會配置「警示」服務依指定的頻率發動觸發程式事件。若要建立時間型的警示，請執行下列指令。
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>瞭解 <code>trigger create periodic</code> 指令元件</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>trigger create periodic</code> 指令元件</th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>您要建立的警示觸發程式類型。</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>定期警示資訊來源的警示套件檔案路徑。</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>將 <code>&lt;cron&gt;</code> 取代為以「世界標準時間 (UTC)」指出何時發動觸發程式的字串。字串基於 <a href="http://crontab.org">UNIX crontab 語法</a>，並且是最多包含 5 個欄位的序列。欄位之間用空格分隔，格式為 <code>X X X X X</code>。下列字串是使用不同頻率持續時間的範例。<ul><li><code>\* \* \* \* \*</code> - 觸發程式在每分鐘開始時發動。</li><li><code>0 \* \* \* \*</code> - 觸發程式在每小時開始時發動。</li><li><code>0 \*/2 \* \* \*</code> - 觸發程式每 2 小時發動一次（即，02:00:00、04:00:00、...).</li><li><code>0 9 8 \* \*</code> - 觸發程式在每個月 8 號上午 9:00:00 (UTC) 發動。</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>（選用）將 <code>&lt;timezone&gt;</code> 取代為用於指定時區的字串。發動觸發程式的實際時間將相對於指定的時區進行修改。如果時區無效，則會擲出錯誤。可以在 [Moment Timezone 網站](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}中檢查可用的所有時區。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>（選用）將 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 取代為發動觸發程式時的觸發程式參數。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>（選用）將 <code>&lt;startDate&gt;</code> 取代為計劃觸發第一個觸發程式的日期。根據 minutes 參數指定的間隔長度，進行後續的發動。此參數支援整數或字串值。整數值代表自 `1 January 1970 00:00:00` UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>（選用）將 <code>&lt;stopDate&gt;</code> 取代為希望觸發程式停止執行的日期。到達此日期之後，就不會發動觸發程式。此參數支援整數或字串值。整數值代表自 `1 January 1970 00:00:00` UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
</tbody></table>

下列指令是建立每 2 分鐘觸發一次的觸發程式的範例。觸發程式直到 `2019 年 1 月 1 日 00:00:00` UTC 才會開始發動，並且將在 `2019 年 1 月 31 日 23:59:00` UTC 停止發動。每一個觸發程式事件都有 `name=Odin` 和 `place=Asgard` 參數。

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}





---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: alarms, serverless

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


# 警示
{: #pkg_alarms}

`/whisk.system/alarms` 套件可以用來依指定的頻率發動觸發程式。警示對於設定循環工作或作業很有用，例如每小時呼叫系統備份動作。
{: shortdesc}

套件包括下列資訊來源。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
|`/whisk.system/alarms` |套件| - |警示和定期公用程式。|
|`/whisk.system/alarms/once` |資訊來源| date, trigger_payload, deleteAfterFire |在特定日期發動觸發程式事件一次。|
|`/whisk.system/alarms/interval` |資訊來源|minutes, trigger_payload, startDate, stopDate | 根據間隔型排程發動觸發程式事件。|
|`/whisk.system/alarms/alarm` |資訊來源| cron、timezone、trigger_payload、startDate、stopDate |使用 cron 根據時間型排程發動觸發程式事件。|



## 發動觸發程式事件一次
{: #pkg_alarms_one}

`/whisk.system/alarms/once` 資訊來源會配置「警示」服務，以在指定的日期發動觸發程式事件。若要建立「發動一次」警示，請執行下列指令：
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
<td>將 <code>&lt;date&gt;</code> 取代為發動觸發程式的日期。觸發程式在指定的時間只會發動一次。附註：`date` 參數支援整數或字串值。整數值代表自 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>選用項目：將 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 取代為發動觸發程式時的觸發程式參數。</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>選用項目：觸發程式發動之後，是否要刪除觸發程式及任何關聯的規則。將 <code>&lt;delete_option&gt;</code> 取代為下列其中一項：<ul><li><code>false</code>（預設值）：觸發程式發動之後不採取任何動作。</li><li><code>true</code>：在觸發程式發動之後，會刪除觸發程式。</li><li><code>rules</code>：在發動觸發程式之後，會刪除觸發程式及其所有關聯規則。</li></ul></td>
</tr>
</tbody></table>

下列範例說明如何建立一個將在 2019 年 12 月 25 日 12:30:00 UTC 發動一次的觸發程式。每一個觸發程式事件都有 `name=Odin` 和 `place=Asgard` 參數。觸發程式發動之後，將刪除觸發程式及所有關聯的規則。

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

`/whisk.system/alarms/interval` 資訊來源會配置「警示」服務，以根據間隔型排程發動觸發程式事件。若要建立間隔型警示，請執行下列指令：
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
<td>間隔資訊來源的警示套件檔案路徑。</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>將 <code>&lt;minutes&gt;</code> 取代為代表觸發程式發動之間的間隔長度（分鐘）的整數。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>選用項目：將 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 取代為發動觸發程式時的觸發程式參數。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>選用項目：將 <code>&lt;startDate&gt;</code> 取代為發動第一個觸發程式的日期。根據分鐘參數指定的間隔長度，進行後續的發動。附註：此參數支援整數或字串值。整數值代表自 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>選用項目：將 <code>&lt;stopDate&gt;</code> 取代為觸發程式停止執行的日期。到達此日期之後，就不會發動觸發程式。附註：此參數支援整數或字串值。整數值代表自 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
</tbody></table>

下列範例會建立每 2 分鐘發動一次的觸發程式。觸發程式會盡快發動，並將在 2019 年 1 月 31 日 23:59:00 UTC 停止發動。每一個觸發程式事件都有 `name=Odin` 和 `place=Asgard` 參數。

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## 使用 cron 根據時間型排程發動觸發程式
{: #pkg_alarms_cron}

`/whisk.system/alarms/alarm` 資訊來源會配置「警示」服務依指定的頻率發動觸發程式事件。若要建立時間型警示，請執行下列指令：
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
<td>將 <code>&lt;cron&gt;</code> 取代為以「世界標準時間 (UTC)」指出何時發動觸發程式的字串。該字串是根據 <a href="http://crontab.org">UNIX crontab 語法</a>，且是一個上限為 5 個欄位的序列。依 <code>X X X X X</code> 格式，以空格區隔欄位。下列字串是使用不同頻率期間的範例：<ul><li><code>\* \* \* \* \*</code>：觸發程式在每分鐘準時發動。</li><li><code>0 \* \* \* \*</code>：觸發程式在每小時準時發動。</li><li><code>0 \*/2 \* \* \*</code>：觸發程式每 2 小時觸發一次（即，02:00:00、04:00:00、...).</li><li><code>0 9 8 \* \*</code>：觸發程式在每月第八天上午 9:00:00 (UTC) 發動。</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>選用項目：將 <code>&lt;timezone&gt;</code> 取代為指定時區的字串。然後，會根據指定的時區來修改發動觸發程式的實際時間。如果時區無效，則會擲出錯誤。可以在 [Moment Timezone 網站](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names)中檢查可用的所有時區。</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>選用項目：將 <code>&lt;key&gt;</code> 和 <code>&lt;value&gt;</code> 取代為發動觸發程式時的觸發程式參數。</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>選用項目：將 <code>&lt;startDate&gt;</code> 取代為發動第一個觸發程式的日期。根據分鐘參數指定的間隔長度，進行後續的發動。附註：此參數支援整數或字串值。整數值代表自 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>選用項目：將 <code>&lt;stopDate&gt;</code> 取代為觸發程式停止執行的日期。到達此日期之後，就不會發動觸發程式。附註：此參數支援整數或字串值。整數值代表自 1970 年 1 月 1 日 00:00:00 UTC 之後的毫秒數，字串值必須為 <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601 格式</a>。</td>
</tr>
</tbody></table>

下列範例會建立每 2 分鐘發動一次的觸發程式。觸發程式在 2019 年 1 月 1 日 00:00:00 UTC 之前都不會開始發動，並將在 2019 年 1 月 31 日 23:59:00 UTC 停止發動。每一個觸發程式事件都有 `name=Odin` 和 `place=Asgard` 參數。

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



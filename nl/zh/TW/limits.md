---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions, functions

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


# 系統詳細資料及限制
{: #limits}

下列各節提供有關 {{site.data.keyword.openwhisk}} 系統及限制設定的技術詳細資料。
{: shortdesc}

## 系統限制
{: #limits_syslimits}

### 動作
{: #limits_actions}

{{site.data.keyword.openwhisk_short}} 有一些系統限制，包括動作可以使用的記憶體數量，以及每分鐘容許的動作呼叫次數。

下表列出動作的預設限制。

|限制 |說明|預設值 |最小值 |最大值|
| --- | ---| --- | --- | --- |
|`codeSize` |動作的程式碼大小上限為 48 MB。對於 JavaScript 動作，使用工具將所有原始碼（包括相依關係）連結至單一組合檔。此限制是固定的，無法變更。|48|1|48| 
|`concurrent`|名稱空間的執行中或置入佇列等待執行的啟動次數不得超過 1000。此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請參閱[增加固定限制](/docs/openwhisk?topic=cloud-functions-limits#limits_increase)，以取得有關如何增加此限制的指示。|1000|1|1000* |
|`logs`|日誌限制 N 在範圍 [0 MB..10 MB] 內，並根據每個動作來設定。建立或更新動作時，使用者可以變更動作日誌限制。超過設定限制的日誌會被截斷，因此會忽略任何新的日誌項目，且會新增一則警告作為最後一個啟動輸出，指出啟動已超出設定的日誌限制。|10|0 |10|
|`memory`	|記憶體限制 M 在範圍 [128 MB..2048 MB] 內，並根據每個動作來設定 (MB)。建立動作時，使用者可以變更記憶體限制。容器使用的記憶體數量不能超過限制所配置的記憶體數量。|256|128 | 2048 |
|`minuteRate` |每個名稱空間每分鐘可以提交的啟動次數不得超過 N 次。速率限制 N 設定為 5000，並限制一分鐘時間範圍內的動作呼叫次數。超過此限制的 CLI 或 API 呼叫會收到與 HTTP 狀態碼 `429: TOO MANY REQUESTS` 對應的錯誤碼。此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請參閱[增加固定限制](#limits_increase)，以取得有關如何增加此限制的指示。|5000|1|5000* | 
|`openulimit` |動作的開啟檔案數上限為 1024（同時適用於硬性和軟性限制）。此限制是固定的，無法變更。呼叫動作時，docker run 指令會使用 `--ulimit nofile=1024:1024` 引數來設定 `openulimit` 值。如需相關資訊，請參閱 [docker run](https://docs.docker.com/engine/reference/commandline/run/){: external} 指令行參考文件。| 1024 |0 | 1024 | 
|`parameters` |可以附加的參數大小上限 (MB)。在建立或更新「動作/套件/觸發程式」時，總參數的大小限制是 5 MB。嘗試建立或更新參數太大的實體時，會拒絕該實體。此限制是固定的，無法變更。| 5 |0 | 5 | 
|`proculimit` |動作容器可用的處理程序數目上限為 1024。此限制是固定的，無法變更。呼叫動作時，docker run 指令會使用 `--pids-limit 1024` 引數來設定 `proculimit` 值。如需相關資訊，請參閱 [docker run](https://docs.docker.com/engine/reference/commandline/run/){: external} 指令行參考文件。| 1024 |0 | 1024 | 
|`result` |動作呼叫結果的輸出大小上限 (MB)。此限制是固定的，無法變更。| 5 |0 | 5 | 
| `sequenceMaxActions` |包含序列的動作數目上限。此限制是固定的，無法變更。| 50 |0 | 50* | 
|`timeout`	|逾時限制 N 在範圍 [100 毫秒..600000 毫秒] 內，並根據每個動作來設定（毫秒）。建立動作時，使用者可以變更逾時限制。終止執行時間超過 N 毫秒的容器。|60000|100 | 600000 | 

### 增加固定限制
{: #limits_increase}

以 (*) 結尾的限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加值。如果您想要增加限制值，請直接從 IBM [{{site.data.keyword.openwhisk_short}} Web 主控台](https://cloud.ibm.com/openwhisk){: external}來開啟問題單，以聯絡 IBM 支援中心。
  1. 選取**支援**
  2. 從下拉功能表中選取**新增問題單**。
  3. 針對問題單類型，請選取**技術**。
  4. 針對支援的技術領域，請選取**函數**。

### 觸發程式
{: #limits_triggers}

觸發程式受限於每分鐘的發動率，如下表所記載。

|限制 |說明|預設值 |最小值 |最大值|
| --- | --- | --- | --- | --- |
|`minuteRate` |速率限制 N 設定為 5000，並限制使用者在一分鐘時間範圍內可發動的觸發程式數目。建立觸發程式時，使用者無法變更觸發程式限制。超過此限制的 CLI 或 API 呼叫會收到與 HTTP 狀態碼 `429: TOO MANY REQUESTS` 對應的錯誤碼。此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](#limits_triggersfixed)一節，以取得如何增加此限制的詳細指示。|5000* |5000* |5000* |

### 增加固定限制
{: #limits_triggersfixed}

以 (*) 結尾的限制值是固定值，但如果業務案例可以證明使用更高的安全限制值是合理的，則可以增加這些值。如果您想要增加限制值，請直接從 IBM [{{site.data.keyword.openwhisk_short}} Web 主控台](https://cloud.ibm.com/openwhisk){: external}來開啟問題單，以聯絡 IBM 支援中心。
  1. 選取**支援**
  2. 從下拉功能表中選取**新增問題單**。
  3. 針對問題單類型，請選取**技術**。
  4. 針對支援的技術領域，請選取**函數**。


## {{site.data.keyword.openwhisk_short}} 實體
{: #limits_entities_ov}

### 名稱空間及套件
{: #limits_namespaces}

{{site.data.keyword.openwhisk_short}} 的動作、觸發程式及規則屬於名稱空間，但有時屬於套件。

套件可以包含動作及資訊來源。套件不能包含另一個套件，因此不容許巢狀套件。實體也不需要包含在套件中。

可以透過執行 `ibmcloud fn namespace create` 來建立以 IAM 為基礎的新名稱空間。以 Cloud Foundry 為基礎的名稱空間是由組織和空間名稱的組合構成的。例如，如果將 `user@email.com` 組織和 `dev` 空間設定為目標，即等於將名為 `user@email.com_dev` 且以 {{site.data.keyword.openwhisk_short}} Cloud Foundry 為基礎的名稱空間設定為目標。 

`/whisk.system` 名稱空間是保留供隨 {{site.data.keyword.openwhisk_short}} 系統一起配送的實體使用。

以 IAM 為基礎的名稱空間不支援[無伺服器架構](https://serverless.com/)
{: note}


### 完整名稱
{: #limits_fullnames}

實體的完整名稱是 `/namespaceName/[packageName]/entityName`。請注意，`/` 用來區隔名稱空間、套件及實體。名稱空間的前面也必須加上 `/`。

為了方便起見，如果名稱空間是使用者的預設名稱空間，則可以將其省略。例如，假設使用者的預設名稱空間為 `/myOrg`。下列範例說明一些實體的完整名稱及其別名。

|完整名稱|別名|名稱空間|套件|名稱|
| --- | --- | --- | --- | --- |
|`/whisk.system/cloudant/read` |  |`/whisk.system` |`cloudant` |`read` |
|`/myOrg/video/transcode` |`video/transcode` |`/myOrg` |`video` |`transcode` |
|`/myOrg/filter` |`filter` |`/myOrg` |  |`filter` |

在其他位置中使用 {{site.data.keyword.openwhisk_short}} CLI 時，您可以使用此命名方法。

更精確地來說，名稱必須符合下列正規表示式（以 Java meta 字元語法表示）：`\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`。

## 動作語意
{: #limits_semantics}

下列各節說明 {{site.data.keyword.openwhisk_short}} 動作的詳細資料。

### 無狀態
{: #limits_stateless}

動作實作是 Stateless 或 *idmpotent*。雖然系統不會強制執行此內容，但是不保證透過動作所維護的任何狀態都可在呼叫時使用。

此外，動作可能會有多個說明實例，每一個說明實例都有自己的狀態。動作呼叫可能會被分派至所有這些說明實例。

### 呼叫輸入及輸出
{: #limits_invocationio}

動作的輸入及輸出是鍵值組的字典。索引鍵是字串，而值是有效的 JSON 值。

### 動作的呼叫排序
{: #limits_ordering}

動作不是依序進行呼叫。如果使用者從指令行或 REST API 呼叫動作兩次，則可能會先執行第二次呼叫，再執行第一次呼叫。如果動作有負面影響，則可能會依任意順序觀察到它們。

此外，不保證動作會自動執行。可以同時執行兩個動作，而其負面影響可能會交錯。OpenWhisk 無法確保任何特定並行一致性模型是否有負面影響。 

### 動作執行
{: #limits_exec}

收到呼叫要求時，系統會記錄要求，並分派啟動。

系統會傳回啟動 ID（使用非區塊處理呼叫）確認已接收到該啟動 ID。
如果在收到 HTTP 回應之前發生網路失敗或其他干擾失敗，則可能是 {{site.data.keyword.openwhisk_short}} 已接收並處理該要求。

系統嘗試呼叫該動作一次，而導致下列四種結果的其中一種：

|結果|說明|
| --- | --- |
|`success`|動作呼叫順利完成。|
|`application error`|動作呼叫成功，但動作有意傳回了錯誤值，例如由於不符合引數上的前置條件。|
|`action developer error`|動作已呼叫，但以異常方式完成，例如動作未偵測到異常或存在語法錯誤。|
|`whisk internal error`|系統無法呼叫該操作。結果會記錄在啟動記錄的 `status` 欄位中（如下節所記載）。|

每個順利收到的呼叫以及每個可能向使用者收費的呼叫，都有一筆啟動記錄。

當結果是*動作開發人員錯誤* 時，可能會局部執行動作，並產生外部可見的負面影響。使用者應負責檢查是否實際發生了此類負面影響，然後發出重試邏輯。某些 *Whisk 內部錯誤* 指出動作開始執行，但在動作完成登錄之前就失敗了。

## 啟動記錄
{: #limits_activation}

每一個動作呼叫及觸發程式發動都會導致啟動記錄。

啟動記錄包含下列欄位：

|欄位|說明
| --- | --- |
|`activationId`|啟動 ID。|
|`start` 和 `end`|記錄啟動開始時間和結束時間的時間戳記。值為 [UNIX 時間格式](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15){: external}。|
|`namespace` 和 `name`|實體的名稱空間和實體名稱。|
|`logs` |字串陣列，其中包含在動作啟動期間由動作產生的日誌。每一個陣列元素都對應至動作之 `stdout` 或 `stderr` 的行輸出，並且包含日誌輸出的時間及串流。結構如下：`TIMESTAMP STREAM: LOG_OUTPUT`。|
|`response`|用於定義 `success`、`status` 和 `result` 鍵的字典。`status`：啟動結果，可能是下列其中一個值："success"、"application error"、"action developer error"、"whisk internal error"。`success`：假設並且只有在狀態為 "`success`" 時，此項為 `true`。|
|`result` |包含啟動結果的字典。如果順利啟動，則結果會包含動作所傳回的值。如果啟動失敗，`result` 會包含 `error` 索引鍵，通常還會有失敗的說明。|





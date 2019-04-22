---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# 系統詳細資料及限制
{: #openwhisk_reference}

下列各節提供有關 {{site.data.keyword.openwhisk}} 系統及限制設定的技術詳細資料。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 實體
{: #openwhisk_entities}

### 名稱空間及套件
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} 的動作、觸發程式及規則屬於名稱空間，但有時屬於套件。

套件可以包含動作及資訊來源。套件不能包含另一個套件，因此不容許巢狀套件。實體也不需要包含在套件中。

在 {{site.data.keyword.Bluemix_notm}} 中，組織+空間配對對應至 {{site.data.keyword.openwhisk_short}} 名稱空間。例如，組織 `BobsOrg` 及空間 `dev` 將對應至 {{site.data.keyword.openwhisk_short}} 名稱空間 `/BobsOrg_dev`。



您可以[建立 Cloud Foundry 組織和空間](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#region_info)，以建立新的 Cloud Foundry 型名稱空間。`/whisk.system` 名稱空間是保留供隨 {{site.data.keyword.openwhisk_short}} 系統一起配送的實體使用。


### 完整名稱
{: #openwhisk_entities_fullyqual}

實體的完整名稱是 `/namespaceName/[packageName]/entityName`。請注意，`/` 用來區隔名稱空間、套件及實體。名稱空間的前面也必須加上 `/`。

為了方便起見，如果是使用者的預設名稱空間，則「名稱空間」可以保留。例如，假設有一個使用者的預設「名稱空間」是 `/myOrg`。下列範例說明一些實體的完整名稱及其別名。



|完整名稱|別名|名稱空間|套件|名稱|
| --- | --- | --- | --- | --- |
|`/whisk.system/cloudant/read` |  |`/whisk.system` |`cloudant` |`read` |
|`/myOrg/video/transcode` |`video/transcode` |`/myOrg` |`video` |`transcode` |
|`/myOrg/filter` |`filter` |`/myOrg` |  |`filter` |

在其他位置中使用 {{site.data.keyword.openwhisk_short}} CLI 時，您可以使用此命名方法。

### 實體名稱
{: #openwhisk_entities_names}

所有實體（包括動作、觸發程式、規則、套件及名稱空間）的名稱都是遵循下列格式的一串字元：

* 第一個字元必須是英數字元或底線。
* 後續字元可以是英數字元、空格或下列任何一值：`_`、`@`、`.`、`-`。
* 最後一個字元不能是空格。

更精確地來說，名稱必須符合下列正規表示式（以 Java meta 字元語法表示）：`\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`。

## 動作語意
{: #openwhisk_semantics}

下列各節說明 {{site.data.keyword.openwhisk_short}} 動作的詳細資料。

### 無狀態
{: #openwhisk_semantics_stateless}

動作實作是 Stateless 或 *idmpotent*。雖然系統不會強制執行此內容，但是不保證透過動作所維護的任何狀態都可在呼叫時使用。

此外，動作可能會有多個說明實例，每一個說明實例都有自己的狀態。動作呼叫可能會被分派至所有這些說明實例。

### 呼叫輸入及輸出
{: #openwhisk_semantics_invocationio}

動作的輸入及輸出是鍵值組的字典。索引鍵是字串，而值是有效的 JSON 值。

### 動作的呼叫排序
{: #openwhisk_ordering}

動作不是依序進行呼叫。如果使用者從指令行或 REST API 呼叫動作兩次，則可能會先執行第二次呼叫，再執行第一次呼叫。如果動作有負面影響，則可能會依任意順序觀察到它們。

此外，不保證動作會自動執行。可以同時執行兩個動作，而其負面影響可能會交錯。OpenWhisk 無法確保任何特定並行一致性模型是否有負面影響。任何並行性負面影響都是根據實作。

### 動作執行保證
{: #openwhisk_atmostonce}

收到呼叫要求時，系統會記錄要求，並分派啟動。

系統會傳回啟動 ID（使用非區塊處理呼叫）確認已接收到該啟動 ID。
如果在收到 HTTP 回應之前發生網路失敗或其他干擾失敗，則可能是 {{site.data.keyword.openwhisk_short}} 已接收並處理該要求。

系統嘗試呼叫該動作一次，而導致下列四種結果的其中一種：
- *成功*：順利完成動作呼叫。
- *應用程式錯誤*：動作呼叫成功，但動作故意傳回錯誤值（例如，因為引數的前置條件不相符）。
- *動作開發人員錯誤*：已呼叫動作，但異常完成（例如，動作偵測不到異常狀況，或有語法錯誤）。
- *Whisk 內部錯誤*：系統無法呼叫動作。結果會記錄在啟動記錄的 `status` 欄位中（如下節所記載）。

每個成功收到的呼叫以及每個可能向使用者收費的呼叫，都有一筆啟動記錄。

當結果是*動作開發人員錯誤* 時，可能會局部執行動作，並產生外部可見的負面影響。使用者必須負責檢查是否發生這類負面影響，如有需要，請發出重試邏輯。某些 *Whisk 內部錯誤* 指出動作開始執行，但在動作完成登錄之前就失敗了。

## 啟動記錄
{: #openwhisk_ref_activation}

每一個動作呼叫及觸發程式發動都會導致啟動記錄。

啟動記錄包含下列欄位：

- *activationId*：啟動 ID。
- *start* 及 *end*：記錄啟動開始及結束的時間戳記。值為 [UNIX 時間格式](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15)。
- *namespace* 及 `name`：實體的名稱空間及名稱。
- *logs*：字串陣列，內含動作在其啟動期間所產生的日誌。每一個陣列元素都對應至動作之 `stdout` 或 `stderr` 的行輸出，並且包含日誌輸出的時間及串流。結構如下：`TIMESTAMP STREAM: LOG_OUTPUT`。
- *response*：定義索引鍵 `success`、`status` 及 `result` 的字典：
  - *status*：啟動結果，可能是下列其中一個值："success"、"application error"、"action developer error"、"whisk internal error"。
  - *success*：假設並且只有在狀態為 `"success"` 時，才會為 `true`
- *result*：包含啟動結果的字典。如果順利啟動，則結果會包含動作所傳回的值。如果啟動失敗，`result` 會包含 `error` 索引鍵，通常還會有失敗的說明。

## REST API
{: #openwhisk_ref_restapi}

在 [REST API 參照](https://cloud.ibm.com/apidocs/functions)中，可以找到 {{site.data.keyword.openwhisk_short}} REST API 的相關資訊。

## 系統限制
{: #openwhisk_syslimits}

### 動作
{{site.data.keyword.openwhisk_short}} 有一些系統限制，包括動作可以使用的記憶體數量，以及每分鐘容許的動作呼叫次數。

下表列出動作的預設限制。

|限制 |說明|預設值 |最小值 |最大值|
| ----- | ----------- | :-------: | :---: | :---: |
|[codeSize](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_codesize) |動作碼的大小上限 (MB)。|48|1|48|
|[concurrent](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_concurrent) |每個名稱空間可以提交的執行中或置入佇列等待執行的啟動次數不得超過 N 次。|1000|1|1000* |
|[logs](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_logs) |不容許容器寫入 stdout 的內容超過 N MB。|10|0 |10|
|[memory](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_memory)	|不容許容器配置超過 N MB 的記憶體。|256|128 | 2048 |
|[minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_minuterate) |每個名稱空間每分鐘可以提交的啟動次數不得超過 N 次。|5000|1|5000* |
|[openulimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_openulimit) |動作的開啟檔案數上限。| 1024 |0 | 1024 |
|[parameters](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_parameters) |可以附加的參數大小上限 (MB)。| 5 |0 | 5 |
|[proculimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_proculimit) |動作可用的處理程序數目上限。| 1024 |0 | 1024 |
|[result](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_result) |動作呼叫結果的大小上限 (MB)。| 5 |0 | 5 |
| [sequenceMaxActions](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_sequencemax) |包含給定序列的動作數目上限。| 50 |0 | 50* |
|[timeout](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_timeout)	|不容許容器執行超過 N 毫秒的時間。|60000|100 | 600000 |

### 增加固定限制
{: #increase_fixed_limit}

以 (*) 結尾的限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加值。如果您想要增加限制值，請直接從 IBM [{{site.data.keyword.openwhisk_short}} Web 主控台](https://cloud.ibm.com/openwhisk)來開啟問題單，以聯絡 IBM 支援中心。
  1. 選取**支援**
  2. 從下拉功能表中選取**新增問題單**。
  3. 針對問題單類型，請選取**技術**。
  4. 針對支援的技術領域，請選取**函數**。

#### codeSize (MB)（固定：48 MB）
{: #openwhisk_syslimits_codesize}
* 動作的程式碼大小上限為 48 MB。
* 對於 JavaScript 動作，使用工具將所有原始碼（包括相依關係）連結至單一組合檔。
* 此限制是固定的，無法變更。

#### concurrent（固定：1000*）
{: #openwhisk_syslimits_concurrent}
* 名稱空間的執行中或置入佇列等待執行的啟動次數不得超過 1000。
* 此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit)一節，以取得如何增加此限制的詳細指示。

#### 日誌 (MB)（預設值：10 MB）
{: #openwhisk_syslimits_logs}
* 日誌限制 N 在範圍 [0 MB..10 MB] 內，並根據每個動作來設定。
* 建立或更新動作時，使用者可以變更動作日誌限制。
* 超過設定限制的日誌會被截斷，因此會忽略任何新的日誌項目，且會新增一則警告作為最後一個啟動輸出，指出啟動已超出設定的日誌限制。

#### 記憶體 (MB)（預設值：256 MB）
{: #openwhisk_syslimits_memory}
* 記憶體限制 M 在範圍 [128 MB..2048 MB] 內，並根據每個動作來設定 (MB)。
* 建立動作時，使用者可以變更記憶體限制。
* 容器使用的記憶體數量不能超過限制所配置的記憶體數量。

#### minuteRate（固定：5000*）
{: #openwhisk_syslimits_minuterate}
* 速率限制 N 設定為 5000，並限制一分鐘時間範圍內的動作呼叫次數。
* 超過此限制的 CLI 或 API 呼叫會收到與 HTTP 狀態碼 `429: TOO MANY REQUESTS` 對應的錯誤碼。
* 此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit)一節，以取得如何增加此限制的詳細指示。

#### openulimit（固定：1024:1024）
{: #openwhisk_syslimits_openulimit}
* 動作的開啟檔案數上限為 1024（同時適用於硬性和軟性限制）。
* 此限制是固定的，無法變更。
* 呼叫動作時，docker run 指令會使用 `--ulimit nofile=1024:1024` 引數來設定 `openulimit` 值。
* 如需相關資訊，請參閱 [docker run](https://docs.docker.com/engine/reference/commandline/run) 指令行參考文件。

#### 參數（固定：5 MB）
{: #openwhisk_syslimits_parameters}
* 在建立或更新「動作/套件/觸發程式」時，總參數的大小限制是 5 MB。
* 嘗試建立或更新參數太大的實體時，會拒絕該實體。
* 此限制是固定的，無法變更。

#### proculimit（固定：1024:1024）
{: #openwhisk_syslimits_proculimit}
* 動作容器可用的處理程序數目上限為 1024。
* 此限制是固定的，無法變更。
* 呼叫動作時，docker run 指令會使用 `--pids-limit 1024` 引數來設定 `proculimit` 值。
* 如需相關資訊，請參閱 [docker run](https://docs.docker.com/engine/reference/commandline/run) 指令行參考文件。

#### 結果（固定：5 MB）
{: #openwhisk_syslimits_result}
* 動作呼叫結果的輸出大小上限 (MB)。
* 此限制是固定的，無法變更。

#### sequenceMaxActions（固定：50*）
{: #openwhisk_syslimits_sequencemax}
* 包含給定序列的動作數目上限。
* 此限制是固定的，無法變更。

#### 逾時（毫秒）（預設值：60 秒）
{: #openwhisk_syslimits_timeout}
* 逾時限制 N 在範圍 [100 毫秒..600000 毫秒] 內，並根據每個動作來設定（毫秒）。
* 建立動作時，使用者可以變更逾時限制。
* 終止執行時間超過 N 毫秒的容器。

### 觸發程式

觸發程式受限於每分鐘的發動率，如下表所記載。

|限制 |說明|預設值 |最小值 |最大值|
| ----- | ----------- | :-------: | :---: | :---: |
|[minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_tminuterate) |每個名稱空間每分鐘的觸發程式發動次數不得超過 N 次。|5000* |5000* |5000* |

### 增加固定限制
{: #increase_fixed_tlimit}

以 (*) 結尾的限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加值。如果您想要增加限制值，請直接從 IBM [{{site.data.keyword.openwhisk_short}} Web 主控台](https://cloud.ibm.com/openwhisk)來開啟問題單，以聯絡 IBM 支援中心。
  1. 選取**支援**
  2. 從下拉功能表中選取**新增問題單**。
  3. 針對問題單類型，請選取**技術**。
  4. 針對支援的技術領域，請選取**函數**。

#### minuteRate（固定：5000*）
{: #openwhisk_syslimits_tminuterate}

* 速率限制 N 設定為 5000，並限制使用者在一分鐘時間範圍內可發動的觸發程式數目。
* 建立觸發程式時，使用者無法變更觸發程式限制。
* 超過此限制的 CLI 或 API 呼叫會收到與 HTTP 狀態碼 `429: TOO MANY REQUESTS` 對應的錯誤碼。
* 此限制值是固定的，但如果業務案例可以調整較高的安全限制值，則可以增加此值。請查閱[增加固定限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_tlimit)一節，以取得如何增加此限制的詳細指示。

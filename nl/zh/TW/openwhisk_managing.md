---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 管理動作
{: #openwhisk_managing}

藉由監視動作輸出、取得動作的特定資訊或刪除動作來管理動作。
{: shortdec}

## 取得動作
{: #getting-actions}

建立動作之後，您可以取得動作詳細資料的相關資訊，並列出您名稱空間中的動作。
{: shortdesc}

若要列出您已建立的所有動作，請執行下列指令：
```
ibmcloud fn action list
```
{: pre}

在建立其他動作時，將相關動作分組成[套件](./openwhisk_packages.html)會非常實用。若要將您的動作清單過濾成只有特定套件內的動作，請執行下列指令：
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

若要取得說明特定動作的 meta 資料，請執行下列指令：

```
ibmcloud fn action get hello
```
{: pre}

輸出範例：
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption>瞭解 <code>action get</code> 指令輸出</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>action get</code> 指令輸出</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>此動作所在的名稱空間。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>動作的名稱。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>動作的語意版本。</td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code>：動作的類型。可能的值為 nodejs:6、nodejs:8、php:7.1、python:3、python-jessie:3、swift:3.1.1、swift:4.1、java、blackbox 及 sequence。</li>
<li><code>code</code>：當類型為 nodejs 或 swift 時要執行的 Javascript 或 Swift 程式碼。</li>
<li><code>components</code>：當類型為 sequence 時在序列中的動作。會依序列出動作。</li>
<li><code>image</code>：當類型為 blackbox 時的容器映像檔名稱。</li>
<li><code>init</code>：當類型為 nodejs 時的選用壓縮檔 (zipfile) 參照。</li>
<li><code>binary</code>：是否將動作編譯成二進位執行檔。</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>此動作的註釋。如需可能的註釋清單，請參閱[動作註釋](openwhisk_annotations.html#action)和 [Web 動作註釋](openwhisk_annotations.html#annotations-specific-to-web-actions)參照主題。</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>：對動作設定在此期間之後即終止動作的逾時值（毫秒）。預設值：6000</li>
<li><code>memory</code>：對動作設定的記憶體上限 (MB)。預設值：256</li>
<li><code>logs</code>：對動作設定的日誌大小上限 (MB)。預設值：10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>是否公開發佈動作。</td>
</tr>
</tbody></table>

## 檢視啟動詳細資料
{: #activation}

其他使用者可呼叫 {{site.data.keyword.openwhisk_short}} 動作來回應各種事件，或是作為動作序列的一部分。每次呼叫動作時，都會建立該呼叫的啟動記錄。若要取得動作呼叫結果的相關資訊，您可以取得啟動的相關詳細資料。

若要取得名稱空間中的所有啟動記錄 ID，請執行下列指令：
```
ibmcloud fn activation list
```
{: pre}

若要取得有關從動作呼叫所產生之特定啟動記錄的詳細資料，請執行下列指令：
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

輸出範例：
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "BobsOrg_dev",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
  },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "BobsOrg_dev/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
  },
        {
            "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>瞭解 <code>activation get</code> 指令輸出</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>activation get</code> 指令輸出</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>此啟動所在的名稱空間。這可能與該動作所在的名稱空間不同。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>動作的名稱。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>動作的語意版本。</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>啟動項目的使用者帳戶。</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>此啟動記錄的 ID。</td>
</tr>
<tr>
<td><code>start</code></td>
<td>啟動開始的時間。</td>
</tr>
<tr>
<td><code>end
</code></td>
<td>啟動完成的時間。</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>完成啟動所花費的時間（毫秒）。</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>：啟動的結束狀態。</li>
<li><code>statusCode</code>：狀態碼。如果發生錯誤，則為 HTTP 錯誤碼。</li>
<li><code>success</code>：動作是否順利完成。</li>
<li><code>result</code>：來自啟動的回覆值。</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>此啟動的日誌。</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>此動作的註釋。如需可能的啟動註釋清單，請參閱[註釋參考主題](openwhisk_annotations.html#activation)。</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>是否公開發佈動作。</td>
</tr>
</tbody></table>

## 存取動作內文內的動作 meta 資料
{: #accessing-action-metadata-within-the-action-body}

動作環境包含執行中動作特有的數個內容。這些內容容許動作透過 REST API 以程式設計方式使用 OpenWhisk 資產，或設定在動作即將用完其分配到時間預算時的內部警示。使用 OpenWhisk Docker 架構時，可以在系統環境中存取所有受支援的運行環境的內容：Node.js、Python、Swift、Java 及 Docker。

| 內容 |說明|
| -------- | ----------- |
| `__OW_API_HOST` | 執行此動作之 OpenWhisk 部署的 API 主機。|
| `__OW_API_KEY` | 呼叫動作之主題的 API 金鑰，此金鑰可能是受限的 API 金鑰。|
| `__OW_NAMESPACE` | 啟動的名稱空間。此名稱空間可能與動作的名稱空間不同。|
| `__OW_ACTION_NAME` | 執行中動作的完整名稱。|
| `__OW_ACTIVATION_ID` | 此執行中動作實例的啟動 ID。|
| `__OW_DEADLINE` | 此動作將耗用其整個有效天數配額的大致時間（以新紀元毫秒為單位）。|

## 取得動作 URL
{: #get-action-url}

您可以透過 HTTPS 要求，使用 REST 介面來呼叫動作。
{: shortdesc}

若要取得動作 URL，請執行下列指令：
```
ibmcloud fn action get actionName --url
```
{: pre}

標準動作的輸出範例：
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

[Web 動作](./openwhisk_webactions.html)的輸出範例：
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**附註：**針對標準動作，透過 HTTPS 要求呼叫時，必須提供鑑別。如需使用 REST 介面來呼叫動作的相關資訊，請參閱 [REST API 參照](https://console.bluemix.net/apidocs/openwhisk)。

## 儲存動作碼
{: #save-action}

您可以取得並在本端儲存與現有動作相關聯的程式碼。您可以儲存所有動作的程式碼，但序列及 Docker 動作除外。
{: shortdesc}

將動作碼儲存至對應於現行工作目錄中現有動作名稱的檔名。
```
ibmcloud fn action get actionName --save
```
{: pre}

會使用對應於動作類型的副檔名。對於為 zip 檔的動作碼，會使用 .zip 副檔名。輸出範例：
```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
{: screen}

您可以改用 `--save-as` 旗標來提供自訂檔案路徑、檔名和副檔名。
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

輸出範例：
```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
{: screen}

## 監視動作日誌
{: #monitor-action-output}

其他使用者可呼叫 {{site.data.keyword.openwhisk_short}} 動作來回應各種事件，或是作為動作序列的一部分。若要取得何時呼叫動作及輸出內容的相關資訊，監視動作日誌會非常實用。

您可以使用 {{site.data.keyword.openwhisk_short}} CLI 來監看所呼叫動作的輸出。

1. 啟動輪詢迴圈，以連續檢查啟動日誌。
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. 切換至另一個視窗並呼叫動作。
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    輸出範例：
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. 在輪詢視窗中，您可以看到啟動日誌。
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
      2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    您也可以即時查看在 {{site.data.keyword.openwhisk_short}} 中代表您執行之任何動作的日誌。

## 刪除動作
{: #deleting-actions}

您可以藉由刪除不要使用的動作來進行清除。

1. 刪除動作。
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    輸出範例：
    ```
    ok: deleted hello
    ```
    {: screen}

2. 驗證動作不再出現於動作清單中。
    ```
    ibmcloud fn action list
    ```
    {: pre}

    輸出範例：
    ```
    actions
    ```
    {: screen}

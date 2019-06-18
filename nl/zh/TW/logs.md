---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

# 視圖日誌
{: #logs}

記載功能在 {{site.data.keyword.openwhisk}} 中會自動啟用，可協助您對問題進行疑難排解。您也可以使用 {{site.data.keyword.cloudaccesstraillong}} 服務來追蹤使用者及應用程式如何與 {{site.data.keyword.openwhisk_short}} 服務互動。


## 檢視產生的動作日誌
{: #logs_poll}

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




## 檢視啟動詳細資料
{: #activation_details}

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
    "namespace": "myNamespace",
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
            "value": "myNamespace/hello"
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
<td>此動作的註釋。如需可能的啟動註釋清單，請參閱[註釋參考主題](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation)。</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>是否公開發佈動作。</td>
</tr>
</tbody></table>



## 在 {{site.data.keyword.loganalysisfull_notm}} 中檢視日誌
{: #logs_view}

您可以直接從「{{site.data.keyword.openwhisk_short}} 監視」儀表板來檢視啟動日誌。日誌也會轉遞至 [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) 來進行檢索，將對所有產生的訊息啟用全文搜尋，並根據特定欄位進行方便的查詢。
{:shortdesc}

**附註**：美國東部地區未提供記載功能。

1. 開啟「[{{site.data.keyword.openwhisk_short}} 監視」頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/openwhisk/dashboard)。

2. 選用項目：若要只檢視特定動作的日誌，請將監視摘若要限制為該動作。在「過濾選項」區段中，從**限制至**下拉清單中選取動作名稱。

3. 在左側導覽中，按一下**日誌**。即會開啟 {{site.data.keyword.loganalysisshort_notm}} Kibana 頁面。

4. 選用項目：若要查看較舊的日誌，請按一下右上角的**最後 15 分鐘**，並選取不同的時間範圍，來變更預設時間範圍值 15 分鐘。

### 查詢日誌
{: #logs_query}

您可以使用 Kibana 的查詢語法，在 [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) 中尋找特定啟動日誌。

下列範例查詢可協助您對錯誤進行除錯：
  * 尋找所有錯誤日誌：
      ```
type: user_logs AND stream_str: stderr
```
      {: codeblock}

  * 尋找 "myAction" 產生的所有錯誤日誌：
      ```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
      {: codeblock}

### 查詢結果
{: #logs_query_results}

除了日誌行之外，[{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) 還會檢索由 {{site.data.keyword.openwhisk_short}} 產生的結果或啟動記錄。結果包含啟動 meta 資料，例如啟動期間或啟動結果碼。查詢結果欄位有助於您瞭解 {{site.data.keyword.openwhisk_short}} 動作的行為。

您可以使用 Kibana 的查詢語法來尋找特定的啟動日誌。下列範例查詢可協助您對錯誤進行除錯：

* 尋找所有失敗活動：
    ```
type: activation_record AND NOT status_str: 0
```
    {: codeblock}
    在結果中，`0` 表示順利結束動作，所有其他值則表示錯誤。

* 尋找失敗且發生特定錯誤的所有啟動：
    ```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
    {: codeblock}


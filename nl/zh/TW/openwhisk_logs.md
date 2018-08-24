---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 記載和監視活動
{: #openwhisk_logs}

{{site.data.keyword.openwhisk_short}} 內會自動啟用記載和監視，以協助您對問題進行疑難排解，並改進動作的性能和效能。

## 視圖日誌
{: #view-logs}

您可以直接從「{{site.data.keyword.openwhisk_short}} 監視」儀表板來檢視啟動日誌。日誌也會轉遞至 [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 來進行檢索，將對所有產生的訊息啟用全文搜尋，並根據特定欄位進行方便的查詢。
{:shortdesc}

1. 開啟「[{{site.data.keyword.openwhisk_short}} 監視」頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/openwhisk/dashboard/)。

2. 選用項目：若要只檢視特定動作的日誌，請將監視摘要限制為該動作。在「過濾選項」區段中，從**限制至**下拉清單中選取動作名稱。

3. 在左側導覽中，按一下**日誌**。即會開啟 {{site.data.keyword.loganalysisshort_notm}} Kibana 頁面。

4. 選用項目：若要查看較舊的日誌，請按一下右上角的**最後 15 分鐘**，並選取不同的時間範圍，來變更預設時間範圍值 15 分鐘。

### 查詢日誌
{: #query-logs}

您可以使用 Kibana 的查詢語法，在 [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 中尋找特定啟動日誌。

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
{: #query-results}

除了日誌行之外，[{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 還會檢索由 {{site.data.keyword.openwhisk_short}} 產生的結果或啟動記錄。結果包含啟動 meta 資料，例如啟動期間或啟動結果碼。查詢結果欄位有助於您瞭解 {{site.data.keyword.openwhisk_short}} 動作的行為。

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

## 監視活動
{: #openwhisk_monitoring}

[{{site.data.keyword.openwhisk_short}} 儀表板](https://console.bluemix.net/openwhisk/dashboard/)提供活動的圖形摘要。使用儀表板，以判定 {{site.data.keyword.openwhisk_short}} 動作的效能及性能。
{:shortdesc}

您可以選取要檢視的動作日誌來過濾日誌，並選取所記載活動的時間範圍。這些過濾器會套用至儀表板上的所有視圖。
隨時按一下**重新載入**，以使用最新啟動日誌資料來更新儀表板。

### 活動摘要
{: #summary}

**活動摘要**視圖提供 {{site.data.keyword.openwhisk_short}} 環境的高階摘要。使用此視圖，以監視已啟用 {{site.data.keyword.openwhisk_short}} 之服務的整體性能及效能。從此視圖中的度量值，您可以執行下列動作：
* 檢視呼叫動作的次數，以判定您的服務已啟用 {{site.data.keyword.openwhisk_short}} 的動作的使用率。
* 判定所有動作的整體失敗率。如果您發現錯誤，則可以檢視**活動直方圖**視圖，來找出發生錯誤的服務或動作。檢視**活動日誌**，以找出錯誤本身。
* 檢視與每一個動作相關聯的平均完成時間，以判定動作的效能。

### 活動時間表
{: #timeline}

**活動時間表**視圖會顯示垂直線圖形，以檢視過去及現在動作的活動。紅色指出特定動作內發生錯誤。請產生此視圖與**活動日誌**的關聯，以尋找錯誤的其他詳細資料。

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### 活動日誌
{: #log}

這個**活動日誌**視圖顯示啟動日誌的格式化版本。此視圖顯示每次啟動的詳細資料，但會每分鐘輪詢一次來尋找新的啟動。按一下動作來顯示詳細日誌。


若要使用 CLI 來取得顯示在「活動日誌」中的輸出，請使用下列指令：
```
ibmcloud fn activation poll
```
{: pre}

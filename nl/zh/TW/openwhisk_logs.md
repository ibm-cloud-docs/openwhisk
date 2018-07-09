---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 在 IBM Cloud 中檢視啟動日誌
{: #openwhisk_logs}

您可以直接從 [{{site.data.keyword.openwhisk}} 監視頁面](https://console.bluemix.net/openwhisk/dashboard/)來檢視啟動日誌。日誌也會轉遞至在其中對其檢索的 [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)，並啟用所有已產生訊息的全文搜尋，以及根據特定欄位（例如記載層次）的便利查詢。
{:shortdesc}

## 查詢日誌
{: #query-logs}

使用 [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 管理 Kibana 時，查詢日誌相當簡單。使用 Kibana 查詢語法來尋找您要尋找的日誌。

{{site.data.keyword.openwhisk_short}} 使用者介面可讓您直接導覽至 Kibana 中動作的日誌及結果。您可以在 [{{site.data.keyword.openwhisk}} 監視頁面](https://console.bluemix.net/openwhisk/dashboard/)的內部左導覽上，找到**日誌**鏈結。存取特定動作的詳細資料頁面時，**日誌**鏈結會將您帶至該特定動作的結果（啟動記錄）。顯示日誌之時間範圍的預設值設為 15 分鐘。如果您想要顯示較舊的記錄，可以直接在右上角的 Kibana 中變更此值。

以下是一些有助於進行錯誤除錯的查詢範例。

### 尋找所有錯誤日誌：
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### 尋找 "myAction" 所產生的所有錯誤日誌：
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## 查詢結果
{: #query-results}

除了日誌行之外，[IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 還會檢索 {{site.data.keyword.openwhisk_short}} 所產生的結果（啟動記錄）。結果會包含用於啟動的豐富相關 meta 資料，例如其持續時間或結果碼（成功、錯誤）。所有欄位都是可查詢的，因此可協助您瞭解 {{site.data.keyword.openwhisk_short}} 動作的行為。

使用 Kibana 查詢語法來尋找您要尋找的啟動。以下是一些有助於進行錯誤除錯的查詢範例。

### 尋找所有失敗的啟動：
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

與 Unix 指令相似，"`0`" 指出已順利結束動作，但其他所有項目都視為錯誤。

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### 尋找所有失敗且發生特定錯誤的啟動：
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}

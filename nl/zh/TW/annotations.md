---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: annotations, annotate, package, parameters, actions, functions

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


# 註釋
{: #annotations}

{{site.data.keyword.openwhisk}} 動作、觸發程式、規則及套件（統稱為實體）可以包括註釋。
{: shortdesc}

註釋會附加至實體，就像參數一樣。註釋包含定義名稱的 `key`，及定義值的 `value`。註釋最常用來記載動作和套件。{{site.data.keyword.openwhisk_short}} 型錄中的套件帶有註釋。這些註釋包括對其動作提供的功能的說明、在套件連結期間要使用的參數、呼叫期間的參數，或者參數是否為密碼。例如，視需要建立註釋來容許使用者介面整合。

您可以使用 `--annotation` 或 `-a` 旗標，從 CLI 記載實體。

## 動作註釋
{: #annotations_action}

|註釋|說明|
| --- | --- |
| `description` |動作的說明。|
|`parameters` |說明執行動作所需之動作的陣列。|
| `sampleInput` |顯示含一般值的輸入綱目的範例。|
| `sampleOutput` |顯示輸出綱目的範例，通常適用於 `sampleInput`。|



下列程式碼是 `echo` 動作的一組註釋範例，此動作會傳回其未經修改的輸入引數。例如，此動作適用於將輸入參數記載為某個序列或規則的一部分。

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Web 動作註釋
{: #annotations-specific-to-web-actions}

下列 Web 動作註釋必須明確地設為 `true`，才能啟用 API 互動。

|註釋|說明|
| --- | --- | 
| `web-export` |套用至動作時，動作會變成 [Web 動作](/docs/openwhisk?topic=cloud-functions-actions_web)。REST 呼叫不需鑑別即可存取此動作，以便使用者從瀏覽器存取動作。Web 動作的擁有者會引發執行它們的成本。換言之，動作的擁有者也擁有啟動記錄。|
| `final` |套用於某個動作時，在呼叫期間提供的參數無法置換先前定義的任何動作參數。|
| `raw-http` |套用至具有 `web-export` 註釋的動作時，會 HTTP 要求查詢和內文參數當作保留內容傳遞至動作。|
| `web-custom-options` |會啟用 Web 動作，以回應具有自訂標頭的 OPTIONS 要求。否則，會套用[預設 CORS 回應](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options)。|
| `require-whisk-auth` |Web 動作只能由提供適當的鑑別認證的要求呼叫。設為布林值時，可控制是否鑑別要求的「基本鑑別」值。`true` 值會鑑別認證，而 `false` 值會呼叫動作，而不進行任何鑑別。設為整數或字串時，此值必須符合要求的 `X-Require-Whisk-Auth` 標頭值。|

## 套件註釋
{: #annotations_package}

|註釋|說明|
| --- | --- |
| `description` |套件的說明。|
|`parameters` |說明範圍設為套件之參數的陣列。|

## 參數註釋
{: #annotations_parameter}

|註釋|說明|
| --- | --- |
| `name` |參數的名稱。|
| `description` |對參數的精簡說明。|
| `doclink` |參數的進一步說明文件的鏈結（適用於 OAuth 記號）。|
| `required` |True 表示必要參數，false 則表示選用性參數。|
| `bindTime` |如果在連結套件時指定參數，則為 True。|
| `type` |參數的類型，為 `password` 或 `array` 其中一個（但可更廣泛使用）。|

## 啟動註釋
{: #annotations_activation}

您可以使用下列註釋來記載啟動記錄：

|註釋|說明|
| --- | --- |
| `path` |產生啟動之動作的完整路徑名稱。如果此啟動是套件連結中動作的結果，則路徑會參照母項套件。|
| `kind` |所執行動作的類型，以及其中一種受支援的 {{site.data.keyword.openwhisk_short}} 運行環境類型。|
| `limits` |此啟動所受的時間、記憶體和日誌限制。|

對於序列相關的啟動，系統會產生下列註釋：

|註釋|說明|
| --- | --- |
| `topmost` |僅對於最外層的序列動作，才會出現此註釋。|
|`causedBy`|僅對於序列中包含的動作，才會出現此註釋。|
|`waitTime`|在內部 {{site.data.keyword.openwhisk_short}} 系統中等待的時間。這大概是接收啟動要求與呼叫程式為動作佈建容器之間所花費的時間。此值只會出現在非序列相關的啟動。對於序列，此資訊可以衍生自 `topmost` 序列啟動記錄。|
|`initTime`|起始設定函數所用時間。如果此值存在，則動作需要起始設定，並代表冷開機。暖啟動會跳過起始設定，在此情況下，不會產生註釋。|

下列範例顯示這些註釋出現在啟動記錄中的樣子。

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}





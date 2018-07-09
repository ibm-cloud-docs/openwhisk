---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 註釋
{: #openwhisk_annotations}

可以使用 `annotations` 來裝飾 {{site.data.keyword.openwhisk}} 動作、觸發程式、規則及套件（統稱為資產）。註釋會附加至資產，就像具有可定義名稱的 `key` 及可定義值的 `value` 的參數。從指令行介面 (CLI) 使用 `--annotation` 旗標或 `-a`（簡稱）設定它們十分方便。
{: shortdesc}

基本原理：已將註釋新增至 {{site.data.keyword.openWhisk_short}} 來容許進行實驗，而不需要變更基礎資產綱目。截至撰寫本文件之前，故意不定義所允許的 `annotations`。不過，因為註釋大多用來告知語意變更，所以開始記載註釋很重要。

到目前為止最常用的註釋是記載動作及套件。{{site.data.keyword.openwhisk_short}} 型錄中的許多套件都帶有註釋，例如，其動作所提供之功能的說明、套件連結期間所使用的參數、呼叫期間參數，或者參數是否為 "secret"（例如，密碼）。例如，視需要建立註釋來容許使用者介面整合。

以下是 `echo` 動作的一組註釋範例，此動作會傳回其未經修改的輸入引數（例如，`function main(args) { return args }`）。例如，此動作適用於將輸入參數記載為某個序列或規則的一部分。
```
ibmcloud wsk action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

說明**套件**的註釋包括：

- `description`：套件的簡潔說明。
- `parameters`：說明範圍設為套件之參數的陣列。

說明**動作**的註釋包括：

- `description`：動作的簡潔說明。
- `parameters`：說明執行動作所需之動作的陣列。
- `sampleInput`：顯示含一般值的輸入綱目的範例。
- `sampleOutput`：顯示輸出綱目的範例，通常適用於 `sampleInput`。

說明**參數**的註釋包括：

- `name`：參數的名稱。
- `description`：參數的簡潔說明。
- `doclink`：參數的進一步說明文件的鏈結（適用於 OAuth 記號）。
- `required`：True 表示必要參數，false 則表示選用性參數。
- `bindTime`：如果在連結套件時指定參數，則為 True。
- `type`：參數的類型，為 `password` 或 `array` 其中一個（但可更廣泛使用）。

註釋_不_ 會進行檢查。因此，舉例來說，雖然可以想見可使用註釋來推斷將兩個動作合併至序列是否正當，但系統並未這麼做。

## Web 動作特定的註釋
{: #annotations-specific-to-web-actions}

最近，核心 API 已用新的特性進行延伸。為了讓套件和動作能夠參與這些特性，引進了語意上有意義的新註釋。這些註釋必須明確地設為 `true` 才會作用。將值從 `true` 變更為 `false`，會從新的 API 中排除所附加的資產。註釋在系統中將沒有意義。請參閱下列註釋：

- `web-export`：僅適用於動作。如果存在的話，它會讓 REST 呼叫存取其對應的動作，而_不_ 需要進行鑑別。這些稱為 [_Web 動作_](openwhisk_webactions.html)，因為它們容許使用者從瀏覽器使用 OpenWhisk 動作（舉例來說）。請務必注意，Web 動作的_擁有者_ 會引發在系統中執行它們的成本。換言之，動作的_擁有者_ 也擁有啟動記錄。
- `final`：僅適用於動作。它會將所有已定義的動作參數設為不可變。參數值透過其含括套件或動作定義而定義之後，即無法以呼叫期間的參數來置換帶有註釋的動作參數。
- `raw-http`：僅於 `web-export` 註釋存在時才適用於動作。如果存在的話，也會將 HTTP 要求查詢和內文參數當作保留內容傳遞至動作。
- `web-custom-options`：設定此註釋時，會啟用 Web 動作，以回應使用自訂標頭的 OPTIONS 要求，否則，會套用[預設 CORS 回應](openwhisk_webactions.html#options-requests)。
- `require-whisk-auth`：此註釋會保護 Web 動作，因此，只有提供適當鑑別認證的要求才會呼叫它。設為布林值時，可控制是否鑑別要求的「基本鑑別」值（即 Whisk 鑑別金鑰）。`true` 值會鑑別認證，而 `false` 值會呼叫動作，而不進行任何鑑別。設為數字或字串時，此值必須符合要求的 `X-Require-Whisk-Auth` 標頭值。在這兩種情況下，請務必注意，Web 動作的_擁有者_ 會引發在系統中執行它們的成本（即動作的_擁有者_ 也會擁有啟動記錄）。

## 啟動專用的註釋

系統可以使用下列註釋來裝飾啟動記錄：

- `path`：產生啟動之動作的完整路徑名稱。請注意，如果此啟動是套件連結中動作的結果，則路徑會參照母項套件。
- `kind`：執行的動作類型，以及一種支援 OpenWhisk 執行時期類型。
- `limits`：限制此啟動的時間、記憶體及日誌限制。

對於序列相關的啟動，系統會產生下列註釋：

- `topmost`：這只適用於最外層序列動作。
- `causedBy`：這只適用於序列所內含的動作。

最後，為了提供效能透通性，啟動也會記錄：

- `waitTime`：在內部 OpenWhisk 系統中等待所花費的時間。這大概是接收啟動要求的控制器與呼叫程式為動作供應容器之間所花費的時間。目前僅對非序列相關的啟動存在此值。對於序列，此資訊可以衍生自 `topmost` 序列啟動記錄。
- `initTime`：起始設定函數所花費的時間。如果此值存在，則動作需要起始設定，並代表冷開機。暖啟動會跳過起始設定，在此情況下，不會產生註釋。

下面顯示了這些註釋出現在啟動記錄中的範例。

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

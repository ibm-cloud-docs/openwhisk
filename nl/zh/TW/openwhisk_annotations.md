---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# OpenWhisk 資產的註釋

可以使用 `annotations` 來裝飾 OpenWhisk 的「動作」、「觸發程式」、「規則」和套件（統稱為資產）。註釋會附加至資產，就像具有可定義名稱的 `key` 及可定義值的 `value` 的參數。從指令行介面 (CLI) 透過 `--annotation` 或 `-a`（簡稱）設定它們十分方便。
{: shortdesc}

基本原理：已將註釋新增至 OpenWhisk 來容許進行實驗，而不需要變更基礎資產綱目。截至撰寫本文件之前，故意不定義所允許的 `annotations`。不過，因為註釋大多用來告知語意變更，所以開始記載註釋很重要。

到目前為止最常用的註釋是記載「動作」及套件。OpenWhisk 型錄中的許多套件都帶有註釋，例如，其「動作」所提供之功能的說明、套件連結期間所使用的參數、呼叫期間參數，或者參數是否為 "secret"（例如，密碼）。例如，視需要建立註釋來容許使用者介面整合。

以下是「`echo` 動作」的一組註釋範例，此動作會傳回其未經修改的輸入引數（例如，`function main(args) { return args }`）。例如，此「動作」適用於將輸入參數記載為某個「序列」或「規則」的一部分。

```
wsk action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

說明套件的註釋包括：

- `description`：套件的簡潔說明。
- `parameters`：說明範圍設為套件之參數的陣列。

說明「動作」的註釋包括：

- `description`：「動作」的簡潔說明。
- `parameters`：說明執行「動作」所需之「動作」的陣列。
- `sampleInput`：顯示含一般值的輸入綱目的範例。
- `sampleOutput`：顯示輸出綱目的範例，通常適用於 `sampleInput`。

說明參數的註釋包括：

- `name`：參數的名稱。
- `description`：參數的簡潔說明。
- `doclink`：參數的進一步說明文件的鏈結（適用於 OAuth 記號）。
- `required`：True 表示必要參數，false 則表示選用性參數。
- `bindTime`：如果在連結套件時指定參數，則為 True。
- `type`：參數的類型，為 `password` 或 `array` 其中一個（但可更廣泛使用）。

註釋_不_ 會進行檢查。因此，舉例來說，雖然可以想見可使用註釋來推斷將兩個「動作」合併至序列是否正當，但系統並未這麼做。

## Web 動作特定的註釋
{: #openwhisk_annotations_webactions}

最近，核心 API 已用新的特性進行延伸。為了讓套件和「動作」能夠參與這些特性，引進了語意上有意義的新註釋。這些註釋必須明確地設為 `true` 才會作用。將值從 `true` 變更為 `false`，會從新的 API 中排除所附加的資產。註釋在系統中將沒有意義。請參閱下列註釋：

- `web-export`：僅適用於「動作」。如果存在，它會讓 REST 呼叫存取其對應的「動作」，而_不_ 需要進行鑑別。這些稱為 [_Web 動作_](openwhisk_webactions.html)，因為它們容許使用者從瀏覽器使用「OpenWhisk 動作」（舉例來說）。請務必注意，「Web 動作」的_擁有者_ 會引發在系統中執行它們的成本。換句話說，「動作」的_擁有者_ 也擁有啟動記錄。
- `final`：僅適用於「動作」。它會將所有已定義的「動作」參數設為不可變。參數值透過其含括套件或「動作」定義而定義之後，便無法以呼叫期間的參數來置換帶有註釋的「動作」參數。
- `raw-http`：僅於 `web-export` 註釋存在時適用於「動作」。如果存在的話，也會將 HTTP 要求查詢和內文參數當作保留內容傳遞至「動作」。
- `web-custom-options`：設定此註釋時，會啟用 Web 動作，以回應使用自訂標頭的 OPTIONS 要求，否則，會套用[預設 CORS 回應](openwhisk_webactions.html#options-requests)。
- `require-whisk-auth`：適用於「動作」。如果「動作」帶有 `web-export` 註釋，且此註釋也是 `true`，則只有已鑑別的身分才能存取路徑。請務必注意，「Web 動作」的_擁有者_ 會引發在系統中執行它們的成本。換句話說，「動作」的_擁有者_ 也擁有啟動記錄。

## 啟動專用的註釋

系統也會以註釋來裝飾啟動記錄。它們包含：

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

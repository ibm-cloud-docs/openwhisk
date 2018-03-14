---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Web 動作
{: #openwhisk_webactions}

「Web 動作」是已註釋的「OpenWhisk 動作」，可快速讓開發人員建置 Web 型應用程式。這些已註釋的「動作」可讓開發人員對後端邏輯進行程式設計，而 Web 應用程式可匿名存取該後端邏輯，而不需要 OpenWhisk 鑑別金鑰。由「動作」開發人員決定實作其擁有的所需鑑別及授權（亦即 OAuth 流程）。
{: shortdesc}

「Web 動作」啟動會與已建立「動作」的使用者相關聯。此「動作」會將「動作」啟動的成本從呼叫者推遲到「動作」的擁有者身上。

請查看下列「JavaScript 動作」`hello.js`：
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

您可以搭配使用 CLI 的 `--web` 旗標與 `true` 或 `yes` 值，在名稱空間 `guest` 的套件 `demo` 中建立 _Web 動作_ `hello`：
```
wsk package create demo
```
{: pre}

```
wsk action create /guest/demo/hello hello.js --web true
```
{: pre}

搭配使用 `--web` 旗標與 `true` 或 `yes` 值，容許透過 REST 介面存取「動作」，而不需要使用認證。您可以使用結構如下的 URL 來呼叫「Web 動作」：`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`。

如果「動作」不在具名「套件」中，則「套件」名稱為 `default`。

範例為 `guest/demo/hello`。「Web 動作」API 路徑可以與 `curl` 或 `wget` 搭配使用，而不需要 API 金鑰。它甚至可以直接輸入瀏覽器中。

請嘗試在 Web 瀏覽器中開啟 [https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane](https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane)。或者，嘗試透過 `curl` 呼叫「動作」：
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

在下列範例中，「Web 動作」會執行 HTTP 重新導向：
```javascript
function main() {
    return { 
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

在下列範例中，「Web 動作」會設定單一 Cookie：
```javascript
function main() {
    return { 
    headers: { 
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}  

在下列範例中，「Web 動作」會設定多個 Cookie：
```javascript
function main() {
    return { 
    headers: { 
      'Set-Cookie': [
        'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
      ],
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
}
```
{: codeblock}

下列範例會傳回 `image/png`：
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

The following example returns `application/json`:
```javascript
function main(params) { 
    return {
        statusCode: 200,
        headers: { 'Content-Type': 'application/json' },
        body: params
    };
}
```
{: codeblock}  

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [回應大小限制](./openwhisk_reference.html)（適用於「動作」），因為如果回應超出預先定義的系統限制，就會失敗。例如，大型物件不會透過 OpenWhisk 在行內傳送，而是改為延遲到物件儲存庫。

## 使用動作處理 HTTP 要求
{: #openwhisk_webactions_http}

不是「Web 動作」的「OpenWhisk 動作」需要這兩個鑑別，而且必須回應 JSON 物件。相對地，「Web 動作」可在未鑑別的情況下呼叫，而且可用來實作回應不同類型的 _headers_、_statusCode_ 及 _body_ 內容的 HTTP 處理程式。「Web 動作」必須傳回 JSON 物件。但是，如果「Web 動作」的結果包含下列其中一個最上層 JSON 內容，則 OpenWhisk 系統（即 `controller`）會以不同的方式處理「Web 動作」：

- `headers`：索引鍵為標頭名稱且值為這些標頭的字串、數字或布林值的 JSON 物件（預設值為無標頭）。若要傳送單一標頭的多個值，則標頭的值是值的 JSON 陣列。
- `statusCode`：有效的 HTTP 狀態碼（預設值為 200 OK）。
- `body`：為純文字或 Base 64 編碼字串（適用於二進位資料）的字串。

控制器會將動作指定的標頭（如果有的話）傳遞給終止要求/回應的 HTTP 用戶端。同樣地，控制器會在出現時回應狀態碼。最後，body 會作為回應的內文傳遞。除非在「動作」結果的 `headers` 中宣告 `Content-Type` 標頭，否則會將 body 視為字串一樣傳遞（或者會導致錯誤）。定義 `Content-Type` 時，控制器會判斷回應是二進位資料還是純文字，並視需要使用 Base64 解碼器來解碼字串。如果 body 無法正確解碼，則會傳回錯誤給呼叫者。

_附註_：JSON 物件或陣列被視為二進位資料，而且必須以 base64 編碼。

## HTTP 環境定義

呼叫時，所有「Web 動作」都會接收 HTTP 要求明細，作為「動作」輸入引數的參數。 

請參閱下列 HTTP 參數：

- `__ow_method`（類型：字串）：要求的 HTTP 方法。
- `__ow_headers`（類型：將字串對映至字串）：要求標頭。
- `__ow_path`（類型：字串）：要求的不相符路徑（比對會在使用「動作」副檔名之後停止）。
- `__ow_user`（類型：字串）：識別 OpenWhisk 已鑑別身分的「名稱空間」
- `__ow_body`（類型：字串）：要求內文實體，當內容為二進位時，為 base64 編碼的字串，否則為一般字串
- `__ow_query`（類型：字串）：要求中的查詢參數，為未剖析的字串

要求無法置換任何具名的 `__ow_` 參數。這麼做會導致要求失敗，其狀態等於「400 不正確的要求」。

只有在「Web 動作」[已註釋為需要鑑別](./openwhisk_annotations.html#openwhisk_annotations_webactions)，並允許「Web 動作」實作自己的授權原則時，`__ow_user` 才會存在。只有在「Web 動作」選擇要處理[「原始」HTTP 要求](#raw-http-handling)時，才能使用 `__ow_query`。這是一個字串，其中包含從 URI 剖析的查詢參數（以 `&` 區隔）。在「原始」HTTP 要求中，或是當 HTTP 要求實體不是 JSON 物件或表單資料時，會出現 `__ow_body` 內容。否則，「Web 動作」會接收查詢和內文參數，作為「動作」引數中的第一個類別內容。內文參數的優先順序高於查詢參數，而查詢參數的優先順序則高於「動作」和套件參數。

## HTTPS 端點支援

支援的 SSL 通訊協定：TLS 1.0、TLS 1.1、TLS 1.2、TLS 1.3（[初步版本 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18)）

不受支援的 SSL 通訊協定：SSLv2、SSLv3

## 額外特性
{: #extra-features}

「Web 動作」提供的額外特性包括：

- `內容副檔名`：要求必須將其所需的內容類型指定為下列其中一項：`.json`、`.html`、`.http`、`.svg` 或 `.text`。指定類型的作法是將副檔名新增至 URI 中的「動作」名稱，因此動作 `/guest/demo/hello` 參照為 `/guest/demo/hello.http`（舉例來說），以接收傳回的 HTTP 回應。為了方便起見，如果未偵測到副檔名，則假設為 `.http` 副檔名。
- `投射結果中的欄位`：接在「動作」名稱後面的路徑是用來投射回應的一個以上層次。`/guest/demo/hello.html/body`。此特性可讓傳回字典 `{body: "..." }` 的「動作」投射 `body` 內容，並且改為直接傳回其字串值。投射路徑遵循絕對路徑模型（如 XPath）。
- `作為輸入的查詢及內文參數`：「動作」會接收查詢參數，以及要求內文中的參數。參數的合併優先順序是：套件參數、「動作」參數、查詢參數及內文參數。這些參數每一個都會在重疊時覆寫任何先前的值。舉例來說，`/guest/demo/hello.http?name=Jane` 會將引數 `{name: "Jane"}` 傳遞給「動作」。
- `表單資料`：除了標準 `application/json` 之外，「Web 動作」還可以接收從資料 `application/x-www-form-urlencoded data` 編碼的 URL，作為輸入。
- `透過多個 HTTP 動詞來啟動`：「Web 動作」可以透過下列任何 HTTP 方法來呼叫：`GET`、`POST`、`PUT`、`PATCH` 和 `DELETE`，以及 `HEAD` 和 `OPTIONS`。
- `非 JSON 內文和原始 HTTP 實體處理`：「Web 動作」可以接受 JSON 物件以外的 HTTP 要求內文，並可選擇一律接收這類值作為不透明值（非二進位時為純文字，否則為 base64 編碼字串）。

下面範例簡短概述如何在「Web 動作」中使用這些特性。請考量具有下列內文的動作 `/guest/demo/hello`：
```javascript
  function main(params) {
      return { response: params };
}
```

將此「動作」當作「Web 動作」呼叫時，您可以從結果投射不同的路徑，以變更「Web 動作」的回應。例如，若要傳回整個物件，並查看「動作」所接收的引數：

```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json
```
{: pre}
```json
{
  "response": {
    "__ow_method": "get",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

若要使用查詢參數來執行，請參閱下列範例指令：
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane
```
{: pre}
```json
{
  "response": {
    "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

您也可以使用表單資料來執行：
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -d "name":"Jane"
```
{: pre}
```json
{
  "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "10",      
      "content-type": "application/x-www-form-urlencoded",      
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

對 JSON 物件執行下列指令：
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}
```json
{
  "response": {
    "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```

執行下列指令以投射名稱（以文字形式）：
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}
```
Jane
```

為了方便起見，查詢參數、表單資料和 JSON 物件內文實體全都被當作字典來處理，並且可以直接存取其值作為「動作」輸入內容。但是選擇以較直接的方式來處理 HTTP 要求實體的「Web 動作」，或是當「Web 動作」接收不是 JSON 物件的實體時，此行為就不是這樣了。

請參閱下列使用 "text" 內容類型的範例（如前所示）。
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}
```json
{
  "response": {
    "__ow_method": "post",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "4",      
      "content-type": "text/plain",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": "",
    "__ow_body": "Jane"
  }
}
```


## 內容副檔名
{: #openwhisk_webactions_extensions}

通常需要內容副檔名才能呼叫「Web 動作」。若沒有副檔名，則會假設 `.http` 為預設值。`.json` 及 `.http` 副檔名不需要投射路徑，而 `.html`、`.svg` 及 `.text` 副檔名則需要。為了方便起見，會假設預設路徑符合副檔名。若要呼叫「Web 動作」並接收 `.html` 回應，「動作」必須回應包含稱為 `html` 的最上層內容的 JSON 物件（或者回應必須位於明確的路徑中）。換言之，`/guest/demo/hello.html` 相當於明確地投射 `html` 內容，如 `/guest/demo/hello.html/html`。「動作」的完整名稱必須包括其套件名稱，如果「動作」不在具名套件中，其為 `default`。

## 受保護的參數
{: #openwhisk_webactions_protected}

動作參數也會受到保護並視為不可變。系統會自動完成參數，以啟用「Web 動作」。

```
 wsk action create /guest/demo/hello hello.js \
      --parameter name Jane \
      --web true
```

這些變更的結果為 `name` 已連結至 `Jane`，而且無法因最終註釋而置換為查詢或內文參數。此設計可保護對意外或有意嘗試變更此值的查詢或內文參數所執行的動作。 

## 停用 Web 動作

若要停用透過 Web API (`https://openwhisk.ng.bluemix.net/api/v1/web/`) 呼叫「Web 動作」，請將 `false` 或 `no` 值傳遞給 `--web` 旗標，以使用 CLI 來更新「動作」。

```
 wsk action update /guest/demo/hello hello.js --web false
```
{: pre}

## 原始 HTTP 處理

「Web 動作」可以選擇直接解譯及處理送入的 HTTP 內文，而不需將 JSON 物件提升為可用於「動作」輸入的第一個類別內容（例如，`args.name` 與剖析 `args.__ow_query`）。透過 `raw-http` [註釋](./openwhisk_annotations.html)即可完成此處理程序。使用稍早顯示的相同範例，但現在是作為「原始」HTTP Web 動作，它會接收 `name` 同時作為查詢參數以及 HTTP 要求內文中的 JSON 值：
```
curl https://openwhisk.ng.bluemix.net/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}
```json 
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"      
    },
    "__ow_path": ""
  }
}
```

OpenWhisk 使用 [Akka Http](http://doc.akka.io/docs/akka-http/current/scala/http/) 架構來[判斷](http://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html)哪些內容類型是二進位，哪些是純文字。

### 啟用原始 HTTP 處理

若要啟用原始 HTTP Web 動作，可以搭配使用 `--web` 旗標與 `raw` 值。

```
 wsk action create /guest/demo/hello hello.js --web raw
```

### 停用原始 HTTP 處理

將 `false` 或 `no` 值傳遞給 `--web` 旗標，即可停用原始 HTTP。

```
 wsk update create /guest/demo/hello hello.js --web false
```

### 從 Base64 解碼二進位內文內容

處理原始 HTTP 內容時，若要求 `Content-Type` 是二進位，則會以 Base64 編碼 `__ow_body` 內容。
下列函數示範如何解碼 Node、Python 及 Swift 中的內文內容。只需要將方法儲存至檔案、建立利用所儲存構件的原始 HTTP Web 動作，然後呼叫「Web 動作」即可。

#### Node

```javascript
function main(args) {
     decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

```swift
extension String {
    func base64Decode() -> String? {
        guard let data = Data(base64Encoded: self) else {
            return nil
        }

        return String(data: data, encoding: .utf8)
    }
}

func main(args: [String:Any]) -> [String:Any] {
    if let body = args["__ow_body"] as? String {
        if let decoded = body.base64Decode() {
            return [ "body" : decoded ]
        }
    }

    return ["body": "Could not decode body from Base64."]
}
```
{: codeblock}

例如，將 Node 函數儲存為 `decode.js`，然後執行下列指令：
```
 wsk action create decode decode.js --web raw
```
{: pre}

```
ok: created action decode
```

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// openwhisk.ng.bluemix.net/api/v1/web/guest/default/decodeNode.json
```
{: pre}

```json
{
  "body": "Decoded body"
}
```

## Options 要求
{: #options-requests}

依預設，對「Web 動作」所提出的 OPTIONS 要求會導致在回應標頭自動新增 CORS 標頭。這些標頭允許所有原點，以及 options、get、delete、post、put、head 和 patch 等 HTTP 動詞。

請參閱下列標頭：

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

或者，可以由「Web 動作」手動處理 OPTIONS 要求。若要啟用此選項，請在「Web 動作」新增 `web-custom-options` 註釋，以及值 `true`。啟用此特性時，不會自動將 CORS 標頭新增至要求回應。而是要由開發人員負責以程式設計方式附加他們想要的標頭。請參閱下列範例，以建立 OPTIONS 要求的自訂回應。

```
  function main(params) {
      if (params.__ow_method == "options") {
    return {
      headers: {
        'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
      },
      statusCode: 200
    }
  }
}
```

將函數儲存到 `custom-options.js`，然後執行下列指令：

```
$ wsk action create custom-option custom-options.js --web true -a web-custom-options true
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
< HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
```

## 錯誤處理
{: #openwhisk_webactions_errors}

「OpenWhisk 動作」在兩種不同的可能失敗模式下失敗。第一個稱為_應用程式錯誤_，類似於捕捉的異常狀況：「動作」會傳回包含最上層 `error` 內容的 JSON 物件。第二個是「動作」災難性地失敗並且未產生回應時所發生的_開發人員錯誤_（這類似於未捕捉到的異常狀況）。對於「Web 動作」，控制器會如下處理應用程式錯誤：

- 忽略所有指定的路徑投射，控制器會改為投射 `error` 內容。
- 控制器會將依「動作」副檔名所暗示的內容處理套用至 `error` 內容的值。

開發人員必須知道 Web 動作的使用方式，以及如何產生適當的錯誤回應。例如，與 `.http` 副檔名搭配使用的「Web 動作」傳回 HTTP 回應，類似於 `{error: { statusCode: 400 }`。無法這麼做時，會導致副檔名中所暗示的 `Content-Type` 與錯誤回應中的動作 `Content-Type` 不符。必須對具有序列的「Web 動作」進行特殊考量，才能讓構成序列的元件在必要時產生足夠的錯誤。


---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 使用 OpenWhisk REST API
{: #openwhisk_rest_api}

啟用 OpenWhisk 環境之後，即可利用 REST API 呼叫來搭配使用 OpenWhisk 與 Web 應用程式或行動應用程式。
{: shortdesc}

如需「動作」、「啟動」、「套件」、「規則」及「觸發程式」之 API 的相關資訊，請參閱 [OpenWhisk API 文件](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json)。


系統中的所有功能都可透過 REST API 來使用。集合及實體端點可用於「動作」、「觸發程式」、「規則」、「套件」、「啟動」及「名稱空間」。

可用的集合端點：
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

`{APIHOST}` 是 OpenWhisk API 主機名稱（例如，openwhisk.ng.bluemix.net、172.17.0.1、192.168.99.100、192.168.33.13 等等）。
針對 `{namespace}`，字元 `_` 可以用來指定使用者的*預設名稱空間*。

您可以在集合端點上執行 GET 要求，以提取集合中的實體清單。

每一種類型的實體都有下列實體端點：
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

「名稱空間」和「啟動」端點支援 GET 要求。「動作」、「觸發程式」、「規則」和「套件」端點支援 GET、PUT 和 DELETE 要求。「動作」、「觸發程式」及「規則」的端點也支援 POST 要求，其用來呼叫「動作」和「觸發程式」，以及啟用或停用「規則」。 

所有 API 都是透過 HTTP 基本鑑別進行保護。您可以使用 [wskadmin ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) 工具，來產生新的「名稱空間」和鑑別。
基本鑑別認證位於 `~/.wskprops` 檔案的 `AUTH` 內容中，以冒號區隔。您也可以使用執行 `wsk property get --auth` 的 CLI 來擷取這些認證。


在下列範例中，[cURL](https://curl.haxx.se) 指令工具用來取得「`whisk.system` 名稱空間」中所有「套件」的清單：
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

在此範例中，利用 `-u` 旗標來傳遞鑑別。您也可以傳遞此值作為 URL 的一部分，例如，`https://$AUTH@{APIHOST}`。

OpenWhisk API 支援 Web 用戶端發出的「要求/回應」呼叫。OpenWhisk 會以「跨原點資源共用」標頭來回應 `OPTIONS` 要求。目前可允許所有原點（亦即，Access-Control-Allow-Origin 為 "`*`"），而 Access-Control-Allow-Header 會產生 Authorization 和 Content-Type。

**注意：**因為 OpenWhisk 目前只支援每個「名稱空間」一個金鑰，因此不建議在簡單實驗之外使用 CORS。使用 [Web 動作](./openwhisk_webactions.html)或 [API 閘道](./openwhisk_apigateway.html)來公開「動作」，對於需要 CORS 的用戶端應用程式，則不要使用 OpenWhisk 授權碼。

## 使用 CLI 詳細模式
{: #openwhisk_rest_api_cli_v}

OpenWhisk CLI 是 OpenWhisk REST API 的介面。
您可以使用 `-v` 旗標以詳細模式執行 CLI，這樣會列印 HTTP 要求及回應的所有相關資訊。

執行下列指令，顯示現行使用者的「名稱空間」值：
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

列印的資訊提供 HTTP 要求的內容，並在 URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` 上執行 HTTP 方法 `GET`，方法是使用 User-Agent 標頭 `OpenWhisk-CLI/1.0 (<CLI-Build-version>)`，以及「基本授權」標頭 `Basic XXXYYYY`。
請注意，授權值是 base64 編碼的 OpenWhisk 授權字串。
回應的內容類型為 `application/json`。

## 動作
{: #openwhisk_rest_api_actions}

若要建立或更新「動作」，請在「動作」集合中使用方法 `PUT` 來傳送 HTTP 要求。例如，若要使用單一檔案內容來建立名稱為 `hello` 的「`nodejs:6` 動作」，請使用下列指令：
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

若要對動作執行區塊處理呼叫，並使用方法 `POST` 以及包含輸入參數 `name` 的內文來傳送 HTTP 要求，請使用下列指令：
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

您會取得下列回應：

```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
          "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
若要取得 `response.result`，請使用查詢參數 `result=true` 重新執行指令，如下列範例所示：
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
您會取得下列回應：
```json
{
  "payload": "hello John"
}
```

## 註釋及 Web 動作
{: #openwhisk_rest_api_webactions}

若要建立「動作」作為「Web 動作」，您需要針對「Web 動作」新增 `web-export=true` 的[註釋](./openwhisk_annotations.html)。因為「Web 動作」可以公開存取，所以您想要使用註釋 `final=true` 來保護預先定義的參數（亦即，將它們視為最終）。如果您建立或更新的「動作」使用 CLI 旗標 `--web true`，則指令會新增註釋 `web-export=true` 和 `final=true`。

執行下列 curl 指令，以提供要在「動作」上設定的完整註釋清單。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

您現在可以將此「動作」呼叫為沒有 OpenWhisk 授權的公用 URL。使用「Web 動作」公用 URL，並在 URL 的結尾（舉例說明）包含副檔名（例如，`.json` 或 `.http`），來嘗試呼叫。
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

此範例原始碼不適用於 `.http`，如需如何修改，請參閱 [Web 動作](./openwhisk_webactions.html)文件。

## 序列
{: #openwhisk_rest_api_sequences}

若要建立「動作」序列，請提供「動作」的名稱，這些動作依所要的順序來組成順序。因此，第一個「動作」的輸出會作為輸入傳遞至下一個「動作」。

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

使用「`/whisk.system/utils/split` 動作」及「`/whisk.system/utils/sort` 動作」來建立序列。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

當您指定「動作」的名稱時，它們必須是完整的。

## 觸發程式
{: #openwhisk_rest_api_triggers}

若要建立「觸發程式」，您至少需要「觸發程式」名稱資訊。您也可以包含預設參數，以在發動「觸發程式」時透過 Tule 來傳遞至「動作」。

使用已設定值 `webhook` 的預設參數 `type`，來建立名稱為 `events` 的「觸發程式」。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

現在，只要您的事件需要發動此「觸發程式」，都可以使用「OpenWhisk 授權」金鑰，以方法 `POST` 來發出 HTTP 要求。

若要使用參數 `temperature` 來發動觸發程式 `events`，請傳送下列 HTTP 要求。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### 具有資訊來源動作的觸發程式
{: #openwhisk_rest_api_triggers_feed}

您可以使用「資訊來源動作」來建立特殊「觸發程式」。「資訊來源動作」是一個「動作」，可協助配置「資訊來源」提供者，每當發生「觸發程式」的事件時，該提供者就會負責發動「觸發程式」。請參閱 [feeds.md] 文件，以進一步瞭解這些資訊來源提供者。

運用「資訊來源動作」的部分可用「觸發程式」為定期/警示、Slack、Github、Cloudant/Couchdb 及 messageHub/Kafka。您也可以建立自己的「資訊來源動作」及「資訊來源」提供者。

建立名稱為 `periodic` 的「觸發程式」，並以指定的頻率（每 2 小時，例如，02:00:00、04:00:00）發動。..).

使用 CLI，執行下列指令來建立「觸發程式」：
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

因為使用 `-v` 旗標，所以會傳送兩個 HTTP 要求。其中一個是建立觸發程式 `periodic`，另一個是呼叫「資訊來源動作」。資訊來源動作 `/whisk.system/alarms/alarm` 會傳送參數，以配置「資訊來源」提供者每 2 小時發動「觸發程式」。

若要使用 REST API 達成此活動，請先建立觸發程式，如下所示：
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

如您所見，註釋 `Feed` 儲存在「觸發程式」中。稍後，此註釋可以用來知道要使用哪個「資訊來源動作」來刪除「觸發程式」。

既然已建立「觸發程式」，則可呼叫「資訊來源動作」
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

刪除「觸發程式」類似於建立「觸發程式」。因此，此時，請使用「資訊來源動作」來刪除「觸發程式」，將「資訊來源提供者」配置成同時刪除「觸發程式」的處理程式。

執行下列指令來呼叫「資訊來源動作」，以從資訊來源提供者刪除「觸發程式」處理程式：
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

現在，使用 `DELETE` 方法，以 HTTP 要求來刪除「觸發程式」：
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## 規則
{: #openwhisk_rest_api_rules}

若要建立在「觸發程式」與「動作」之間建立關聯的規則，請傳送含有 `PUT` 方法的 HTTP 要求，以便在要求內文中提供「觸發程式」和「動作」。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

您可以啟用或停用規則，也可以透過更新其狀態內容來變更「規則」的狀態。例如，若要停用規則 `t2a`，請使用 `POST` 方法，在要求內文中傳送 `status: "inactive"`。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## 套件
{: #openwhisk_rest_api_packages}

若要在「套件」中建立「動作」，您必須先建立一個「套件」。執行下列指令，以建立一個名稱為 `iot` 的「套件」，並以 `PUT` 方法傳送 HTTP 要求：
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## 啟動
{: #openwhisk_rest_api_activations}

若要取得最後三個「啟動」的清單，請利用 `GET` 方法來使用 HTTP 要求，並傳遞查詢參數 `limit=3`，如下所示：
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

若要取得「啟動」的所有詳細資料（包括結果和日誌），請利用 `GET`方法來傳送 HTTP 要求，並傳遞「啟動 ID」作為路徑參數，如下所示：
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}

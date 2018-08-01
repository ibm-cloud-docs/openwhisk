---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 一般概念
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} 觸發程式及規則將事件驅動功能帶給平台。來自外部及內部事件來源的事件是透過觸發程式進行傳送，而規則容許您的動作反應這些事件。
{: shortdesc}

## 何謂觸發程式？
{: #openwhisk_triggers_create}

觸發程式是某類別事件的具名通道。以下是觸發程式範例：
- 位置更新事件的觸發程式。
- 將文件上傳至網站的觸發程式。
- 送入電子郵件的觸發程式。

使用鍵值組字典，可以*發動*（啟動）觸發程式。此字典有時稱為*事件*。與動作相同，每次發動觸發程式都會導致**啟動 ID**。

使用者可以明確地發動觸發程式，或者由外部事件來源代表使用者發動觸發程式。
*資訊來源* 是一種簡便的方法，可配置外部事件來源來發動 {{site.data.keyword.openwhisk_short}} 所使用的觸發程式事件。請參閱下列範例「資訊來源」：
- {{site.data.keyword.cloudant}} 資料變更資訊來源，在每次新增或修改資料庫中的文件時，都會發動觸發程式事件。
- Git 資訊來源，其對 Git 儲存庫的每次確定發動觸發程式事件。

## 規則對觸發程式的影響？
{: #openwhisk_rules_use}

規則會建立一個觸發程式與一個動作的關聯，每次發動觸發程式時，都會呼叫對應的動作，並將觸發程式事件作為輸入。

運用一組適當的規則，單一觸發程式事件可能會呼叫多個動作，也可能會呼叫動作以作為多個觸發程式的事件的回應。

例如，請考慮具有下列動作的系統：
- `classifyImage` - 可偵測影像中的物件並進行分類的動作。
- `thumbnailImage` - 可建立影像縮圖版本的動作。

另外，假設有兩個事件來源正在發動下列觸發程式：
- `newTweet` - 在張貼新的推文時發動的觸發程式。
- `imageUpload` - 在將影像上傳至網站時發動的觸發程式。

您可以設定規則，以讓單一觸發程式事件呼叫多個動作，以及讓多個觸發程式呼叫相同的動作：
- `newTweet -> classifyImage` 規則。
- `imageUpload -> classifyImage` 規則。
- `imageUpload -> thumbnailImage` 規則。

這三個規則都會建立下列行為：
- 分類兩個推文中的影像。
- 分類已上傳的影像。
- 產生縮圖版本。

## 建立及發動觸發程式
{: #openwhisk_triggers_fire}

觸發程式可以在發生特定事件時發動，也可以手動發動。

例如，建立觸發程式來傳送使用者位置更新，以及手動發動觸發程式。
1. 輸入下列指令，以建立觸發程式：
  ```
  ibmcloud fn trigger create locationUpdate
  ```
  {: pre}

  輸出範例：
  ```
ok: created trigger locationUpdate
  ```
  {: screen}

2. 列出這組觸發程式，確認您已建立觸發程式。
  ```
  ibmcloud fn trigger list
  ```
  {: pre}

  輸出範例：
  ```
triggers
  /someNamespace/locationUpdate                            private
  ```
  {: screen}

  現在，已建立可發動事件的具名「通道」。

3. 接下來，請指定觸發程式名稱及參數，以發動觸發程式事件：
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  輸出範例：
  ```
ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

發動觸發程式時，如果沒有隨附可供比對的規則，則不會有可見的效果。
觸發程式無法在套件內建立，必須直接在**名稱空間**下建立。

## 使用規則建立觸發程式與動作的關聯
{: #openwhisk_rules_assoc}

規則是用來建立觸發程式與動作的關聯。每次發動觸發程式事件時，都會使用事件參數來呼叫動作。

舉例來說，只要張貼位置更新，就會建立一個規則來呼叫 `hello` 動作。
1. 建立名為 'hello.js' 且具有下列動作碼的檔案：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 確定觸發程式和動作已存在：
  ```
  ibmcloud fn trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. 下一步是建立規則。規則會在建立時啟用，表示它會立即可用來回應觸發程式的「啟動」。三個參數為：_規則名稱_、_觸發程式名稱_ 及_動作名稱_。
  ```
  ibmcloud fn rule create myRule locationUpdate hello
  ```
  {: pre}

  您隨時都可以選擇停用規則：
  ```
  ibmcloud fn rule disable myRule
  ```
  {: pre}

4. 發動 **locationUpdate** 觸發程式。每次發動事件時，都會使用事件參數來呼叫 **hello** 動作。
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  輸出範例：
  ```
ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. 檢查最新的「啟動」，來驗證已呼叫 **hello** 動作。
  ```
  ibmcloud fn activation list --limit 1 hello
  ```
  {: pre}

  輸出範例：
  ```
activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  現在，請查詢前一個指令輸出中所列出的啟動 ID：
  ```
  ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  輸出範例：
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  您會看到 **hello** 動作收到事件有效負載，並傳回預期的字串。

您可以建立多個規則，來建立相同觸發程式與不同動作的關聯。觸發程式及規則不能屬於套件。不過，規則可以與屬於套件的動作相關聯，例如：
  ```
  ibmcloud fn rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

您也可以搭配使用規則與序列。例如，某人可以建立透過規則 `anotherRule` 所啟動的動作序列 `recordLocationAndHello`。
  ```
  ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}

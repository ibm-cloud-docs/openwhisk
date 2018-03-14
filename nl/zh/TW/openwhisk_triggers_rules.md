---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 建立觸發程式及規則
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} 的「觸發程式」及「規則」將事件驅動功能帶給平台。來自外部及內部事件來源的事件是透過「觸發程式」進行傳送，而「規則」容許「動作」反應這些事件。
{: shortdesc}

## 建立觸發程式

{: #openwhisk_triggers_create}

觸發程式是某類別事件的具名通道。以下是「觸發程式」的範例：
- 位置更新事件的「觸發程式」。
- 將文件上傳至網站的「觸發程式」。
- 送入電子郵件的「觸發程式」。

使用鍵值組字典，可以*發動*（啟動）觸發程式。此字典有時稱為*事件*。與「動作」相同，每次發動「觸發程式」都會導致啟動 ID。

使用者可以明確地發動觸發程式，或者由外部事件來源代表使用者發動觸發程式。
*資訊來源* 是一種簡便的方法，可配置外部事件來源來發動 {{site.data.keyword.openwhisk_short}} 所使用的「觸發程式」事件。請參閱下列範例「資訊來源」：
- Cloudant 資料變更資訊來源，在每次新增或修改資料庫中的文件時，都會發動「觸發程式」事件。
- Git 資訊來源，其在每次確定至 Git 儲存庫時，都會發動「觸發程式」事件。

## 使用規則

{: #openwhisk_rules_use}

「規則」會建立一個「觸發程式」與一個「動作」的關聯，每次發動「觸發程式」，都會呼叫對應的「動作」，並將「觸發程式」事件作為輸入。

使用適當的規則集，單一「觸發程式」事件可能會呼叫多個「動作」，或者可能呼叫「動作」作為多個「觸發程式」之事件的回應。

例如，考量具有下列「動作」的系統：
- `classifyImage` - 可偵測影像中的物件並進行分類的「動作」。
- `thumbnailImage` - 可建立影像縮圖版本的「動作」。

另外，假設有兩個事件來源正在發動下列「觸發程式」：
- `newTweet` - 在張貼新的推文時發動的「觸發程式」。
- `imageUpload` - 在將影像上傳至網站時發動的「觸發程式」。

您可以設定規則，以便單一「觸發程式」事件呼叫多個「動作」，且有多個「觸發程式」呼叫相同的「動作」：
- `newTweet -> classifyImage` 規則。
- `imageUpload -> classifyImage` 規則。
- `imageUpload -> thumbnailImage` 規則。

這三個「規則」會建立下列行為： 
- 分類兩個推文中的影像。
- 分類已上傳的影像。
- 產生縮圖版本。

## 建立及發動觸發程式
{: #openwhisk_triggers_fire}

觸發程式可以在發生特定事件時發動，也可以手動發動。

例如，建立「觸發程式」來傳送使用者位置更新項目，並手動發動「觸發程式」。
1. 輸入下列指令，以建立「觸發程式」：
  ```
wsk trigger create locationUpdate
  ```
  {: pre}

  ```
ok: created trigger locationUpdate
  ```

2. 列出「觸發程式」集，確認您已建立觸發程式。
  ```
wsk trigger list
  ```
  {: pre}

  ```
triggers
  /someNamespace/locationUpdate                            private
  ```

  現在，已建立可發動事件的具名「通道」。

3. 接下來，指定「觸發程式」名稱和參數，以發動「觸發程式」事件：
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```

發動「觸發程式」時，如果沒有隨附可供比對的「規則」，則不會有可見的效果。
觸發程式無法在「套件」內建立，必須直接在「名稱空間」下建立。

## 使用規則來關聯觸發程式和動作
{: #openwhisk_rules_assoc}

規則用來建立「觸發程式」與「動作」的關聯。每次發動「觸發事件」時，都會使用事件參數來呼叫「動作」。

舉例來說，只要張貼位置更新，就會建立一個規則來呼叫「`hello` 動作」。
1. 使用如下所示的「動作」碼來建立 'hello.js' 檔案：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 確定「觸發程式」和「動作」已存在。
  ```
wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  wsk action update hello hello.js
  ```
  {: pre}

3. 下一步是建立規則。規則會在建立時啟用，表示它會立即可用來回應「觸發程式」的「啟動」。三個參數為：「規則」、「觸發程式」及「動作」的名稱。
  ```
  wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  您隨時都可以選擇停用「規則」。
  ```
  wsk rule disable myRule
  ```
  {: pre}

4. 發動「`locationUpdate` 觸發程式」。每次發動事件時，都會使用事件參數來呼叫「`hello` 動作」。
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```

5. 檢查最新的「啟動」，來驗證已呼叫「動作」。
  ```
wsk activation list --limit 1 hello
  ```
  {: pre}

  ```
activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  ```
wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  ```json
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```

  您會看到「`hello` 動作」收到事件有效負載，並傳回預期的字串。

您可以建立多個「規則」，使相同的「觸發程式」與不同的「動作」產生關聯。
「觸發程式」及「規則」不可以屬於「套件」。不過，「規則」可以與屬於「套件」的「動作」相關聯，例如：
  ```
  wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

您也可以搭配使用「規則」與序列。例如，某人可以建立透過規則 `anotherRule` 所啟動的「動作」序列 `recordLocationAndHello`。
  ```
  wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}

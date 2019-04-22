---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: trigger rules, triggers, actions, channel events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用觸發程式和規則來回應事件
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk}} 觸發程式及規則將事件驅動功能帶給平台。來自外部及內部事件來源的事件是透過觸發程式進行傳送，而規則容許您的動作反應這些事件。
{: shortdesc}

## 一般概念
{: #definitions}

### 觸發程式
{: #openwhisk_triggers_create}

觸發程式是某類別事件的具名通道。
{: shortdesc}

觸發程式是您要反應特定類型事件的宣告（不論是由使用者還是透過事件來源）。下列是觸發程式範例。
- 位置更新事件的觸發程式
- 將文件上傳至網站的觸發程式
- 送入電子郵件的觸發程式

您可以使用鍵值組字典來發動或啟動觸發程式。這個字典有時稱為事件。使用者可以明確地發動觸發程式，或者由外部事件來源代表使用者發動觸發程式。
與動作一樣，與規則相關聯的觸發程式的每一次發動都會導致啟動 ID。與規則沒有關聯的觸發程式在發動時看不見效果。

資訊來源是一種簡便的方法，可配置外部事件來源來發動 {{site.data.keyword.openwhisk_short}} 所使用的觸發程式事件。下列是資訊來源範例。
- {{site.data.keyword.cloudant}} 資料變更資訊來源，它會在每次新增或修改資料庫中的文件時發動觸發程式事件
- Git 資訊來源，它會在每次確定至 Git 儲存庫時發動觸發程式事件

### 規則
{: #openwhisk_rules_use}

規則會建立觸發程式與動作的關聯。
{: shortdesc}

每次發動觸發程式時，規則都會使用觸發事件作為輸入，並呼叫關聯的動作。運用一組適當的規則，單一觸發程式事件可能會呼叫多個動作，也可能會呼叫動作以作為多個觸發程式的事件的回應。

例如，考量具有下列動作的系統。
- `classifyImage` - 可偵測影像中的物件並進行分類的動作。
- `thumbnailImage` - 可建立影像縮圖版本的動作。

另外，假設有兩個事件來源正在發動下列觸發程式。
- `newTweet` - 在張貼新的推文時發動的觸發程式。
- `imageUpload` - 在將影像上傳至網站時發動的觸發程式。

您可以設定規則，讓單一觸發程式事件呼叫多個動作，以及讓多個觸發程式呼叫相同的動作。
- `newTweet -> classifyImage` 規則
- `imageUpload -> classifyImage` 規則
- `imageUpload -> thumbnailImage` 規則

這三個規則會建立下列行為。
- 分類兩個推文中的影像。
- 分類已上傳的影像。
- 產生縮圖版本。

## 建立通道事件的觸發程式
{: #openwhisk_triggers_fire}

下列步驟顯示如何建立範例觸發程式來傳送使用者位置更新，以及如何手動發動觸發程式。

1. 建立觸發程式。必須直接在名稱空間內建立觸發程式，而不能在套件內建立。
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    輸出範例：
    ```
    ok: created trigger locationUpdate
    ```
    {: screen}

2. 驗證觸發程式已建立。
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
    觸發程式是作為具名通道，可在其中發動事件。

3. 發動觸發程式事件。
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    因為沒有與此觸發程式相關聯的規則，所以傳遞的參數不會用來作為任何動作的輸入。輸出範例：
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

在下一節中，您可以建立規則，讓觸發程式與動作產生關聯。

## 使用規則建立觸發程式與動作的關聯
{: #openwhisk_rules_assoc}

規則是用來建立觸發程式與動作的關聯。每次發動觸發程式事件時，都會使用觸發程式事件的參數來呼叫動作。

建立 [`locationUpdate` 觸發程式](#openwhisk_triggers_fire)之後，下列步驟會顯示如何建立範例規則，每當張貼位置更新時，都會呼叫 `hello` 動作。

1. 建立名為 'hello.js' 且具有下列動作碼的檔案：
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: pre}

2. 建立 `hello` 動作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 建立 `myRule` 規則，讓 `locationUpdate` 觸發程式與 `hello` 動作產生關聯。必須直接在名稱空間內建立規則，而不能在套件內建立。
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. 發動 `locationUpdate` 觸發程式。每次觸發程式事件發生時，都會使用事件參數來呼叫 `hello` 動作。
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    輸出範例：
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. 檢查最新啟動記錄，來驗證已呼叫 `hello` 動作。
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

6. 從先前的指令輸出中，取得啟動 ID 的相關資訊。
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    輸出範例：
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    您會看到 `hello` 動作收到事件有效負載，並傳回預期的字串。

7. 若要停用規則，您可以執行下列指令。
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

您也可以使用規則來建立觸發程式與序列之間的關聯。例如，您可以建立稱為 `recordLocationAndHello` 的動作序列，它是由 `anotherRule` 規則所啟動：
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}

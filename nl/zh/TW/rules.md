---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

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



# 透過規則關聯觸發程式和動作
{: #rules}

每次發動觸發程式時，規則都會使用觸發事件作為輸入，並呼叫關聯的動作。使用適當的規則集時，可以透過單一觸發程式來呼叫多個動作，也可以呼叫一個動作以回應來自多個觸發程式的事件。
{: shortdesc}


## 透過使用者介面建立規則
{: #rules_ui}

在使用者介面中，系統已建立用於關聯動作和觸發程式的規則。
{: shortdesc}

建立動作或觸發程式或者存取其詳細資料時，可以選擇連接現有或新的動作或觸發程式。建立連線時，系統會建立規則，並以 `ACTION_NAME-TRIGGER_NAME` 格式對其命名。

在 CLI 中，可以執行 `ibmcloud fn rule list` 來驗證是否已建立該規則。


## 透過 CLI 建立規則
{: #rules_create}

規則是用來建立觸發程式與動作的關聯。每次發動觸發程式事件時，都會使用觸發程式事件的參數來呼叫動作。

開始之前，請先建立[動作](/docs/openwhisk?topic=cloud-functions-actions)和[觸發程式](/docs/openwhisk?topic=cloud-functions-triggers)。


建立規則以便建立觸發程式與動作的關聯。必須直接在名稱空間內建立規則，而不能在套件內建立。
    
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


若要停用規則，您可以執行下列指令。
    
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## 為動作序列建立規則
{: #rules_seq}

還可以使用規則將觸發程式與動作序列關聯在一起。

開始之前，請先建立[動作序列](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)和[觸發程式](/docs/openwhisk?topic=cloud-functions-triggers)。

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## 關聯多個觸發程式和動作
{: #rules_assoc}

可以透過為動作和觸發程式的每種組合建立一個規則，以便能使用不同的組合。動作和觸發程式無需按 1:1 的比例組合。

例如，假設使用下列動作。

|動作|說明|
| --- | --- |
|`classifyImage`|此動作用於偵測影像中的物件並對其進行分類。|
|`thumbnailImage`|此動作用於建立影像的縮圖版本。|

此外，假設有兩個事件來源將觸發下列觸發程式。

|觸發程式 (Trigger)|說明|
| --- | --- |
|`newTweet`|此觸發程式在張貼新推文時發動。|
|`imageUpload`|此觸發程式在將影像上傳到網站時發動。|

您可以設定規則，讓單一觸發程式事件呼叫多個動作，以及讓多個觸發程式呼叫相同的動作。
- `newTweet -> classifyImage` 規則
- `imageUpload -> classifyImage` 規則
- `imageUpload -> thumbnailImage` 規則

這三個規則會建立下列行為。
- 分類兩個推文中的影像。
- 分類已上傳的影像。
- 產生縮圖版本。


---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}

# 建立 Ballerina 動作
{: #ballerina-actions}

下列各節會引導您建立及呼叫單一 Ballerina 動作，並且將參數新增至該動作。動作是一個最上層 Ballerina 函數，可接受並傳回 JSON 物件。 

Ballerina 動作是在 Ballerina [0.990.2](https://ballerina.io/downloads) 中執行。您需要一個本端相容版本的編譯器，才能產生執行檔。沒有 Ballerina 編譯器，您就無法建立動作。

## 建立及呼叫 Ballerina 動作

**開始之前：**使用下列原始碼建立稱為 `hello.bal` 的檔案。

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

動作的進入方法依預設是 `main`。當您使用 `--main` 來建立具有 `wsk` CLI 的動作時，可以指定此變數。 

**附註：**Ballerina 編譯器預期有一個稱為 `main` 的函數會產生執行檔，因此原始檔必須包括稱為 `main` 的位置保留元。

若要建立稱為 `hello` 的動作，請完成下列步驟。

1. 產生 .balx 檔案。
  ```
  ballerina build hello.bal
  ```
{: pre}

2. 使用 .balx 檔案來建立動作。
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. CLI 還無法從原始檔副檔名判定動作類型。您必須明確地指定類型。對於 `.balx` 原始檔，動作會使用 Ballerina 0.990.2 運行環境來執行。
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

輸出範例。
```json
{
  "greeting": "Hello World!"
  }
```
{: screen}

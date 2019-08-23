---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: triggers, serverless, functions

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



# 為事件建立觸發程式
{: #triggers}

觸發程式是您要反應特定類型事件的宣告（不論是由使用者還是透過事件來源）。
{: shortdesc}

以下是觸發程式範例。
- 位置更新事件
- 文件上傳到網站
- 傳入電子郵件



## 透過 CLI 建立觸發程式
{: #triggers_create}


1. 建立觸發程式。必須直接在名稱空間內建立觸發程式，而不能在套件內建立。
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    **輸出範例**
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. 驗證觸發程式已建立。
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    **輸出範例**
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



接下來，可以[測試觸發程式](/docs/openwhisk?topic=cloud-functions-test#test_triggers)或[建立規則](/docs/openwhisk?topic=cloud-functions-rules)，以將觸發程式與動作關聯在一起。



## 資訊來源與觸發程式的差異
{: #triggers_difference}

資訊來源及觸發程式密切相關，但技術上而言是不同的概念。

- {{site.data.keyword.openwhisk_short}} 會處理流入系統的**事件**。

- **觸發程式**是指某類別事件的名稱。每個事件都只屬於一個觸發程式；若用類比說明，觸發程式類似於以主題為基礎之發佈/訂閱系統中的主題。**規則**表示每當來自觸發程式的事件到達時，透過觸發程式有效負載呼叫動作。

- **資訊來源**是一種簡便的方法，可配置外部事件來源來發動 {{site.data.keyword.openwhisk_short}} 所使用的觸發程式事件。資訊來源是全部屬於某個觸發程式的事件的串流。預先安裝的套件、可安裝的套件和您自己的自訂套件可能包含資訊來源。資訊來源透過**資訊來源動作**進行控制，資訊來源動作用於處理組成資訊來源的事件串流的建立、刪除、暫停和繼續。資訊來源動作一般會使用管理通知的 REST API，與產生事件的外部服務互動。

資訊來源範例：
- {{site.data.keyword.cloudant}} 資料變更資訊來源，它會在每次新增或修改資料庫中的文件時發動觸發程式事件
- Git 資訊來源，它會在每次確定至 Git 儲存庫時發動觸發程式事件



## 為資訊來源建立觸發程式
{: #triggers_feeds}

此範例顯示如何使用「警示」套件中的資訊來源每分鐘發動觸發程式一次，以及如何使用規則每分鐘呼叫動作一次。

1. 取得 `/whisk.system/alarms` 套件中實體的說明清單。

    ```
  ibmcloud fn package get --summary /whisk.system/alarms
  ```
    {: pre}

    **輸出範例**
    ```
    package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
    {: screen}
2. 取得 `/whisk.system/alarms` 套件中資訊來源的說明，以查看可以使用的參數。

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  **輸出範例**
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` 資訊來源接受兩個參數：
  - `cron`：何時發動觸發程式的 crontab 規格。
  - `trigger_payload`：要在每一個觸發程式事件中設定的有效負載參數值。

2. 建立每一分鐘發動一次的觸發程式。
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  **輸出範例**
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. 建立應用程式。範例 `hello.js`：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. 建立動作。
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. 建立規則，以在每次 `everyOneMinute` 觸發程式發動時呼叫 `hello` 動作。
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  **輸出範例**
  ```
  ok: created rule myRule
  ```
  {: screen}

6. 確認是透過輪詢啟動日誌來呼叫動作。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  您可以看到針對觸發程式、規則和動作，每一分鐘啟動一次。在每次呼叫時，動作都會接收到參數 `{"name":"Mork", "place":"Ork"}`。




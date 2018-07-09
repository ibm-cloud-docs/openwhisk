---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant
{: #cloudant_actions}

`/whisk.system/cloudant` 套件可讓您使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) 資料庫，並包括下列動作及「資訊來源」：

|實體|類型|參數|說明
|
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |套件|dbname、host、username、password|使用 Cloudant 資料庫。|
|`/whisk.system/cloudant/read` |動作|dbname、id|讀取資料庫中的文件。|
|`/whisk.system/cloudant/write` |動作|dbname、overwrite、doc|將文件寫入資料庫。|
|`/whisk.system/cloudant/changes` |資訊來源|dbname、filter、query_params、maxTriggers|在資料庫變更時發動觸發程式事件。|
{: shortdesc}

下列各節將引導您逐步設定 {{site.data.keyword.cloudant_short_notm}} 資料庫，並且說明如何在其中讀取及寫入。
如需如何使用「資訊來源」與 `/whisk.system/cloudant` 套件搭配的相關資訊，請參閱 [{{site.data.keyword.cloudant_short_notm}} 事件來源](./openwhisk_cloudant.html)。

## 在 {{site.data.keyword.Bluemix_notm}} 中設定 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudantdb_cloud}

如果您是從 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk}}，則 {{site.data.keyword.openwhisk_short}} 會自動為 {{site.data.keyword.cloudant_short_notm}} 服務實例建立套件連結。如果您不是從 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}} 及 {{site.data.keyword.cloudant_short_notm}}，請跳到下一節。

1. 在 [{{site.data.keyword.Bluemix_notm}}儀表板](http://console.bluemix.net)中，建立 {{site.data.keyword.cloudant_short_notm}} 服務實例。

  務必為每一個新的服務實例建立認證金鑰。

2. 重新整理名稱空間中的套件。重新整理會自動為每一個 {{site.data.keyword.cloudant_short_notm}} 服務實例建立一個套件連結，並定義認證金鑰。
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  輸出範例：
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud wsk package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  您的套件連結現在包含與 {{site.data.keyword.cloudant_short_notm}} 服務實例相關聯的認證。

3. 確認先前建立的套件連結是使用 {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}} 服務實例主機及認證進行配置。

  ```
  ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  輸出範例：
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
      ]
  ```
  {: screen}

## 在 {{site.data.keyword.Bluemix_notm}} 外部設定 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudantdb_nocloud}

如果您未在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或如果要在 {{site.data.keyword.Bluemix_notm}} 外部設定 {{site.data.keyword.cloudant_short_notm}} 資料庫，則必須手動建立 {{site.data.keyword.cloudant_short_notm}} 帳戶的套件連結。您需要 {{site.data.keyword.cloudant_short_notm}} 帳戶主機名稱、使用者名稱及密碼。

1. 建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的套件連結。
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. 驗證套件連結已存在。
  ```
wsk package list
  ```
  {: pre}

  輸出範例：
  ```
packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## 讀取 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudant_read}

您可以使用動作，從稱為 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 資料庫中提取文件。請確定此資料庫存在於 {{site.data.keyword.cloudant_short_notm}} 帳戶中。

- 使用您先前建立的套件連結中的 **read** 動作，來提取文件。請務必將 `/_/myCloudant` 取代為您的套件名稱。
  ```
  ibmcloud wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  輸出範例：
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

## 寫入 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudant_write}

您可以使用動作，將文件儲存至稱為 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 資料庫。請確定此資料庫存在於 {{site.data.keyword.cloudant_short_notm}} 帳戶中。

1. 使用您先前建立的套件連結中的 **write** 動作，來儲存文件。請務必將 `/_/myCloudant` 取代為您的套件名稱。
  ```
  ibmcloud wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  輸出範例：
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 在 {{site.data.keyword.cloudant_short_notm}} 儀表板中瀏覽到文件，以驗證文件已存在。

  **testdb** 資料庫的儀表板 URL 可能與下面類似：`https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`。

---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloudant 套件
{: #cloudant_actions}

`/whisk.system/cloudant` 套件可讓您使用 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 資料庫，並包括下列動作及資訊來源：

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |套件|dbname、host、username、password|使用 Cloudant 資料庫。|
|`/whisk.system/cloudant/read` |動作|dbname、id|讀取資料庫中的文件。|
|`/whisk.system/cloudant/write` |動作|dbname、overwrite、doc|將文件寫入資料庫。|
|`/whisk.system/cloudant/changes` |資訊來源| dbname、iamApiKey、iamUrl、filter、query_params、maxTriggers |在資料庫變更時發動觸發程式事件。|
{: shortdesc}

下列各節將引導您逐步設定 {{site.data.keyword.cloudant_short_notm}} 資料庫，並且說明如何在其中讀取及寫入。如需如何使用資訊來源與 `/whisk.system/cloudant` 套件搭配的相關資訊，請參閱 [{{site.data.keyword.cloudant_short_notm}} 事件來源](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant)。

## 在 {{site.data.keyword.Bluemix_notm}} 中設定 {{site.data.keyword.cloudant_short_notm}} 資料庫
{: #cloudantdb_cloud}

如果您從 {{site.data.keyword.Bluemix_notm}} 使用 {{site.data.keyword.openwhisk}}，則可以使用 [{{site.data.keyword.openwhisk}} CLI 外掛程式](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)，將服務連結至動作或套件。

您必須先手動建立 {{site.data.keyword.cloudant_short_notm}} 帳戶的套件連結。

1. 建立針對 {{site.data.keyword.cloudant_short_notm}} 帳戶所配置的套件連結。
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 驗證套件連結已存在。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}
  
3. 取得您要連結至動作或套件之服務實例的名稱。
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    輸出範例：
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. 取得針對您在前一個步驟中所取得之服務實例而定義的認證名稱。
    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    輸出範例：
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. 將服務連結至您在步驟 1 中建立的套件。
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1' 
    ```
    {: pre}

6. 驗證認證已順利連結。
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
            {
                "key": "bluemixServiceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
                "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

    在此範例中，Cloudant 服務的認證屬於名為 `__bx_creds` 的參數。
  

## 從 {{site.data.keyword.cloudant_short_notm}} 資料庫中讀取
{: #cloudant_read}

您可以使用動作，從稱為 **testdb** 的 {{site.data.keyword.cloudant_short_notm}} 資料庫中提取文件。請確定此資料庫存在於 {{site.data.keyword.cloudant_short_notm}} 帳戶中。

- 使用您先前建立的套件連結中的 **read** 動作，來提取文件。請務必將 `/_/myCloudant` 取代為您的套件名稱。
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
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
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
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

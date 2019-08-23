---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"

keywords: services, serverless, functions

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


# 將 {{site.data.keyword.cloud_notm}} 服務連結到 {{site.data.keyword.openwhisk_short}} 實體
{: #services}

您可以在 {{site.data.keyword.openwhisk_short}} 應用程式中包含 IBM Cloud 服務中的功能。
{: shortdesc}

**如何將 IBM Cloud 服務新增至我的應用程式？**

1. 可以將 REST API 呼叫寫到應用程式的程式中。此選項可能是與 IBM Cloud 服務進行通訊的最快方法。
2. 可以使用預先安裝或可安裝的套件來合併功能。您可以在應用程式程式碼內使用儲存在套件中的動作和資訊來源。此選項可能會略為精簡程式碼，如果應用程式已接近系統限制，此選項可能很有用。


**如何設定必須由應用程式存取的參數？**

這些參數可能包含使應用程式可重複使用不同資料的值，也可能包含服務需要的值（例如，認證）。 
1. 可以將參數寫到應用程式的程式中。此選項可能並不是儲存機密資訊（如認證）的最安全方式。
2. 可以透過將參數連結到動作或套件，以便能將這些參數連結到應用程式。


## 將服務連結至動作或套件
{: #services_bind}

將任何 {{site.data.keyword.cloud_notm}} 服務連結到任何動作。連結服務時，會在現有動作上建立包含服務實例認證的新參數。

不能將相同服務的多個實例連結到一個動作或套件。只能連結服務的一個實例。
{: note}

開始之前，為要連結到動作的服務[建立動作](/docs/openwhisk?topic=cloud-functions-actions)並[定義認證](/docs/resources?topic=resources-externalapp#externalapp)。

1. 取得要連結到動作或套件的服務和服務實例的名稱。在範例輸出中，`composer` 是服務，`Composer-qp` 是服務實例名稱。
    ```
    ibmcloud service list
    ```
    {: pre}

    **輸出範例**
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 取得為服務實例所定義的認證的名稱。
    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    **範例**
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    **輸出範例**
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
    Credentials-1
    Credentials-2
    ```
    {: screen}

3. 將服務連結至動作。
    `ibmcloud fn service bind` 指令可使您的 {{site.data.keyword.cloud_notm}} 服務認證在運行環境可用於 {{site.data.keyword.openwhisk_short}} 程式碼。
    下列指令參數可用於 `ibmcloud fn service bind` 指令。

    <table>
    <thead>
        <tr>
        <th> 參數 </th>
        <th>說明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><code>SERVICE</code></td>
        <td>要連結的服務名稱。</td>
        </tr>
        <tr>
        <td><code>ACTION_NAME</code></td>
        <td>您要連結服務之動作或套件的名稱。</td>
        </tr>
        <tr>
        <td><code>--instance INSTANCE_NAME</code></td>
        <td>（選用）指定服務實例名稱。如果您未指定服務實例名稱，則會選取服務的第一個實例。</td>
        </tr>
        <tr>
        <td><code>--keyname CREDENTIALS_NAME</code></td>
        <td>（選用）指定認證名稱。如果未指定認證名稱，則將選取服務實例的第一個認證。</td>
        </tr>
    </tbody>
    </table>

    **範例語法**
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME][--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    例如，要將 {{site.data.keyword.ibmwatson}} Composer 服務連結到名稱為 `hello` 的動作，請執行下列指令。
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    **輸出**
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. 驗證認證已順利連結。服務所連結的動作不支援任何自訂旗標，但支援除錯及詳細旗標。

    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got action Hello World
    {
        "parameters": [
            {
                "key": "var1",
                "value": "val1"
            },
            {
                "key": "dog",
                "value": "cat"
            },
            {
                "key": "__bx_creds",
                "value": {
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    在此範例中，Composer 服務的認證（以及其他服務類型的其他任何認證）屬於名稱為 `__bx_creds` 的參數。動作會尋找 `__bx_creds` 連結參數，並移除所列服務類型的參照。如果該服務類型是唯一列出的服務類型，則動作會將 `__bx_creds` 參數值設為空值。如果有多個服務連結至動作，則會保留 `__bx_creds` 參數與任何仍連結的服務。

如需將參數傳遞到動作或套件的相關資訊，請參閱[將參數連結到動作](/docs/openwhisk?topic=cloud-functions-actions#actions_params)。

## 取消服務與動作的連結
{: #services_unbind}

取消服務與動作或套件的連結會移除現有服務連結。

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}


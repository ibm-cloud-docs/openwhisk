---

copyright:
  years: 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# 將服務連結至動作
{: #binding_services}

您可以使用 [{{site.data.keyword.openwhisk}} CLI 外掛程式](./bluemix_cli.html)，將服務連結至動作或套件。{{site.data.keyword.openwhisk_short}} `ibmcloud fn service bind` 指令，讓您的 {{site.data.keyword.Bluemix_notm}} 服務認證可供您的 {{site.data.keyword.openwhisk_short}} 程式碼在運行環境使用。
{: shortdesc}

不要混淆 `ibmcloud fn service bind` 指令與 Cloud Foundry 中可用的 `cf bind-service` 指令。
{: tip}

## 將服務連結至動作或套件
{: #cli_bind}

將任何 {{site.data.keyword.Bluemix_notm}} 服務連結至 {{site.data.keyword.openwhisk_short}} 中所定義的任何動作。連結服務時，會在包含服務實例認證的現有動作上建立新參數。

**附註**：每一種類型只能有一個服務連結到動作或套件。不支援連結多個相同類型的服務。

開始之前，請為您要連結的服務[定義認證](/docs/apps/reqnsi.html#accser_external)。

1. 取得您要連結至動作或套件之服務實例的名稱。
    ```
    ibmcloud service list
    ```
    {: pre}

    輸出範例：
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 取得針對您在前一個步驟中所取得之服務實例而定義的認證名稱。
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    輸出範例：
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
    Credentials-1
    Credentials-2
    ```
    {: screen}

3. 將服務連結至動作。
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>瞭解 <code>ibmcloud fn service bind</code> 指令元件</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="「構想」圖示"/> 瞭解 <code>ibmcloud fn service bind</code> 指令元件</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>您要連結的服務類型。</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>您要連結服務之動作或套件的名稱。</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>選用項目：指定服務實例名稱。如果您未指定服務實例名稱，則會選取服務的第一個實例。</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>選用項目：指定認證集名稱。如果您未指定認證集名稱，則會選取服務實例的第一個認證集。</td>
    </tr>
    </tbody></table>

    例如，若要將 {{site.data.keyword.ibmwatson}} Conversation 服務連結至名為 `hello` 的動作，請執行下列指令：
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. 驗證認證已順利連結。服務所連結的動作不支援任何自訂旗標，但支援除錯及詳細旗標。
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    輸出範例：
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
                    "conversation": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Conversation-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    在此範例中，Conversation 服務的認證以及其他服務類型的任何其他認證，都屬於名為 `__bx_creds` 的參數。動作會尋找 `__bx_creds` 連結參數，並移除所列服務類型的參照。如果該服務類型是唯一列出的服務類型，則動作會將 `__bx_creds` 參數值設為空值。如果有多個服務連結至動作，則會保留 `__bx_creds` 參數與任何仍連結的服務。

如需將參數傳遞給動作或套件以及 `update` 作業期間對認證之影響的相關資訊，請參閱[使用參數](./parameters.html#pass-params-action)。


## 取消服務與動作或套件的連結
{: #cli_unbind}

取消服務與動作或套件的連結。取消連結服務時會移除 `service bind` 指令所建立的現有連結。

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: services, serverless

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
{:gif: data-image-type='gif'}


# IBM Cloud サービスの追加
{: #services}

アプリで IBM Cloud サービスの組み込み機能を使用することができます。
{: shortdesc}

**IBM Cloud サービスを自分のアプリに追加するにはどうすればよいですか?**

1. REST API 呼び出しをアプリにハードコーディングできます。IBM Cloud サービスと通信するには、この方法が最も早い可能性があります。
2. 事前にインストールされているパッケージまたはインストール可能パッケージを使用して、機能を組み込むことができます。それらのパッケージに入っているアクションとフィードを、アプリのコード内で使用できます。この方法を使用するとコードのサイズが若干抑えられるので、システム限度に近いアプリにとっては有用です。


**アプリで利用する必要があるパラメーターをセットアップするにはどうすればよいですか?**

こうしたパラメーターには、データを変えてアプリを再使用できるような値や、サービスが必要とする資格情報などの値があります。 
1. パラメーターをアプリにハードコーディングできます。この方法は、資格情報などの機密情報を保管するにはあまり安全な方法とは言えません。
2. パラメーターをアクションまたはパッケージにバインドして、アプリにバインドできます。


## アクションまたはパッケージへのサービスのバインド
{: #services_bind}

任意の {{site.data.keyword.Bluemix_notm}} サービスを任意のアクションにバインドします。サービスをバインドすると、サービス・インスタンスの資格情報を含む新しいパラメーターが既存のアクションに作成されます。

**注**: 同一サービスの複数のインスタンスを 1 つのアクションまたはパッケージにバインドすることはできません。1 つのサービスの 1 つのインスタンスのみをバインドできます。 

始めに、[アクションを作成](/docs/openwhisk?topic=cloud-functions-actions)し、そのアクションにバインドするサービスの[資格情報を定義](/docs/resources?topic=resources-externalapp#externalapp)します。

1. アクションまたはパッケージにバインドするサービスおよびサービス・インスタンスの名前を取得します。以下の出力例では、`composer` がサービスで、`Composer-qp` がサービス・インスタンス名です。
    ```
    ibmcloud service list
    ```
    {: pre}

    出力例:
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. サービス・インスタンスに定義されている資格情報の名前を取得します。

    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    例:
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    出力例:
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. サービスをアクションにバインドします。 {{site.data.keyword.openwhisk_short}} `ibmcloud fn service bind` コマンドによって、実行時に {{site.data.keyword.Bluemix_notm}} サービス資格情報を {{site.data.keyword.openwhisk_short}} コードで使用できるようになります。
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    <table>
    <caption><code>ibmcloud fn service bind</code> コマンド・コンポーネントの説明</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>ibmcloud fn service bind</code> コマンド・コンポーネントの説明</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE</code></td>
    <td>バインドするサービス名。</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>サービスをバインドする対象のアクションまたはパッケージの名前。</td>
    </tr>
    <tr>
    <td>--instance <code>INSTANCE_NAME</code></td>
    <td>オプション: サービス・インスタンス名を指定します。 サービス・インスタンス名を指定しない場合は、サービスの最初のインスタンスが選択されます。</td>
    </tr>
    <tr>
    <td>--keyname <code>CREDENTIALS_NAME</code></td>
    <td>オプション: 資格情報名を指定します。資格情報名を指定しない場合、サービス・インスタンスの最初の資格情報が選択されます。</td>
    </tr>
    </tbody></table>

    例えば、{{site.data.keyword.ibmwatson}} Composer サービスを `hello` というアクションにバインドするには、次のようにします。
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    出力:
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. 資格情報が正常にバインドされたことを確認します。 サービスがバインドされた先のアクションでは、カスタム・フラグはサポートされませんが、デバッグ・フラグおよび詳細フラグはサポートされます。
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    出力例:
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

    この例では、Composer サービスの資格情報が、他のサービス・タイプの資格情報とともに、`__bx_creds` という名前のパラメーターに含まれています。アクションは、`__bx_creds` バインド済みパラメーターを検索し、リストされているサービス・タイプへの参照を削除します。 当該サービス・タイプがリストされている唯一のものである場合、アクションは、`__bx_creds` パラメーターの値をヌルにします。 複数のサービスがアクションにバインドされている場合、`__bx_creds` パラメーターは、まだバインドされているサービスに残ります。

アクションまたはパッケージにパラメーターを渡す方法について詳しくは、[アクションへのパラメーターのバインド](/docs/openwhisk?topic=cloud-functions-actions#actions_params)を参照してください。




## アクションからのサービスのアンバインド
{: #services_unbind}

サービスをアクションまたはパッケージからアンバインドすると、既存のサービス・バインディングが削除されます。

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

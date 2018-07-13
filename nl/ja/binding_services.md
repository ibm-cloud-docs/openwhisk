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

# アクションへのサービスのバインド
{: #binding_services}

[{{site.data.keyword.openwhisk}} CLI プラグイン](./bluemix_cli.html)を使用して、サービスをアクションまたはパッケージにバインドできます。 {{site.data.keyword.openwhisk_short}} `ibmcloud wsk service bind` コマンドよって、実行時に {{site.data.keyword.Bluemix_notm}} サービス資格情報を {{site.data.keyword.openwhisk_short}} コードで使用できるようになります。
{: shortdesc}

`ibmcloud wsk service bind` コマンドを、Cloud Foundry で使用可能な `cf bind-service` コマンドと混同しないようにしてください。
{: tip}

## アクションまたはパッケージへのサービスのバインド
{: #cli_bind}

任意の {{site.data.keyword.Bluemix_notm}} サービスを {{site.data.keyword.openwhisk_short}} に定義されている任意のアクションにバインドできます。サービスをバインドすると、既存のアクションにサービス・インスタンス資格情報を含む新規パラメーターが作成されます。

**注**: アクションまたはパッケージにバインドできる各タイプのサービスは 1 つのみです。同じタイプの複数のサービスをバインドする操作はサポートされません。

開始する前に、バインドするサービスの[資格情報を定義](/docs/apps/reqnsi.html#accser_external)します。

1. アクションまたはパッケージにバインドするサービス・インスタンスの名前を取得します。
    ```
    ibmcloud service list
    ```
    {: pre}

    出力例:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 前のステップで取得したサービス・インスタンスに定義された資格情報の名前を取得します。
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    出力例:
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. サービスをアクションにバインドします。
    ```
    ibmcloud wsk service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption><code>ibmcloud wsk service bind</code> コマンド・コンポーネントの説明</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>ibmcloud wsk service bind</code> コマンド・コンポーネントの説明</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>バインドしようとしているサービスのタイプ。</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>サービスをバインドする対象のアクションまたはパッケージの名前。</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>オプション: サービス・インスタンス名を指定します。サービス・インスタンス名を指定しない場合は、サービスの最初のインスタンスが選択されます。</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>オプション: 資格情報セット名を指定します。資格情報セット名を指定しない場合は、サービス・インスタンスの最初の資格情報セットが選択されます。</td>
    </tr>
    </tbody></table>

    例えば、{{site.data.keyword.ibmwatson}} Conversation サービスを `hello` というアクションにバインドするには、次のようにします。
    ```
    ibmcloud wsk service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. 資格情報が正常にバインドされたことを確認します。サービスがバインドされた先のアクションでは、カスタム・フラグはサポートされませんが、デバッグ・フラグおよび詳細フラグはサポートされます。
    ```
    ibmcloud wsk action get hello parameters
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

    この例では、conversation サービスの資格情報は、他のサービス・タイプのその他の資格情報とともに、`__bx_creds` という名前のパラメーターに属しています。アクションは、`__bx_creds` バインド済みパラメーターを検索し、リストされているサービス・タイプへの参照を削除します。 当該サービス・タイプがリストされている唯一のものである場合、アクションは、`__bx_creds` パラメーターの値をヌルにします。 複数のサービスがアクションにバインドされている場合、`__bx_creds` パラメーターは、まだバインドされているサービスに残ります。

アクションまたはパッケージへのパラメーターの引き渡し、および資格情報が `update` 操作時に受ける影響について詳しくは、[パラメーターの処理](./parameters.html#pass-params-action)を参照してください。


## アクションまたはパッケージからのサービスのアンバインド
{: #cli_unbind}

サービスをアクションまたはパッケージからアンバインドします。 サービスをアンバインドすると、`service bind` コマンドによって作成された既存のバインディングが削除されます。

```
ibmcloud wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

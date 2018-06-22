---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# アクションからのサービスの使用
{: #binding_services}

[{{site.data.keyword.openwhisk}} CLI プラグイン](./bluemix_cli.html)を利用して、サービスをアクションにバインドできます。{{site.data.keyword.openwhisk_short}} には、`service bind` コマンドが用意されています。このコマンドは、{{site.data.keyword.Bluemix}} サービス資格情報を、実行時に Cloud Functions コードで使用できるようにします。`service bind` コマンドは、Cloud Foundry で使用可能な `cf bind-service` コマンドと混同しないようにしてください。それは単に、既存のアクションでサービス資格情報が含まれた新規パラメーターを作成する自動化手段です。{{site.data.keyword.openwhisk_short}} `service bind` コマンドは、より柔軟であり、{{site.data.keyword.openwhisk_short}} で定義された任意のアクションに任意の {{site.data.keyword.Bluemix_notm}} サービスをバインドできます。唯一の注意として、バインドするサービスに対して資格情報が定義されている必要があります。
{: shortdesc}

## サービスをアクションにバインドする方法
{: #cli_bind}

[{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) CLI プラグインで提供されている `ibmcloud wsk service bind` コマンドを使用して、サービスをアクションにバインドします。『[制限](./binding_services.html#limitations)』セクションに、追加情報があります。

`bind` を使用した使用法の構文:
```
ibmcloud wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

`service bind` コマンドでは、サービス・タイプとバインド先のアクション名が必要です。例えば、Watson 会話サービスを `hello` というアクションにバインドする場合、呼び出しは、以下のようなコマンドになります。
```
ibmcloud wsk service bind conversation hello
```
{: pre}

これにより、以下の出力が生成されます。
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

このコマンドは、現行スペースで既存の Watson 会話サービスを検索し、検出された最初の会話サービスを対象にし、そのサービスに属するすべての資格情報を取得します。そのサービスに属している最初の資格情報セットを使用して、指定された `hello` アクションに対してパラメーターとして当該資格情報をバインドします。出力では、正確にどのサービスにアクションがバインドされたのか、およびそのサービスのどの資格情報セットがバインドに使用されたのかが示されます。

資格情報が正常にバインドされたことを確認するために、以下のコマンドを発行します。
```
ibmcloud wsk action get hello parameters
```
{: pre}

サンプル出力:
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

ここから、この会話サービスの資格情報が (他のサービス・タイプの他の資格情報とともに) `__bx_creds` という名前のパラメーターに属しており、他のバインドされたパラメーターを使用できるのと同様に、アクション・コード内から使用できるようになったことが分かります。アクションは、当該サービスで定義されている最初の資格情報セットが含まれた最初の使用可能な会話サービスを選択します。 

アクションへのパラメーターの受け渡し、および `action update` 操作の実行時における資格情報への影響について詳しくは、資料『[アクションの作成と起動](openwhisk_actions.html#openwhisk_pass_params)』を参照してください。

`wsk service` コマンドでは、以下の 2 つのフラグがサポートされます。

<dl>
    <dt>--instance</dt>
    <dd>使用するタイプの特定のサービスの名前。</dd>
    <dt>--keyname</dt>
    <dd>使用するサービス内の特定の資格情報の名前。</dd>
</dl>

上記フラグの使用方法の説明については、以下の例を参照してください。前述の `ibmcloud wsk service bind` コマンドを使用したときに、実際に 2 つの会話サービスが存在し、アクションのデフォルトで正しくないサービスと資格情報がバインドされたものとします。`--instance` フラグおよび `--keyname` フラグを指定してコマンドを再実行することで、正しいサービスを正しいアクションにバインドできるようになります。まず、どのサービスが使用可能で、どの資格情報が各サービスにバインドされているのかを確認します。サービスをリストすると、以下のような出力が表示されます。

```
ibmcloud service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

この出力から、**Conversation-qp** が、リストされている 2 つのサービスの最初のものであり、それが最初の `ibmcloud wsk service bind conversation hello` コマンドでのバインド先になったことが分かります。そうではなく、**Conversation-uc** サービスにバインドしたいものとします。そのため、念を入れて、どの資格情報が **Conversation-uc** に含まれているのかを確認することで、確実に適切な資格情報セットを使用してバインドが行われるようにすることができます。

```
ibmcloud service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

このサービスの「Credentials-2」にバインドしたいと考えています。アクションが目的の動作を確実に実行するように、以下のコマンドを実行します。
```
ibmcloud wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

これにより、以下の出力が生成されます。
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

出力から、正しい資格情報セットがアクションにバインドされたことが分かります。ここでも、確認するために、以下の `ibmcloud wsk action get` コマンドを表示できます。
```
ibmcloud wsk action get hello parameters
```
{: pre}

これにより、以下の結果が生成されます。
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

通常のデバッグ・フラグがサポートされ、呼び出しから応答ヘッダーが出力されます。

## サービスをアクションからアンバインドする方法
{: #cli_unbind}

サービスをアクションからアンバインドするには、`ibmcloud wsk service unbind` を使用します。`service unbind` コマンドは、`service bind` コマンドで作成された既存のバインディングを削除します。

`unbind` を使用した使用法の構文:
```
ibmcloud wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## 制限
{: #limitations}

`service` アクションでは、カスタム・フラグはサポートされませんが、通常のデバッグ・フラグおよび詳細フラグはサポートされます。アクションは、`__bx_creds` バインド済みパラメーターを検索し、リストされているサービス・タイプへの参照を削除します。当該サービス・タイプがリストされている唯一のものである場合、アクションは、`__bx_creds` パラメーターの値をヌルにします。複数のサービスがアクションにバインドされている場合、`__bx_creds` パラメーターは、まだバインドされているサービスに残ります。

アクションにバインドできる各タイプのサービスは 1 つのみです。単一のアクション内に同じタイプの複数のサービスをバインドする操作はサポートされません。
{: tip}


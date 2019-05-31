---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

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



# 名前空間の管理
{: #namespaces}

{{site.data.keyword.openwhisk}} では、Identity and Access (IAM) 管理の名前空間を作成して、アクションやトリガーなどのエンティティーをグループにまとめることができます。そして、それらの名前空間に対する IAM アクセス・ポリシーを作成できます。
{: shortdesc}


**名前空間とは何ですか?**

名前空間は、アクションやトリガーなどの {{site.data.keyword.openwhisk_short}} エンティティーを入れるものであり、リソース・グループに属しています。名前空間に対するアクセス権限をユーザーに付与すると、そのユーザーがエンティティーにアクセスできるようになります。

エンティティーの完全修飾名は、
`/namespaceName/[packageName]/entityName` です。


**名前空間を作成するとどうなりますか?**

{{site.data.keyword.openwhisk_short}} 内に作成された名前空間は、IAM サービス・インスタンスとして識別されます。
名前空間の作成時に、このサービス・インスタンスを追加する[リソース・グループ](/docs/resources?topic=resources-rgs)を指定できます。

名前空間を作成すると、同時に以下の成果物も作成されます。

* サービス ID。アウトバウンド・コールを行うときに機能 ID として使用できます。この名前空間に作成するすべてのアクションは、このサービス ID を使用して他のリソースにアクセスできます。すべてのサービス ID を参照するには、`ibmcloud iam service-ids` を実行します。

* サービス ID の API キー。これを使用して IAM トークンを生成できます。そのトークンを使用すれば、この名前空間を他の {{site.data.keyword.Bluemix_notm}} サービスで認証できます。API キーは、環境変数としてアクションに対して提供されます。

    API キーは、削除しないでください。
    {: tip}

**名前空間に関する制限はありますか?**

[API ゲートウェイを使用した API の作成](/docs/openwhisk?topic=cloud-functions-apigateway)と[モバイル SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) の使用は、IAM 管理の名前空間ではサポートされていません。

{{site.data.keyword.openwhisk_short}} には、名前空間の名前に対する制限があります。 詳しくは、[システムの詳細および制限](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov)の資料を参照してください。
{: tip}



**Cloud Foundry ベースの名前空間がある場合はどうすればよいですか?**

Cloud Foundry ベースの名前空間も正常に機能します。しかし、新機能を利用するには、[名前空間を IAM にマイグレーション](/docs/resources?topic=resources-migrate)する必要があります。

</br>


## CLI での名前空間の作成
{: #namespaces_create}

IAM 管理の名前空間をリソース・グループの一部として作成して、名前空間の作成時にリソース・グループをターゲットにすることによってリソースのアクセス・ポリシーを管理できます。 お客様の名前空間に対するアクセス権限を必要とする他のユーザーがいる場合や、お客様が自分の名前空間のアクションから他のリソースにアクセスする場合には、名前空間の作成後に IAM ポリシーを設定してください。
{: shortdesc}

1. 名前空間を作成するリソース・グループをターゲットに指定します。 [リソース・グループ](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)をまだ作成していない場合は、`default` グループをターゲットに指定できます。

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. IAM 対応の名前空間を作成します。

  ```
  ibmcloud fn namespace create <namespace_name> [-n <description>]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>IAM ベースの名前空間の表示名。</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>オプション: 名前空間に説明 (例えば、それに含まれることになるアクションやパッケージの種類など) を追加します。</td>
      </tr>
    </tbody>
  </table>

  出力例:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. 新しい名前空間が作成されたことを確認します。

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  出力例:

  ```
  Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  IAM ベースの名前空間や Cloud Foundry ベースの名前空間など、すべての名前空間をリストすることもできます。

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 名前空間にエンティティーを作成する前に、その名前空間をターゲットとして指定して CLI コンテキストに設定します。

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

</br>

## API での名前空間の作成
{: #namespaces_create_api}

IAM 管理の名前空間をリソース・グループの一部として作成して、名前空間の作成時にリソース・グループをターゲットにすることによってリソースのアクセス・ポリシーを管理できます。 お客様の名前空間に対するアクセス権限を必要とする他のユーザーがいる場合や、お客様が自分の名前空間のアクションから他のリソースにアクセスする場合には、名前空間の作成後に IAM ポリシーを設定してください。
{: shortdesc}


1. 名前空間を作成するリソース・グループをターゲットに指定します。 [リソース・グループ](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)をまだ作成していない場合は、`default` グループをターゲットに指定できます。

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. IAM 対応の名前空間を作成します。

  ```
  curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> このコマンドの構成要素について</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) トークン。IAM トークンを取得するには、<code>ibmcloud iam oauth-tokens</code> を実行します。</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>名前空間の名前。</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>名前空間を作成するリソース・グループの ID。 リソース・グループの ID を確認するには、<code>ibmcloud resource groups</code> を実行します。</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>functions-base-plan などのリソース・プランの ID</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>オプション: 名前空間に説明 (例えば、それに含まれることになるアクションやパッケージの種類など) を追加します。</td>
      </tr>
    </tbody>
  </table>

  出力例:

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. 新しい名前空間が作成されたことを確認します。

  ```
  curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
  ```
  {: pre}

  IAM ベースの名前空間や Cloud Foundry ベースの名前空間など、すべての名前空間をリストすることもできます。
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  出力例:
  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}


HTTP REST の使用方法について詳しくは、[{{site.data.keyword.openwhisk_short}} API の資料](/apidocs/functions)を参照してください。
{: tip}



## 次のステップ
{: #namespaces_next}

これで名前空間が作成されたので、それを保護するために役立つ IAM アクセス・ポリシーを作成できます。 開始するには、[アクセス権限の管理](/docs/openwhisk?topic=cloud-functions-iam)を参照してください。 IAM ベースの名前空間を管理する方法について詳しくは、[{{site.data.keyword.openwhisk_short}} REST API リファレンス](/apidocs/functions)を参照してください。



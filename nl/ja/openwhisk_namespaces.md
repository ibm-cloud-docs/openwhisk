---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: namespaces, actions, create

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 名前空間の作成
{: #openwhisk_namespaces}

東京地域では、{{site.data.keyword.openwhisk_short}} が Identity and Access (IAM) 管理の名前空間を使用して、アクションやトリガーなどのエンティティーをグループにまとめます。その後、名前空間のアクセス・ポリシーを作成できます。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 名前空間を作成すると、それは IAM サービス・インスタンスとして識別されます。IAM 管理サービス・インスタンスは、[リソース・グループ](/docs/resources?topic=resources-rgs)内に作成する必要があります。
独自のリソース・グループを作成するか、デフォルトをターゲットにすることができます。アカウント内の IAM サービス・インスタンスを参照するには、`ibmcloud resource service-instances` を実行できます。

以下の成果物が名前空間と共に作成されます。これらは、削除しないでください。

* アウトバウンド・コールを行うときに機能 ID として使用できるサービス ID が作成されます。この名前空間内に作成されたすべてのアクションは、このサービス ID を使用して他のリソースにアクセスできます。すべてのサービス ID を参照するには、`ibmcloud iam service-ids` を実行します。

* 上記のサービス ID に対して、IAM トークンを生成するために使用できる API キーが作成されます。その後、そのトークンを使用して、名前空間を他の IBM Cloud サービスで認証できます。API キーは、環境変数としてアクションに対して提供されます。


## 制限
{: #limitations}

現在のところ、[API ゲートウェイを使用した API の作成](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)および[モバイル SDK](/docs/openwhisk?topic=cloud-functions-openwhisk_mobile_sdk) の使用は、IAM 管理の名前空間ではサポートされていません。

</br>

東京ロケーションで {{site.data.keyword.openwhisk_short}} バックエンド・サービスをターゲットにするには、`ibmcloud fn namespace list --apihost jp-tok.functions.cloud.ibm.com` のように、`apihost` をすべての CLI 呼び出しに付加する必要があります。これは、ロケーションを `ibmcloud target -r jp-tok` でターゲットにできるようになるまでの、一時的な方法です。
{: tip}



</br>
</br>


## CLI での名前空間の作成
{: #create_iam_cli}

IAM 管理の名前空間をリソース・グループの一部として作成して、名前空間の作成時にリソース・グループをターゲットにすることによってリソースのアクセス・ポリシーを管理できます。お客様の名前空間に対するアクセス権限を必要とする他のユーザーがいる場合や、お客様が自分の名前空間のアクションから他のリソースにアクセスする場合には、名前空間の作成後に IAM ポリシーを設定してください。
{: shortdesc}

1. 名前空間を作成するリソース・グループをターゲットに指定します。[リソース・グループ](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)をまだ作成していない場合は、`default` グループをターゲットに指定できます。

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
          <td>新しい IAM ベースの名前空間の表示名。</td>
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
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
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
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. 新しい名前空間にエンティティーを作成する前に、その名前空間をターゲットに指定することにより、そこに CLI コンテキストを設定します。
    ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

</br>

## API での名前空間の作成
{: #create_iam_api}

IAM 管理の名前空間をリソース・グループの一部として作成して、名前空間の作成時にリソース・グループをターゲットにすることによってリソースのアクセス・ポリシーを管理できます。お客様の名前空間に対するアクセス権限を必要とする他のユーザーがいる場合や、お客様が自分の名前空間のアクションから他のリソースにアクセスする場合には、名前空間の作成後に IAM ポリシーを設定してください。
{: shortdesc}



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
          <td>IBM Cloud Identity and Access Management (IAM) トークン。IAM トークンを取得するには、<code>ibmcloud iam oauth-tokens</code> を実行します。</td>
        </tr>
        <tr>
          <td><code>-n &lt;name&gt;</code></td>
          <td>名前空間の名前。</td>
        </tr>
        <tr>
          <td><code>-n &lt;resource_group_id&gt;</code></td>
          <td>名前空間を作成するリソース・グループの ID。リソース・グループの ID を確認するには、<code>ibmcloud resource groups</code> を実行します。</td>
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

    IAM ベースの名前空間や Cloud Foundry  ベースの名前空間など、すべての名前空間をリストすることもできます。
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


HTTP REST の作業について詳しくは、[Cloud Functions API の資料](https://cloud.ibm.com/apidocs/functions)を参照してください。
{: tip}

</br>
</br>


## 次のステップ
{: #next}

これで名前空間が作成されたので、それを保護するために役立つ IAM アクセス・ポリシーを作成できます。開始するには、[アクセス権限の管理](/docs/openwhisk?topic=cloud-functions-iam#iam)を参照してください。IAM ベースの名前空間を管理する方法について詳しくは、[{{site.data.keyword.openwhisk_short}} REST API リファレンス](https://cloud.ibm.com/apidocs/functions)を参照してください。

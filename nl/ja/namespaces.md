---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# 名前空間の管理
{: #namespaces}

{{site.data.keyword.openwhisk}} では、Identity and Access (IAM) 管理の名前空間を作成して、アクションやトリガーなどのエンティティーをグループにまとめることができます。 そして、それらの名前空間に対する IAM アクセス・ポリシーを作成できます。 IAM の概要については、[{{site.data.keyword.openwhisk_short}} の IAM 対応に関する発表ブログ](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}を参照してください。
{: shortdesc}

## 名前空間とは何ですか?

名前空間は、アクションやトリガーなどの {{site.data.keyword.openwhisk_short}} エンティティーを入れるものであり、リソース・グループに属しています。 名前空間に対するアクセス権限をユーザーに付与すると、そのユーザーがエンティティーにアクセスできるようになります。

エンティティーの完全修飾名は、
`/namespaceName/packageName/entityName` です。

### 名前空間を作成するとどうなりますか?

{{site.data.keyword.openwhisk_short}} 内に作成された名前空間は、IAM サービス・インスタンスとして識別されます。
名前空間の作成時に、このサービス・インスタンスを追加する[リソース・グループ](/docs/resources?topic=resources-rgs)を指定できます。

名前空間を作成すると、以下のコンポーネントが作成されます。

| コンポーネント | 説明 |
| --- | --- | 
| サービス ID | アウトバウンド・コールを行うときに、サービス ID を機能 ID として使用できます。この名前空間に作成するすべてのアクションは、このサービス ID を使用して他のリソースにアクセスできます。 機能ユーザーは、デフォルトでリーダー役割を取得します。リーダー権限は、名前空間のエンティティーの読み取りとアクションの呼び出しを行えることを意味します。トリガーがアクションを呼び出すときにはリーダー役割が使用されます。 インバウンド・トラフィックを制御するために、アクションを呼び出すリーダー役割などのアクセス権限を他のユーザーに割り当てることができます。 |
| API キー | サービス ID の API キー。これを使用して IAM トークンを生成できます。 そのトークンを使用して、名前空間を他の {{site.data.keyword.cloud_notm}} サービスで認証できます。 API キーは、環境変数 `__OW_IAM_NAMESPACE_API_KEY` としてアクションに対して提供されます。 |

サービス ID をすべて表示します。
```
ibmcloud iam service-ids
```
{: pre}

サービス ID に関連付けられた API キーを表示します。 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

API キーは、削除しないでください。
{: tip}

### 名前空間に関する制限はありますか?

[API ゲートウェイを使用した API の作成](/docs/openwhisk?topic=cloud-functions-apigateway)と[モバイル SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) は、IAM 管理の名前空間ではサポートされていません。 

アクション、トリガー、ルール、パッケージ、および名前空間を含めて、すべてのエンティティーの名前は、次の形式の文字列です。
* 先頭文字は、英数字または下線でなければなりません。
* 後続の文字には、英数字、スペース、または `_`、`@`、`.`、`-` のどの値でも使用できます。
* 最後の文字をスペースにすることはできません。

### Cloud Foundry ベースの名前空間がある場合はどうすればよいですか?

Cloud Foundry ベースの名前空間は引き続き機能します。ただし、新機能を利用するには、IAM 対応の名前空間を作成する必要があります。


## UI での IAM ベースの名前空間の作成
{: #create_iam_ui}

1. [{{site.data.keyword.openwhisk_short}} コンソール](https://cloud.ibm.com/openwhisk){: external}で、名前空間ドロップダウン・メニューをクリックします。

2. **「名前空間の作成」**をクリックします。

3. 名前空間の表示名と簡略説明 (この名前空間に作成するアクションやパッケージの種類など) を入力します。

4. 名前空間を作成するリソース・グループと、名前空間リソースをデプロイするロケーションを選択します。

5. **「作成」**をクリックします。

6. 名前空間リソースのサービス・インスタンスを表示するには、[{{site.data.keyword.cloud_notm}} ダッシュボード](https://cloud.ibm.com/resources){: external}に移動し、**「Functions 名前空間」**ペインで名前空間名を見つけます。

必要に応じて、{{site.data.keyword.openwhisk_short}} コンソールの**「名前空間設定 (Namespace settings)」**ページで、名前空間の名前または説明を更新できます。

## CLI での IAM ベースの名前空間の作成
{: #namespaces_create}

IAM 管理の名前空間をリソース・グループの一部として作成して、名前空間の作成時にリソース・グループをターゲットにすることによってリソースのアクセス・ポリシーを管理できます。 お客様の名前空間に対するアクセス権限を必要とする他のユーザーがいる場合や、お客様が自分の名前空間のアクションから他のリソースにアクセスする場合には、名前空間の作成後に IAM ポリシーを設定してください。
{: shortdesc}

1. 名前空間を作成するリソース・グループをターゲットに指定します。 [リソース・グループ](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)をまだ作成していない場合は、`default` グループをターゲットに指定できます。

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. IAM 対応の名前空間を作成します。 オプション: `-n` または `--description` フラグを使用して、名前空間の説明を含めます。説明が 1 語より長い場合は、引用符で囲む必要があります。

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
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
        <td>オプション: 名前空間に説明 (作成するアクションやパッケージの種類など) を追加します。説明が 1 語より長い場合は、引用符で囲む必要があります。</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>オプション: 名前空間に説明 (作成するアクションやパッケージの種類など) を追加します。説明が 1 語より長い場合は、引用符で囲む必要があります。</td>
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
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  IAM ベースの名前空間や Cloud Foundry ベースの名前空間など、すべての名前空間をリストすることもできます。

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 名前空間にエンティティーを作成するには、その名前空間をターゲットに指定することにより、CLI コンテキストを設定しておく必要があります。

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

`--namespace` プロパティーなどのプロパティーを設定した後は、手動で設定解除するまでそのプロパティーは保持されます。IAM 名前空間を切り替える場合、または Cloud Foundry と IAM を切り替える場合は、名前空間プロパティーを設定解除して再設定する必要があります。詳しくは、[`ibmcloud fn property set`] を参照してください。
{: note}

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
        <td>{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) トークン。 IAM トークンを取得するには、<code>ibmcloud iam oauth-tokens</code> を実行します。</td>
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

  **出力例**

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

  **出力例**

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

### 名前空間から他のリソースへのアクセス
{: #namespace-access}

通常、アクションが呼び出す他の {{site.data.keyword.cloud_notm}} リソースとサービスには、適切な認証が必要になります。 それらのサービスが IAM 対応で IAM トークンを受け入れる場合は、名前空間の機能 ID をアウトバウンド通信に利用できます。
{: shortdesc}

[IAM アクセス権限の管理](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)で説明しているように、名前空間ごとに、名前空間を表すサービス ID が作成されます。 IAM ポリシー管理を使用して適切な役割を割り当てることで、このサービス ID に他のサービスおよびリソースに対するアクセス権限を付与できます。 サービス ID を作成して他の IAM 対応サービスにアクセスする方法について詳しくは、[サービス ID の作成と処理](/docs/iam?topic=iam-serviceids#serviceids)を参照してください。

実行時に、{{site.data.keyword.openwhisk_short}} は名前空間のサービス ID の API キーを、環境変数 `__OW_IAM_NAMESPACE_API_KEY` としてアクション・コードに渡します。 アクション・コードでは、この API キーを使用して IAM トークンを生成します。 サポートされる大多数の {{site.data.keyword.openwhisk_short}} SDK (Cloudant、{{site.data.keyword.watson}}、{{site.data.keyword.cos_full_notm}} など) は、IAM API キーそのものを認証に使用します。 REST API を使用するその他の IAM 管理のサービスまたはリソースは、IAM API キーから生成されたトークンで認証を行えます。 詳しくは、[ユーザーまたはサービス ID の IAM アクセス・トークンの作成](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)を参照してください。

API キーとトークンがどのように組み合わされるかについて詳しくは、[IAM の資料](/docs/iam?topic=iam-iamapikeysforservices)を参照してください。

## 次のステップ
{: #namespaces_next}

これで名前空間が作成されたので、それを保護するために役立つ IAM アクセス・ポリシーを作成できます。開始するには、[アクセス権限の管理](/docs/openwhisk?topic=cloud-functions-iam)を参照してください。 

IAM ベースの名前空間を管理する方法について詳しくは、[{{site.data.keyword.openwhisk_short}} REST API リファレンス](/apidocs/functions)を参照してください。









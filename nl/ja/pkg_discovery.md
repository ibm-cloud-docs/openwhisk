---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: functions, cognitive,

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


# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

インストール可能な {{site.data.keyword.discoveryfull}} サービスは、コグニティブ検索・コンテンツ分析エンジンです。このエンジンをアプリケーションに追加すれば、パターン、傾向、アクション可能な洞察を見出して有効な意思決定を促進できます。 事前強化されたコンテンツで構造化データおよび非構造化データを安全に統合し、簡略化された照会言語を使用して、結果のフィルター処理を手動で行わずに済むようにします。
{: shortdesc}

{{site.data.keyword.discoveryshort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.discoveryshort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html){: external} | パッケージ | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | {{site.data.keyword.discoveryshort}} サービスを操作します。 |
| [`create-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`description`、`size` | 環境を作成します。 |
| [`delete-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id` | 環境を削除します。 |
| [`get-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`  | 環境に関する情報を取得します。 |
| [`list-environments`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`  | 環境をリストします。 |
| [`list-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_ids` | コレクション全体のフィールドをリストします。 |
| [`update-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name`、`description` | 環境を更新します。 |
| [`create-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name`、`description`、`conversions`、`enrichments`、`normalizations`  | 構成を追加します。 |
| [`delete-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration_id` | 構成を削除します。 |
| [`get-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration_id` | 構成の詳細を取得します。 |
| [`list-configurations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name` | 構成をリストします。 |
| [`update-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration){: external} | アクション |  `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration_id`、`name`、`description`、`conversions`、`enrichments`、`normalizations` | 構成を更新します。 |
| [`test-configuration-in-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration`、    `step`、`configuration_id`、`file`、`metadata`、`file_content_type` | 構成をテストします。 |
| [`create-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection){: external} | アクション |  `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name`、`description`、`configuration_id`、`language` | コレクションを作成します。 |
| [`delete-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | コレクションを削除します。 |
| [`get-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | コレクションの詳細を取得します。 |
| [`list-collection-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | コレクション・フィールドをリストします。 |
| [`list-collections`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name` | コレクションをリストします。 |
| [`update-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`name`、`description`、`configuration_id` | コレクションを更新します。 |
| [`create-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、``collection_id`、`expansions` | 拡張リストを作成または更新します。 |
| [`delete-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 拡張リストを削除します。 |
| [`list-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 拡張リストを取得します。 |
| [`add-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`file`、`metadata`、`file_content_type` | 文書を追加します。 |
| [`delete-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`document_id` | 文書を削除します。 |
| [`get-document-status`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`document_id` | 文書の詳細を取得します。 |
| [`update-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`document_id`、`file`、`metadata`、`file_content_type` | 文書を更新します。 |
| [`federated-query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_ids`、`filter`、`query`、`natural_language_query`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`deduplicate`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields` | 複数のコレクション内の文書を照会します。 |
| [`federated-query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_ids`、`filter`、`query`、`natural_language_query`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields` | 複数のコレクション・システム通知を照会します。 |
| [`query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`filter`、`query`、`natural_language_query`、`passages`、    `aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`passages_fields`、`passages_count`、`passages_characters`、`deduplicate`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields` | コレクションを照会します。 |
| [`query-entities`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`feature`、`entity`、`context`、`count`、`evidence_count` | ナレッジ・グラフ・エンティティーを照会します。 |
| [`query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`filter`、`query`、`natural_language_query`、`passages`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`passages_fields`、`passages_count`、`passages_characters`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields`  | システム通知を照会します。 |
| [`query-relations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`entities`、`context`、`sort`、`filter`、`count`、`evidence_count` | ナレッジ・グラフ関係を照会します。 |
| [`add-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data){: external} | アクション |  `username`、`password`、 `iam_access_token`、`iam_apikey、iam_url`、 `headers、headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`natural_language_query`、`filter`、`examples`、 | 照会をトレーニング・データに追加します。 |
| [`create-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`document_id`、`cross_reference`、`relevance` | 例をトレーニング・データ照会に追加します。 |
| [`delete-all-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | すべてのトレーニング・データを削除します。 |
| [`delete-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id` | トレーニング・データ照会を削除します。 |
| [`delete-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`example_id` | トレーニング・データ照会の例を削除します。 |
| [`get-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id` | 照会に関する詳細を取得します。 |
| [`get-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`example_id` | トレーニング・データの例に関する詳細を取得します。 |
| [`list-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | トレーニング・データをリストします。 |
| [`list-training-examples`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples){: external} | アクション | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id` | トレーニング・データ照会の例をリストします。 |
| [`update-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`example_id`、`cross_reference`、`relevance` | 例のラベルまたは相互参照を変更します。 |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id` | ラベル付きデータを削除します。 |

## {{site.data.keyword.discoveryshort}} サービス・インスタンスの作成
{: #service_instance_discovery}

パッケージをインストールする前に、{{site.data.keyword.discoveryshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。

1. [{{site.data.keyword.discoveryshort}} サービス・インスタンスを作成します](https://cloud.ibm.com/catalog/services/discovery){: external}。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.discoveryshort}} パッケージのインストール
{: #install_discovery}

{{site.data.keyword.discoveryshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.discoveryshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #discovery_cli}

**始める前に**
[{{site.data.keyword.cloud_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.discoveryshort}} パッケージをインストールするため、以下のコマンドを実行します。

1. {{site.data.keyword.discoveryshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
    ```
    {: pre}

3. パッケージがパッケージ・リストに追加されたことを確認します。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **出力**
    ```
    packages
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.discoveryshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    **出力例**
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.discoveryshort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    **出力例**
    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "discovery": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #discovery_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external} に移動します。

2. 名前空間ドロップダウン・メニューを使用して、パッケージをインストールする名前空間を選択します。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Discovery」**パッケージをクリックします。

6. **「インストール (Install)」**をクリックします。

7. パッケージがインストールされると、アクション・ページにリダイレクトされ、**discovery-v1** という名前の新規パッケージを検索できます。

8. `discovery-v1` パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、CLI の手順のステップ 4 を実行します。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。

  使用するアクションごとに以下のステップを実行する必要があります。
  {: note}

    1. 使用する `discovery-v1` パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.discoveryshort}} パッケージの使用
{: #usage_discovery}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

すべてのアクションに、YYYY-MM-DD 形式の version パラメーターが必要です。 API が後方非互換の方法で変更された場合、新しいバージョン日付がリリースされます。 [API 参照](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning){: external}で詳細を確認してください。

このパッケージの機能は、Discovery の現行バージョンである `2018-03-05` を使用します。 `list-environments` アクションを試行します。
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}




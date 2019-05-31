---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: natural language, understanding, watson knowledge studio

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

# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

インストール可能な {{site.data.keyword.nlufull}} サービスは、大規模なテキスト・コンテンツのさまざまな特徴を分析するために役立ちます。
{: shortdesc}

テキスト、生の HTML、またはパブリック URL を指定すると、要求した特徴の結果が {{site.data.keyword.nlushort}} によって表示されます。 このサービスは、デフォルトで分析前に HTML コンテンツをクリーニングするため、結果では、ほとんどの広告やその他の不要なコンテンツを無視することができます。 Watson Knowledge Studio を使用して、カスタムのエンティティーと関係を Natural Language Understanding で検出するために使用できる<a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">カスタム・モデル</a>を作成できます。

{{site.data.keyword.nlushort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.nlushort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html) | パッケージ | `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、  |  {{site.data.keyword.nlushort}} サービスを操作します。 |
| [analyze](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze) | アクション |  `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`features`、`text`、`html`、`url`、`clean`、`xpath`、`fallback_to_raw`、`return_analyzed_text`、`language`、`limit_text_characters`、  | テキスト、HTML、または公開 Web ページを分析します。 |
| [delete-model](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model) | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`model_id`、  | モデルを削除します。 |
| [list-models](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models) | アクション |  `username`、`password`、 `iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、 | モデルをリストします。 |

## {{site.data.keyword.nlushort}} サービス・インスタンスの作成
{: #service_instance_understanding}

パッケージをインストールする前に、{{site.data.keyword.nlushort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.nlushort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します](https://cloud.ibm.com/catalog/services/natural-language-understanding)。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.nlushort}} パッケージのインストール
{: #install_understanding}

{{site.data.keyword.nlushort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.nlushort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #nlus_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.nlushort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.nlushort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
    ```
    {: pre}

3. パッケージがパッケージ・リストに追加されたことを確認します。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    出力:
    ```
    packages
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.nlushort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.nlushort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #nlus_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、パッケージのインストール先の名前空間を選択します。 名前空間は、組織名とスペース名の組み合わせから形成されます。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Natural Language Understanding」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、**natural-language-understanding-v1** という名前の新規パッケージを検索できます。

7. **natural-language-understanding-v1** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **natural-language-understanding-v1** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.nlushort}} パッケージの使用
{: #usage_understanding}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

すべてのアクションに、YYYY-MM-DD 形式の version パラメーターが必要です。 API が後方非互換の方法で変更された場合、新しいバージョン日付がリリースされます。 [API 参照](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning)で詳細を確認してください。

このパッケージの機能は、Natural Language Understanding の現行バージョンである 2018-03-16 を使用します。 `list-models` アクションを試行します。
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}


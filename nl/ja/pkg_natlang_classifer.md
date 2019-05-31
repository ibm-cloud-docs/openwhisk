---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: nlc, natural language classifier, machine learning

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

# {{site.data.keyword.nlclassifiershort}}
{: #pkg_natlang_classifier}

{{site.data.keyword.nlclassifierfull}} は機械学習アルゴリズムを使用して、ショート・テキスト入力に対して最も一致度の高い定義済みクラスを返します。 サービスが定義済みクラスを新規の入力に適用できるように、種別を作成し、これらのクラスをサンプル・テキストに結合するようにトレーニングします。
{: shortdesc}

{{site.data.keyword.nlclassifiershort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.nlclassifiershort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html) | パッケージ | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url  | {{site.data.keyword.nlclassifiershort}} サービスを操作します。 |
| [classify](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id、text  | 句を分類します。 |
| [classify-collection](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id、collection  | 複数の句を分類します。 |
| [create-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、metadata、training_data  | 種別を作成します。 |
| [delete-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id  | 種別を削除します。 |
| [get-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id  | 種別に関する情報を取得します。 |
| [list-classifiers](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | 種別をリストします。 |

## {{site.data.keyword.nlclassifiershort}} サービス・インスタンスの作成
{: #service_instance_classifier}

パッケージをインストールする前に、{{site.data.keyword.nlclassifiershort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.nlclassifiershort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します](https://cloud.ibm.com/catalog/services/natural_language_classifier)。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.nlclassifiershort}} パッケージのインストール
{: #install_classifier}

{{site.data.keyword.nlclassifiershort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.nlclassifiershort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #nlclassifier_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.nlclassifiershort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.nlclassifiershort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.nlclassifiershort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。 上記のコマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.nlclassifiershort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #nlclassifier_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、パッケージのインストール先の名前空間を選択します。 

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Natural Language Classifier」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、**natural-language-classifier-v1** という名前の新規パッケージを検索できます。

7. **natural-language-classifier-v1** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **natural-language-classifier-v1** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.nlclassifiershort}} パッケージの使用
{: #usage_classifier}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

`list-classifiers` アクションを試行します。
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}


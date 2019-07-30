---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: functions, serverless, watson

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


# {{site.data.keyword.toneanalyzershort}}
{: #pkg_tone_analyzer}

インストール可能な {{site.data.keyword.toneanalyzerfull}} サービスは、言語分析を使用して、文章から感情トーンと言語トーンを検出します。
{:shortdesc}

このサービスは、文書レベルと文レベルの両方でトーンを分析できます。 このサービスを使用して、書面によるコミュニケーションがどのように認識されるかを理解して、コミュニケーションのトーンを向上させることができます。 企業はこのサービスを使用して顧客のコミュニケーションのトーンを学習して、各顧客に対応したり、顧客の会話を理解して改善したりすることができます。

Tone Analyzer サービスの要求ロギングは使用不可になっています。 このサービスは、`X-Watson-Learning-Opt-Out` 要求ヘッダーが設定されているかどうかに関係なく、要求と応答に含まれるデータをログに記録することも、保存することもしません。
{: note}

{{site.data.keyword.toneanalyzershort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.toneanalyzershort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html){: external} | パッケージ | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url` | {{site.data.keyword.toneanalyzershort}} サービスを操作します。 |
| [`tone`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`,`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、   `tone_input`、`content_type`、`sentences`、`tones`、`content_language`、`accept_language` | 一般的なトーンを分析します。 |
| [`tone-chat`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat){: external} | アクション |  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`utterances`、`content_language`、`accept_language` | 顧客エンゲージメント・トーンを分析します。 |


## {{site.data.keyword.toneanalyzershort}} サービス・インスタンスの作成
{: #service_instance_tone}

パッケージをインストールする前に、{{site.data.keyword.toneanalyzershort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.toneanalyzershort}} サービス・インスタンスを作成します](https://cloud.ibm.com/catalog/services/tone_analyzer){: external}。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.toneanalyzershort}} パッケージのインストール
{: #install_tone}

{{site.data.keyword.toneanalyzershort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.toneanalyzershort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #toneanalyzer_cli}

**始める前に**
[{{site.data.keyword.cloud_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.toneanalyzershort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.toneanalyzershort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.toneanalyzershort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。 コマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}

    **出力例**
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.toneanalyzershort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    **出力例**
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #toneanalyzer_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external} に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、パッケージのインストール先の名前空間を選択します。 

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Tone Analyzer」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、**`tone-analyzer-v3`** という名前の新規パッケージを検索できます。

7. **tone-analyzer-v3** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、[CLI の手順](#toneanalyzer_cli)のステップ 4 と 5 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 
  
  使用するアクションごとに以下のステップを実行する必要があります。
  {: note}
  
    1. 使用する **tone-analyzer-v3** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.toneanalyzershort}} パッケージの使用
{: #usage_tone}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

すべてのアクションに、YYYY-MM-DD 形式の version パラメーターが必要です。 API が後方非互換の方法で変更された場合、新しいバージョン日付がリリースされます。 [API 参照](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning){: external}で詳細を確認してください。

このパッケージの機能は、Tone Analyzer の現行バージョンである 2017-09-21 を使用します。 `tone` アクションを試行します。
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}


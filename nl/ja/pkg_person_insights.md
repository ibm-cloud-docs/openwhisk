---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: personality insights, cognitive, serverless, functions

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

# {{site.data.keyword.personalityinsightsshort}}
{: #pkg_person_insights}

インストール可能な {{site.data.keyword.personalityinsightsfull}} サービスを使用すると、アプリケーションは、ソーシャル・メディア、エンタープライズ・データ、その他のデジタル通信から洞察を導き出すことができます。 このサービスは、言語分析を使用して、E メール、テキスト・メッセージ、ツイート、フォーラム投稿などのデジタル通信から、ビッグ・ファイブ、ニーズ、価値を含む個人の本質的なパーソナリティー特性を推測します。
{: shortdesc}

このサービスは、混乱を招くようなソーシャル・メディアから、パーソナリティー特性を反映する個人の人物像を自動的に推測できます。 このサービスでは、分析の結果に基づいて消費嗜好性を推測することができ、タイム・スタンプが付いた JSON コンテンツでは、時間的な行動を報告できます。
* サービスがパーソナリティー特性を説明するために使用するモデルの意味については、[パーソナリティー・モデル](/docs/services/personality-insights?topic=personality-insights-models)を参照してください。
* 消費嗜好性の意味については、[消費嗜好性](/docs/services/personality-insights?topic=personality-insights-preferences)を参照してください。

**注:** {{site.data.keyword.personalityinsightsshort}} サービスの要求ロギングは使用不可になっています。 このサービスは、`X-Watson-Learning-Opt-Out` 要求ヘッダーが設定されているかどうかに関係なく、要求と応答からのデータをログに記録することも、保存することもありません。

{{site.data.keyword.personalityinsightsshort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.personalityinsightsshort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | パッケージ | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url  | {{site.data.keyword.personalityinsightsshort}} V3 サービスを操作します。 |
| [`profile`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、content、content_type、content_language、accept_language、raw_scores、csv_headers、consumption_preferences  | プロファイルを取得します。 |
| [`profile-as-csv`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、content、content_type、content_language、accept_language、raw_scores、csv_headers、consumption_preferences  | プロファイルを CSV ファイルとして取得します。 |

## {{site.data.keyword.personalityinsightsshort}} サービス・インスタンスの作成
{: #service_instance_insights}

パッケージをインストールする前に、{{site.data.keyword.personalityinsightsshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.personalityinsightsshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します](https://cloud.ibm.com/catalog/services/personality_insights)。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.personalityinsightsshort}} パッケージのインストール
{: #install_insights}

{{site.data.keyword.personalityinsightsshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.personalityinsightsshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #personalityinsights_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.personalityinsightsshort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.personalityinsightsshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.personalityinsightsshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。 上記のコマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.personalityinsightsshort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #personalityinsights_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、パッケージのインストール先の名前空間を選択します。 

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Personality Insights」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、**personality-insights-v3** という名前の新規パッケージを検索できます。

7. **personality-insights-v3** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **personality-insights-v3** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.personalityinsightsshort}} パッケージの使用
{: #usage_insights}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

すべてのアクションに、YYYY-MM-DD 形式の version パラメーターが必要です。 API が後方非互換の方法で変更された場合、新しいバージョン日付がリリースされます。 [API 参照](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning)で詳細を確認してください。

このパッケージの機能は、Personality Insights の現行バージョンである 2017-10-13 を使用します。 `profile` アクションを試行します。
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "自分自身に関する抜粋をここに記述できますが、少なくとも 100 語の長さにする必要があります。 この抜粋は単なる充てんテキストであり、Personality Insights サービスからは重要な内容は返されません。 このサービスは、言語分析を使用して、E メール、テキスト・メッセージ、ツイート、フォーラム投稿などのデジタル通信から、ビッグ・ファイブ、ニーズ、価値を含む個人の本質的なパーソナリティー特性を推測します。 このサービスは、混乱を招くようなソーシャル・メディアから、パーソナリティー特性を反映する個人の人物像を自動的に推測できます。 このサービスでは、分析の結果に基づいて消費嗜好性を推測することができ、タイム・スタンプが付いた JSON コンテンツでは、時間的な行動を報告できます。
```
{: pre}


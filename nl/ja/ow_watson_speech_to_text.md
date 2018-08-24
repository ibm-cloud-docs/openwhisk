---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.speechtotextshort}} パッケージ 

{{site.data.keyword.speechtotextfull}} サービスは、IBM の音声認識機能を使用して会話音声の書き起こしを生成する API を提供します。
{:shortdesc}

このサービスは、さまざまな言語と音声形式の音声を書き起こすことができます。基本的な書き起こしに加えて、音声の多数の側面に関する詳細情報を生成できます。このサービスは、ほとんどの言語について広帯域と狭帯域の 2 つのサンプリング・レートをサポートします。これは、すべての JSON 応答コンテンツを UTF-8 文字セットで返します。このサービスについて詳しくは、[IBM&reg; Cloud の資料](https://console.bluemix.net/docs/services/speech-to-text/index.html)を参照してください。

{{site.data.keyword.speechtotextshort}} パッケージには、以下のエンティティーが含まれています。エンティティー名をクリックすると、{{site.data.keyword.speechtotextshort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) | パッケージ | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | {{site.data.keyword.speechtotextshort}} V1 サービスを操作します。|
| [get-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、model_id  | モデルを取得します。 |
| [list-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models) | アクション | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | モデルをリストします。 |
| [recognize-sessionless](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、audio、content_type、model、customization_id、acoustic_customization_id、base_model_version、customization_weight、inactivity_timeout、keywords、keywords_threshold、max_alternatives、word_alternatives_threshold、word_confidence、timestamps、profanity_filter、smart_formatting、speaker_labels  | 音声 (セッションなし) を認識します。 |
| [check-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、id  | ジョブを検査します。 |
| [check-jobs](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs) | アクション | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | ジョブを検査します。 |
| [create-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、audio、content_type、model、callback_url、events、user_token、results_ttl、customization_id、acoustic_customization_id、base_model_version、customization_weight、inactivity_timeout、keywords、keywords_threshold、max_alternatives、word_alternatives_threshold、word_confidence、timestamps、profanity_filter、smart_formatting、speaker_labels  | ジョブを作成します。 |
| [delete-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、id  | ジョブを削除します。 |
| [register-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、callback_url、user_secret  | コールバックを登録します。 |
| [unregister-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、callback_url  | コールバックを登録抹消します。 |
| [create-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、base_model_name、dialect、description  | カスタム言語モデルを作成します。 |
| [delete-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタム言語モデルを削除します。 |
| [get-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタム言語モデルを取得します。 |
| [list-language-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、language  | カスタム言語モデルをリストします。 |
| [reset-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタム言語モデルをリセットします。 |
| [train-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word_type_to_add、customization_weight  | カスタム言語モデルをトレーニングします。 |
| [upgrade-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタム言語モデルをアップグレードします。 |
| [add-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、corpus_name、corpus_file、allow_overwrite  | コーパスを追加します。 |
| [delete-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、corpus_name  | コーパスを削除します。 |
| [get-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、corpus_name  | コーパスを取得します。 |
| [list-corpora](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | コーパスをリストします。 |
| [add-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word_name、word、sounds_like、display_as  | カスタムの単語を追加します。 |
| [add-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、words  | カスタムの単語を追加します。 |
| [delete-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word_name  | カスタムの単語を削除します。 |
| [get-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word_name  | カスタムの単語を取得します。 |
| [list-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word_type、sort  | カスタムの単語をリストします。 |
| [create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、base_model_name、description  | カスタムの音響モデルを作成します。 |
| [delete-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタムの音響モデルを削除します。 |
| [get-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタムの音響モデルを取得します。 |
| [list-acoustic-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、language  | カスタムの音響モデルをリストします。 |
| [reset-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタムの音響モデルをリセットします。 |
| [train-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、custom_language_model_id  | カスタムの音響モデルをトレーニングします。 |
| [upgrade-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、custom_language_model_id  | カスタムの音響モデルをアップグレードします。 |
| [add-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、audio_name、audio_resource、content_type、contained_content_type、allow_overwrite  | 音声リソースを追加します。 |
| [delete-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、audio_name  | 音声リソースを削除します。 |
| [get-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、audio_name  | 音声リソースを取得します。 |
| [list-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | 音声リソースをリストします。 |
| [delete-user-data](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id  | ラベル付きデータを削除します。|

## {{site.data.keyword.speechtotextshort}} サービス・インスタンスの作成
{: #service_instance}

パッケージをインストールする前に、{{site.data.keyword.speechtotextshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.speechtotextshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/speech_to_text) します。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.speechtotextshort}} パッケージのインストール
{: #install}

{{site.data.keyword.speechtotextshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.speechtotextshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #speechtotext_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](bluemix_cli.html#cloudfunctions_cli)。
  2. [`wskdeploy` コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) をインストールして、ダウンロードしたバイナリーを PATH に追加します。

{{site.data.keyword.speechtotextshort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.speechtotextshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    wskdeploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.speechtotextshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。上記のコマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.speechtotextshort}} サービス・インスタンス資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package speech-to-text-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #speechtotext_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、{{site.data.keyword.cos_short}} パッケージのインストール先の名前空間を選択します。名前空間は、組織名とスペース名の組み合わせから形成されます。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Speech To Text」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。 

6. パッケージのインストール後に「アクション」ページにリダイレクトされ、**speech-to-text-v1** という名前の新規パッケージを検索できます。

7. **speech-to-text-v1** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。 
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。**注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **speech-to-text-v1** パッケージのアクションをクリックします。そのアクションの詳細ページが開きます。 
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。 
    3. 新しい**パラメーター**を入力します。key には `__bx_creds` を入力します。value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.speechtotextshort}} パッケージの使用
{: #usage}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

`list-models` アクションを試行します。
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

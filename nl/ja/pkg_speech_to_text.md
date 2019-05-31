---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: speech to text, watson, package, cognitive,

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

# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## パッケージ

| 名前 | 可用性 | 説明 |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | インストール可能 | {{site.data.keyword.speechtotextshort}} V1 サービスを操作します。 |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | 事前インストール済み (東京では提供されていません) | Watson API を呼び出して音声をテキストに変換します |


## {{site.data.keyword.speechtotextshort}} サービス
{: #speech_to_text}

インストール可能な {{site.data.keyword.speechtotextfull}} サービスは、IBM の音声認識機能を使用して会話音声の書き起こしを生成する [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) を提供します。
{:shortdesc}

このサービスは、さまざまな言語と音声形式の音声を書き起こすことができます。 基本的な書き起こしに加えて、音声の多数の側面に関する詳細情報を生成できます。 このサービスは、ほとんどの言語について広帯域と狭帯域の 2 つのサンプリング・レートをサポートします。 これは、すべての JSON 応答コンテンツを UTF-8 文字セットで返します。 このサービスについて詳しくは、[IBM&reg; Cloud の資料](/docs/services/speech-to-text?topic=speech-to-text-about)を参照してください。

{{site.data.keyword.speechtotextshort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.speechtotextshort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) | パッケージ | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url  | {{site.data.keyword.speechtotextshort}} V1 サービスを操作します。 |
| [get-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、model_id  | モデルを取得します。 |
| [list-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | モデルをリストします。 |
| [recognize-sessionless](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、audio、content_type、model、customization_id、acoustic_customization_id、base_model_version、customization_weight、inactivity_timeout、keywords、keywords_threshold、max_alternatives、word_alternatives_threshold、word_confidence、timestamps、profanity_filter、smart_formatting、speaker_labels  | 音声 (セッションなし) を認識します。 |
| [check-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、id  | ジョブを検査します。 |
| [check-jobs](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | ジョブを検査します。 |
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
| [delete-user-data](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id  | ラベル付きデータを削除します。 |

### {{site.data.keyword.speechtotextshort}} サービス・インスタンスの作成
{: #service_instance_speechtotext}

パッケージをインストールする前に、{{site.data.keyword.speechtotextshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.speechtotextshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") します](https://cloud.ibm.com/catalog/services/speech_to_text)。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

### {{site.data.keyword.speechtotextshort}} パッケージのインストール
{: #install_speechtotext}

{{site.data.keyword.speechtotextshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.speechtotextshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #speechtotext_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.speechtotextshort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.speechtotextshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。 上記のコマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.speechtotextshort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
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

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、パッケージのインストール先の名前空間を選択します。 

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Speech To Text」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、**speech-to-text-v1** という名前の新規パッケージを検索できます。

7. **speech-to-text-v1** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。 **注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **speech-to-text-v1** パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しい**パラメーター**を入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

### {{site.data.keyword.speechtotextshort}} パッケージの使用
{: #usage_speechtotext}

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

## Watson: Speech to Text
{: #preinstall_speechtotext}

この事前インストール済みパッケージは、東京地域では利用できません。 IAM 認証を使用してインストール可能な [Speech to Text](#install_speechtotext) パッケージを参照してください。
{: tip}

`/whisk.system/watson-speechToText` パッケージを利用して、スピーチをテキストに変換するための Watson API を簡単に呼び出すことができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | パッケージ | username、password | スピーチをテキストに変換するパッケージ |
| `/whisk.system/watson-speechToText/speechToText` | アクション | payload、content_type、encoding、username、password、continuous、inactivity_timeout、interim_results、keywords、keywords_threshold、max_alternatives、model、timestamps、watson-token、word_alternatives_threshold、word_confidence、X-Watson-Learning-Opt-Out | 音声のテキストへの変換 |

**注**: パッケージ `/whisk.system/watson` は、アクション `/whisk.system/watson/speechToText` を含めて非推奨です。 代わりに [{{site.data.keyword.speechtotextshort}} パッケージ](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud)を参照してください。

### {{site.data.keyword.Bluemix_notm}} での Watson Speech to Text パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用している場合、パッケージ・バインディングは {{site.data.keyword.Bluemix_notm}} Watson サービス・インスタンス用に自動的に作成されます。

1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](http://cloud.ibm.com)で Watson Speech to Text のサービス・インスタンスを作成します。

  サービス・インスタンスの名前、および今使用している {{site.data.keyword.Bluemix_notm}} の組織とスペースの名前を忘れないようにしてください。

2. 名前空間でパッケージを最新表示します。 最新表示により、作成した Watson サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  出力例:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  パッケージをリストして、バインディングが作成されたことを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### {{site.data.keyword.Bluemix_notm}} 外部での Watson Speech to Text パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}} を使用していない場合、または {{site.data.keyword.Bluemix_notm}} の外部で Watson Speech to Text をセットアップしたい場合は、Watson Speech to Text サービスのパッケージ・バインディングを手動で作成する必要があります。 Watson Speech to Text サービスのユーザー名とパスワードが必要になります。

Watson Speech to Text サービス用に構成されるパッケージ・バインディングを作成します。
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### スピーチをテキストに変換

`/whisk.system/watson-speechToText/speechToText` アクションは、音声スピーチをテキストに変換します。 パラメーターは次のとおりです。

- `username`: Watson API ユーザー名。
- `password`: Watson API パスワード。
- `payload`: テキストに変換するエンコード・スピーチ・バイナリー・データ。
- `content_type`: 音声の MIME タイプ。
- `encoding`: スピーチ・バイナリー・データのエンコード。
- `continuous`: 長い休止で区切られた連続する句を示す複数の最終結果が返されるかどうかを示します。
- `inactivity_timeout`: 送信された音声に音が検出されない場合に、この時間 (秒数) が経過すると、接続がクローズされます。
- `interim_results`: サービスが暫定の結果を返すかどうかを示します。
- `keywords`: 音声で特定するキーワードのリスト。
- `keywords_threshold`: キーワードを特定するための下限である信頼値。
- `max_alternatives`: 返される代替トランスクリプトの最大数。
- `model`: 認識要求のために使用されるモデルの識別子。
- `timestamps`: 単語ごとにタイム・アライメントが返されるかどうかを示します。
- `watson-token`: サービス資格情報を指定する代わりとして、サービスに認証トークンを指定します。
- `word_alternatives_threshold`: 可能な単語の代替として仮説を識別するための下限である信頼値。
- `word_confidence`: 単語ごとに信頼度の指標を 0 から 1 の範囲で返すかどうかを示します。
- `X-Watson-Learning-Opt-Out`: 呼び出しのデータ収集をオプトアウトするかどうかを示します。

パッケージ・バインディング内の **speechToText** アクションを呼び出して、エンコードされた音声を変換します。
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

出力例:
```
{
  "data": "Hello Watson"
  }
```
{: screen}


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

# {{site.data.keyword.texttospeechshort}} パッケージ 

{{site.data.keyword.texttospeechfull}} サービスが提供する API は、IBM の音声合成機能を使用して、テキストをさまざまな言語、方言、および発声の自然に聞こえる音声に合成します。
{:shortdesc}

このサービスでは、言語ごとに少なくとも 1 つの男性または女性の声 (場合によっては両方) がサポートされます。音声は、最小限の遅延でクライアントにストリーム化されて返されます。このサービスについて詳しくは、[IBM Cloud 資料](https://console.bluemix.net/docs/services/text-to-speech/index.html)を参照してください。

{{site.data.keyword.texttospeechshort}} パッケージには、以下のエンティティーが含まれています。エンティティー名をクリックすると、{{site.data.keyword.texttospeechshort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) | パッケージ | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url |{{site.data.keyword.texttospeechshort}} サービスを操作します。 |
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、voice、customization_id  | 音声を取得します。 |
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) | アクション | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | 音声をリストします。 |
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、accept、voice、customization_id  | 音声を合成します。 |
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、voice、format、customization_id  | 発音を取得します。 |
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、language、description  | カスタム・モデルを作成します。 |
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタム・モデルを削除します。 |
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタム・モデルを取得します。 |
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、language  | カスタム・モデルをリストします。 |
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、name、description、words  | カスタム・モデルを更新します。 |
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word、translation、part_of_speech  | カスタムの単語を追加します。 |
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、words  | カスタムの単語を追加します。 |
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word  | カスタムの単語を削除します。 |
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word  | カスタムの単語を取得します。 |
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id  | カスタムの単語をリストします。 |
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id  | ラベル付きデータを削除します。|

## {{site.data.keyword.texttospeechshort}} サービス・インスタンスの作成
{: #service_instance}

パッケージをインストールする前に、{{site.data.keyword.texttospeechshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.texttospeechshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/text_to_speech) します。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.texttospeechshort}} パッケージのインストール
{: #install}

{{site.data.keyword.texttospeechshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.texttospeechshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #texttospeech_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](bluemix_cli.html#cloudfunctions_cli)。
  2. [`wskdeploy` コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) をインストールして、ダウンロードしたバイナリーを PATH に追加します。

{{site.data.keyword.texttospeechshort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.texttospeechshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    wskdeploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.texttospeechshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。上記のコマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
    出力例:
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.texttospeechshort}} サービス・インスタンス資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #texttospeech_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、{{site.data.keyword.cos_short}} パッケージのインストール先の名前空間を選択します。名前空間は、組織名とスペース名の組み合わせから形成されます。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Text To Speech」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。 

6. パッケージのインストール後に「アクション」ページにリダイレクトされ、**text-to-speech-v1** という名前の新規パッケージを検索できます。

7. **text-to-speech-v1** パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。 
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。**注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **text-to-speech-v1** パッケージのアクションをクリックします。そのアクションの詳細ページが開きます。 
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。 
    3. 新しい**パラメーター**を入力します。key には `__bx_creds` を入力します。value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.texttospeechshort}} パッケージの使用
{: #usage}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

`list-voices` アクションを試行します。
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}

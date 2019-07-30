---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, functions, packages

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


# Text to Speech
{: #pkg_text_to_speech}

## パッケージのオプション

| パッケージ | 可用性 | 説明 |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | 事前インストール済み (東京では提供されていません) | テキストをスピーチに変換するパッケージ |
| [`text-to-speech-v1`](#text_to_speech_ins) | インストール可能 | {{site.data.keyword.texttospeechshort}} サービスを操作します。 |

## Watson Text to Speech
{: #text_to_speech}

この事前インストール済みパッケージは、東京地域では利用できません。 IAM 認証を使用してインストール可能な [Text to Speech](#text_to_speech_ins) パッケージを参照してください。
{: tip}

`/whisk.system/watson-textToSpeech` パッケージを利用して、テキストをスピーチに変換するための Watson API を簡単に呼び出すことができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | パッケージ | `username`、`password` | テキストをスピーチに変換するパッケージ。 |
| `/whisk.system/watson-textToSpeech/textToSpeech` | アクション | `payload`、`voice`、`accept`、`encoding`、`username`、`password` | テキストを音声に変換します。 |

パッケージ `/whisk.system/watson` は、アクション `/whisk.system/watson/textToSpeech` を含めて非推奨です。 代わりに、[インストール可能な {{site.data.keyword.texttospeechshort}} パッケージ](#text_to_speech_ins)を参照してください。
{: deprecated}

### {{site.data.keyword.cloud_notm}} での {{site.data.keyword.texttospeechshort}} パッケージのセットアップ

{{site.data.keyword.cloud_notm}} から {{site.data.keyword.openwhisk}} を使用している場合、パッケージ・バインディングは {{site.data.keyword.cloud_notm}} Watson サービス・インスタンス用に自動的に作成されます。

1. {{site.data.keyword.cloud_notm}} [ダッシュボード](https://cloud.ibm.com){: external}で {{site.data.keyword.texttospeechshort}} サービス・インスタンスを作成します。 サービス・インスタンスの名前、および現行の {{site.data.keyword.cloud_notm}} の組織とスペースを忘れないようにしてください。

2. 名前空間でパッケージを最新表示します。 最新表示により、作成した Watson サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **出力例**
  ```
  created bindings:
  Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  パッケージをリストして、パッケージ・バインディングが作成されたことを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **出力例**
  ```
  packages
  /myOrg_mySpace/Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### {{site.data.keyword.cloud_notm}} 外部での {{site.data.keyword.texttospeechshort}} パッケージのセットアップ

{{site.data.keyword.cloud_notm}} で {{site.data.keyword.openwhisk_short}} を使用していない場合、または {{site.data.keyword.cloud_notm}} の外部で {{site.data.keyword.texttospeechshort}} をセットアップしたい場合は、{{site.data.keyword.texttospeechshort}} サービスのパッケージ・バインディングを手動で作成する必要があります。{{site.data.keyword.texttospeechshort}} サービスのユーザー名とパスワードが必要になります。

{{site.data.keyword.texttospeechshort}} サービス用に構成されるパッケージ・バインディングを作成します。
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username <username> -p password <password>
```
{: pre}

### テキストから音声への変換

`/whisk.system/watson-textToSpeech/textToSpeech` アクションは、テキストを音声スピーチに変換します。 以下のパラメーターがサポートされます。

| `username` | Watson API ユーザー名。 |
| `password` | Watson API パスワード。 |
| `payload` | スピーチに変換するテキスト。 |
| `voice` | スピーカーの音声。 |
| `accept` | スピーチ・ファイルの形式。 |
| `encoding` | スピーチ・バイナリー・データのエンコード。 |

パッケージ・バインディング内の `textToSpeech` アクションをテストして、テキストを変換します。
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

**出力例**
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

インストール可能な {{site.data.keyword.texttospeechfull}} サービスには、IBM の音声合成機能を使用する API が用意されています。この音声合成機能は、テキストをさまざまな言語、方言、発声の、自然に聞こえる音声に合成します。
{:shortdesc}

このサービスでは、言語ごとに少なくとも 1 つの男性または女性の声 (場合によっては両方) がサポートされます。 音声は、最小限の遅延でクライアントにストリーム化されて返されます。 このサービスについて詳しくは、[IBM Cloud 資料](/docs/services/text-to-speech?topic=text-to-speech-about)を参照してください。

{{site.data.keyword.texttospeechshort}} パッケージには、以下のエンティティーが含まれています。 エンティティー名をクリックすると、{{site.data.keyword.texttospeechshort}} API リファレンスで詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html){: external} | パッケージ | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url` | {{site.data.keyword.texttospeechshort}} サービスを操作します。 |
| [`get-voice`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`voice`、`customization_id` | 音声を取得します。 |
| [`list-voices`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url` | 音声をリストします。 |
| [`synthesize`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`text`、`accept`、`voice`、`customization_id` | 音声を合成します。 |
| [`get-pronunciation`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`text`、`voice`、`format`、`customization_id` | 発音を取得します。 |
| [`create-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`language`、`description` | カスタム・モデルを作成します。 |
| [`delete-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | カスタム・モデルを削除します。 |
| [`get-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | カスタム・モデルを取得します。 |
| [`list-voice-models`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`language` | カスタム・モデルをリストします。 |
| [`update-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`name`、`description`、`words` | カスタム・モデルを更新します。 |
| [`add-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word`、`translation`、`part_of_speech` | カスタムの単語を追加します。 |
| [`add-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`words`  | カスタムの単語を追加します。 |
| [`delete-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word` | カスタムの単語を削除します。 |
| [`get-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word` | カスタムの単語を取得します。 |
| [`list-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、 `headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | カスタムの単語をリストします。 |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data){: external} | アクション | `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id` | ラベル付きデータを削除します。 |

### {{site.data.keyword.texttospeechshort}} サービス・インスタンスの作成
{: #service_instance_texttospeech}

パッケージをインストールする前に、{{site.data.keyword.texttospeechshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。
{: shortdesc}

1. [{{site.data.keyword.texttospeechshort}} サービス・インスタンスを作成します](https://cloud.ibm.com/catalog/services/text_to_speech){: external}。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

### {{site.data.keyword.texttospeechshort}} パッケージのインストール
{: #install_texttospeech}

{{site.data.keyword.texttospeechshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.texttospeechshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #texttospeech_cli}

**始める前に**
[{{site.data.keyword.cloud_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](/docs/openwhisk?topic=cloud-functions-cli_install)。

{{site.data.keyword.texttospeechshort}} パッケージをインストールするため、以下のコマンドを実行します。

1. {{site.data.keyword.texttospeechshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.texttospeechshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    これは IAM サービスであるため、サービス・インスタンスを作成した地域によっては、サービス・インスタンスの名前が異なる場合があります。 上記のコマンドが失敗した場合は、bind コマンドに以下のサービス名を使用します。
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}

    **出力例**
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.texttospeechshort}} サービス・インスタンスの資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    **出力例**
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

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ](https://cloud.ibm.com/openwhisk/create){: external} に移動します。

2. 名前空間ドロップダウン・メニューを使用して、パッケージをインストールする名前空間を選択します。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Text To Speech」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。

6. パッケージのインストール後にアクション・ページにリダイレクトされ、`text-to-speech-v1` という名前の新規パッケージを検索できます。

7. `text-to-speech-v1` パッケージでアクションを使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、[CLI の手順のステップ 4 と 6](#texttospeech_cli) に従います。
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。

  使用するアクションごとに以下のステップを実行する必要があります。
  {: note}

    1. 使用する `text-to-speech-v1` パッケージのアクションをクリックします。 そのアクションの詳細ページが開きます。
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。
    3. 新しいパラメーターを入力します。 key には `__bx_creds` を入力します。 value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.texttospeechshort}} パッケージの使用
{: #usage_texttospeech}

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



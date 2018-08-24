---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: Text to Speech パッケージ
{: #openwhisk_catalog_watson_texttospeech}

`/whisk.system/watson-textToSpeech` パッケージを利用して、テキストをスピーチに変換するための Watson API を簡単に呼び出すことができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | パッケージ | username、password | テキストをスピーチに変換するパッケージ |
| `/whisk.system/watson-textToSpeech/textToSpeech` | アクション | payload、voice、accept、encoding、username、password | テキストの音声への変換 |

**注**: パッケージ `/whisk.system/watson` は、アクション `/whisk.system/watson/textToSpeech` を含めて非推奨です。

## {{site.data.keyword.Bluemix_notm}} での Watson Text to Speech パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用している場合、パッケージ・バインディングは {{site.data.keyword.Bluemix_notm}} Watson サービス・インスタンス用に自動的に作成されます。

1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](http://console.bluemix.net)で Watson Text to Speech のサービス・インスタンスを作成します。

  サービス・インスタンスの名前、および自分が所属している {{site.data.keyword.Bluemix_notm}} の組織とスペースの名前を忘れないようにしてください。

2. 名前空間でパッケージを最新表示します。 最新表示により、作成した Watson サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  出力例:
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  パッケージをリストして、パッケージ・バインディングが作成されたことを確認します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 外部での Watson Text to Speech パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}} を使用していない場合、または {{site.data.keyword.Bluemix_notm}} の外部で Watson Text to Speech をセットアップしたい場合は、Watson Text to Speech サービスのパッケージ・バインディングを手動で作成する必要があります。 Watson Text to Speech サービスのユーザー名とパスワードが必要になります。

Watson Speech to Text サービス用に構成されるパッケージ・バインディングを作成します。
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## テキストをスピーチに変換

`/whisk.system/watson-textToSpeech/textToSpeech` アクションは、テキストを音声スピーチに変換します。 パラメーターは次のとおりです。

- `username`: Watson API ユーザー名。
- `password`: Watson API パスワード。
- `payload`: スピーチに変換するテキスト。
- `voice`: スピーカーの音声。
- `accept`: スピーチ・ファイルの形式。
- `encoding`: スピーチ・バイナリー・データのエンコード。

パッケージ・バインディング内の **textToSpeech** アクションを呼び出して、テキストを変換します。
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

出力例:
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}

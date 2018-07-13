---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: Translator パッケージ
{: #openwhisk_catalog_watson_translator}

`/whisk.system/watson-translator` パッケージを利用して、翻訳を行う Watson API を簡単に呼び出すことができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | パッケージ | username、password | テキスト翻訳および言語識別のパッケージ  |
| `/whisk.system/watson-translator/translator` | アクション | payload、translateFrom、translateTo、translateParam、username、password | テキストの翻訳 |
| `/whisk.system/watson-translator/languageId` | アクション | payload、username、password | 言語の識別 |

**注**: パッケージ `/whisk.system/watson` は、アクション `/whisk.system/watson/translate` および `/whisk.system/watson/languageId` を含めて非推奨です。

## {{site.data.keyword.Bluemix_notm}} での Watson Translator パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} から {{site.data.keyword.openwhisk}} を使用している場合、パッケージ・バインディングは {{site.data.keyword.Bluemix_notm}} Watson サービス・インスタンス用に自動的に作成されます。

1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](http://console.bluemix.net)で Watson Translator のサービス・インスタンスを作成します。 サービス・インスタンスの名前、および自分が所属している {{site.data.keyword.Bluemix_notm}} の組織とスペースの名前を忘れないようにしてください。

2. 名前空間でパッケージを最新表示します。 最新表示により、作成した Watson サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  出力例:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
  ```
  {: screen}

  パッケージをリストして、パッケージ・バインディングが作成されたことを確認します。
  ```
  ibmcloud wsk package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 外部での Watson Translator パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} で {{site.data.keyword.openwhisk_short}} を使用していない場合、または {{site.data.keyword.Bluemix_notm}} の外部で Watson Translator をセットアップしたい場合は、Watson Translator サービスのパッケージ・バインディングを手動で作成する必要があります。 Watson Translator サービスのユーザー名とパスワードが必要になります。

- Watson Translator サービス用に構成されるパッケージ・バインディングを作成します。
  ```
  ibmcloud wsk package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## テキストの翻訳

`/whisk.system/watson-translator/translator` アクションは、テキストをある言語から別の言語に翻訳します。 パラメーターは次のとおりです。

- `username`: Watson API ユーザー名。
- `password`: Watson API パスワード。
- `payload`: 翻訳するテキスト。
- `translateParam`: 翻訳するテキストを示す入力パラメーター。 例えば、`translateParam=payload` の場合は、そのアクションに渡される `payload` パラメーターの値が翻訳されます。
- `translateFrom`: ソース言語の 2 桁のコード。
- `translateTo`: ターゲット言語の 2 桁のコード。

パッケージ・バインディング内の **translator** アクションを呼び出して、テキストを英語からフランス語に翻訳します。
```
ibmcloud wsk action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

出力例:
```
{
    "payload": "Ciel bleu a venir"
  }
```
{: screen}

## テキストの言語の識別

`/whisk.system/watson-translator/languageId` アクションは、テキストの言語を識別します。 パラメーターは次のとおりです。

- `username`: Watson API ユーザー名。
- `password`: Watson API パスワード。
- `payload`: 識別するテキスト。

パッケージ・バインディング内の **languageId** アクションを呼び出して、言語を識別します。
```
ibmcloud wsk action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

出力例:
```
{
  "payload": "Ciel bleu a venir",
    "language": "fr",
    "confidence": 0.710906
  }
```
{: screen}

---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson Speech to Text パッケージの使用
{: #openwhisk_catalog_watson_texttospeech}

`/whisk.system/watson-speechToText` パッケージを利用して、スピーチをテキストに変換するための Watson API を簡単に呼び出すことができます。
{: shortdesc}

このパッケージには、以下のアクションが含まれています。

| エンティティー| タイプ | パラメーター| 説明 |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | パッケージ| username、password| スピーチをテキストに変換するパッケージ|
| `/whisk.system/watson-speechToText/speechToText` | アクション | payload、content_type、encoding、username、password、continuous、inactivity_timeout、interim_results、keywords、keywords_threshold、max_alternatives、model、timestamps、watson-token、word_alternatives_threshold、word_confidence、X-Watson-Learning-Opt-Out| 音声のテキストへの変換|

**注**: パッケージ `/whisk.system/watson` は、アクション `/whisk.system/watson/speechToText` を含めて非推奨です。

## {{site.data.keyword.Bluemix_notm}} での Watson Speech to Text パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} から OpenWhisk を使用している場合、{{site.data.keyword.Bluemix_notm}} Watson サービス・インスタンスのパッケージ・バインディングは OpenWhisk が自動的に作成します。

1. {{site.data.keyword.Bluemix_notm}} [ダッシュボード](http://console.ng.Bluemix.net)で Watson Speech to Text のサービス・インスタンスを作成します。
  
  サービス・インスタンスの名前、およびユーザーが所属している {{site.data.keyword.Bluemix_notm}} の組織とスペースの名前を忘れないようにしてください。
  
2. 名前空間でパッケージを最新表示します。最新表示により、ユーザーが作成した Watson サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  wsk package refresh
  ```
  {: pre}
  
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  
  ```
  wsk package list
  ```
  {: pre}
  
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  

## {{site.data.keyword.Bluemix_notm}} 外部での Watson Speech to Text パッケージのセットアップ

{{site.data.keyword.Bluemix_notm}} で OpenWhisk を使用していない場合、または {{site.data.keyword.Bluemix_notm}} の外部で Watson Speech to Text をセットアップしたい場合は、Watson Speech to Text サービスのパッケージ・バインディングを手動で作成する必要があります。Watson Speech to Text サービスのユーザー名とパスワードが必要になります。

- Watson Speech to Text サービス用に構成されるパッケージ・バインディングを作成します。
  
  ```
  wsk package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}
  

## スピーチをテキストに変換

`/whisk.system/watson-speechToText/speechToText` アクションは、音声スピーチをテキストに変換します。パラメーターは次のとおりです。

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
 

- パッケージ・バインディング内の `speechToText` アクションを呼び出して、エンコードされた音声を変換します。
  ```
  wsk action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
  ```
  {: pre}

  ```json
  {
    "data": "Hello Watson"
  }
  ```
  

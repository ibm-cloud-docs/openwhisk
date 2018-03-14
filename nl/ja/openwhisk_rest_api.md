---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# OpenWhisk REST API の使用
{: #openwhisk_rest_api}

OpenWhisk 環境が有効になった後、OpenWhisk を、REST API 呼び出しを使用して Web アプリまたはモバイル・アプリと共に使用できます。
{: shortdesc}

アクション、アクティベーション、パッケージ、ルール、およびトリガー用の API について詳しくは、[ OpenWhisk API 資料](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json)を参照してください。


システム上のすべての機能は、REST API を通じて使用可能です。アクション、トリガー、ルール、パッケージ、アクティベーション、および名前空間のコレクション・エンドポイントとエンティティー・エンドポイントが使用可能です。

使用可能なコレクション・エンドポイント:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

`{APIHOST}` は、OpenWhisk API ホスト名 (例えば、openwhisk.ng.bluemix.net、172.17.0.1、192.168.99.100、192.168.33.13 など) です。
`{namespace}` には、文字 `_` を使用して、ユーザーの
*デフォルト名前空間* を指定できます。

コレクション・エンドポイントで GET 要求を実行して、コレクションのエンティティーのリストをフェッチします。

エンティティーのタイプごとに、以下のエンティティー・エンドポイントが使用可能です。
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

名前空間とアクティベーションのエンドポイントは GET 要求をサポートします。アクション、トリガー、ルール、およびパッケージのエンドポイントは、GET、PUT、および DELETE の要求をサポートします。アクション、トリガー、およびルールのエンドポイントは、POST 要求もサポートします。POST 要求は、アクションおよびトリガーの呼び出しや、ルールの有効化または無効化に使用されます。 

すべての API は、HTTP 基本認証で保護されています。
[wskadmin ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) ツールを使用して、新しい名前空間および認証を生成できます。
基本認証の資格情報は `~/.wskprops` ファイルの `AUTH` プロパティーにあり、コロンで区切られています。
CLI を使用して `wsk property get --auth` を実行することで、これらの資格情報を取得することもできます。


次の例では、[cURL](https://curl.haxx.se) コマンド・ツールを使用して、`whisk.system` 名前空間内のすべてのパッケージのリストを取得します。
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

この例では、`-u` フラグを使用して認証が渡されました。また、この値を URL の一部としても渡すことができます (例: `https://$AUTH@{APIHOST}`)。

OpenWhisk API は、Web クライアントからの要求/応答呼び出しをサポートします。OpenWhisk は、Cross-Origin Resource Sharing ヘッダーを使用して `OPTIONS` 要求に応答します。現在は、すべてのオリジンが許可され (すなわち、Access-Control-Allow-Origin は「`*`」)、Access-Control-Allow-Headers に Authorization と Content-Type が使用されます。

**注意:** OpenWhisk は、現在は名前空間ごとに 1 つのキーしかサポートしていないため、試験的な目的の範囲を超えて CORS を使用することは推奨されません。[Web アクション](./openwhisk_webactions.html)または [API ゲートウェイ](./openwhisk_apigateway.html)を使用してアクションをパブリックに公開し、CORS を必要とするクライアント・アプリケーションに OpenWhisk 許可キーは使用しないでください。

## CLI 冗長モードの使用
{: #openwhisk_rest_api_cli_v}

OpenWhisk CLI は、OpenWhisk REST API へのインターフェースです。
フラグ `-v` を使用して CLI を冗長モードで実行できます。これにより、HTTP 要求と応答に関するすべての情報が出力されます。

次のコマンドを実行して、現行ユーザーの名前空間値を表示します。
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

出力された情報は、HTTP 要求のプロパティーを示し、User-Agent ヘッダー `OpenWhisk-CLI/1.0 (<CLI-Build-version>)` および基本許可ヘッダー `Basic XXXYYYY` を使用して、URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` で HTTP メソッド `GET` を実行します。
許可の値は、base64 エンコードの OpenWhisk 許可ストリングです。
応答のコンテンツ・タイプは `application/json` です。

## アクション
{: #openwhisk_rest_api_actions}

アクションを作成または更新するには、アクションのコレクションに対するメソッド `PUT` を指定して HTTP 要求を送信します。例えば、単一ファイル・コンテンツを使用して、`hello` という名前の `nodejs:6` アクションを作成するには、以下のコマンドを使用します。
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true
```
{: pre}

アクションに対してブロッキング呼び出しを実行し、メソッド `POST` と、入力パラメーター `name` を含む本体を指定して HTTP 要求を送信するには、以下のコマンドを使用します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'
```
{: pre}

以下の応答を受け取ります。
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
`response.result` を取得するには、次の例のように、照会パラメーター `result=true` を指定してコマンドを再実行します。
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'
```
{: pre}
以下の応答を受け取ります。
```json
{
  "payload": "hello John"
}
```

## アノテーションおよび Web アクション
{: #openwhisk_rest_api_webactions}

アクションを Web アクションとして作成するには、Web アクションに `web-export=true` の[アノテーション](./openwhisk_annotations.html)を追加する必要があります。Web アクションはパブリックにアクセス可能であるため、アノテーション `final=true` を使用して、事前定義パラメーターを保護する (つまり、最終的なものとして扱う) 必要があります。CLI フラグ `--web true` を使用してアクションを作成または更新すると、このコマンドは `web-export=true` と `final=true` の両方のアノテーションを追加します。

以下の curl コマンドを実行して、アクションに設定するアノテーションの全リストを提供します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

これで、OpenWhisk 許可を持たずに、パブリック URL としてこのアクションを呼び出すことができます。Web アクションのパブリック URL を使用し、URL の末尾に拡張子 (例えば、`.json` または `.http` など) を含めて、呼び出してください。
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

この例のソース・コードは、`.http` では機能しません。修正方法については、[Web アクション](./openwhisk_webactions.html)の資料を参照してください。

## シーケンス
{: #openwhisk_rest_api_sequences}

アクション・シーケンスを作成するには、シーケンスを構成するアクションの名前を必要な順序で指定します。これにより、最初のアクションからの出力が、次のアクションの入力として渡されます。

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

アクション `/whisk.system/utils/split` と `/whisk.system/utils/sort` でシーケンスを作成します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

アクションは、完全修飾名で指定してください。

## トリガー
{: #openwhisk_rest_api_triggers}

トリガーを作成する場合に最低限必要な情報は、トリガーの名前です。また、トリガーの起動時にルールを介してアクションに渡されるデフォルト・パラメーターを含めることもできます。

名前 `events` で、デフォルトのパラメーター `type` に値 `webhook` を設定してトリガーを作成します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}'
```
{: pre}

これで、このトリガーの起動を必要とするイベントが発生するたびに、OpenWhisk 許可キーを使用して、メソッド `POST` を指定して HTTP 要求を行えば済むようになりました。

パラメーター `temperature` を使用してトリガー `events` を起動するには、以下の HTTP 要求を送信します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}'
```
{: pre}

### フィード・アクションを使用したトリガー
{: #openwhisk_rest_api_triggers_feed}

フィード・アクションを使用して、特別なトリガーを作成することができます。フィード・アクションとは、トリガーの対象イベントが発生するたびにトリガーの起動を担当するフィード・プロバイダーの構成に役立つアクションです。フィード・プロバイダーについて詳しくは、[feeds.md] 資料を参照してください。

フィード・アクションを利用する使用可能なトリガーには、定期/アラーム、Slack、Github、Cloudant/Couchdb、messageHub/Kafka が含まれます。また、独自のフィード・アクションおよびフィード・プロバイダーを作成することもできます。

2 時間という指定された頻度 (例えば 02:00:00、04:00:00 など) で起動される、名前が `periodic` というトリガーを作成します。

CLI を使用し、以下のコマンドを実行してトリガーを作成します。
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

`-v` フラグが使用されているため、2 つの HTTP 要求が送信されます。1 つは、`periodic` というトリガーを作成するため、もう 1 つはフィード・アクションを呼び出すためのものです。フィード・アクション `/whisk.system/alarms/alarm` には、トリガーを 2 時間ごとに起動するようフィード・プロバイダーを構成するためのパラメーターが送信されます。

REST API でこのアクティビティーを実行するには、最初に以下のようにトリガーを作成します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'
```
{: pre}

アノテーション `Feed` がトリガーに保管されているのが分かります。後で、このアノテーションを使用して、トリガーを削除するために使用するフィード・アクションを知ることができます。

これで、トリガーが作成されたので、フィード・アクションを呼び出しましょう。
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}"
```
{: pre}

トリガーの削除は、トリガーの作成と似ています。そのため今回は、トリガーのハンドラーも削除するようにフィード・プロバイダーを構成するためにフィード・アクションを使用することで、トリガーを削除します。

以下のコマンドを実行して、フィード・プロバイダーからトリガー・ハンドラーを削除するフィード・アクションを起動します。
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"
```
{: pre}

ここで、`DELETE` メソッドを使用して、HTTP 要求でトリガーを削除します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json"
```
{: pre}

## ルール
{: #openwhisk_rest_api_rules}

トリガーをアクションに関連付けるルールを作成するには、`PUT` メソッドを使用し、要求の本体でトリガーとアクションを指定して、HTTP 要求を送信します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}'
```
{: pre}

ルールは、有効または無効にすることができます。ルールの状況を変更するには、その status プロパティーを更新します。例えば、ルール `t2a` を無効にするには、`POST` メソッドを使用して、要求の本体で `status: "inactive"` を送信します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'
```
{: pre}

## パッケージ
{: #openwhisk_rest_api_packages}

パッケージにアクションを作成するには、まずパッケージを作成する必要があります。`iot` という名前のパッケージを作成するには、以下のコマンドを実行して、`PUT` メソッドを指定した HTTP 要求を送信します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}'
```
{: pre}

## アクティベーション
{: #openwhisk_rest_api_activations}

最後の 3 つのアクティベーションのリストを取得するには、次のような照会パラメーター `limit=3` を渡して、`GET` メソッドを指定した HTTP 要求を使用します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

結果およびログを含めたアクティベーションのすべての詳細を取得するには、次のように、アクティベーション ID をパス・パラメーターとして渡して、`GET` メソッドを指定した HTTP 要求を送信します。
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}

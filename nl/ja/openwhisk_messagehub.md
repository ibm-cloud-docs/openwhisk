---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub パッケージの使用
{: #openwhisk_catalog_message_hub}

ネイティブのハイパフォーマンス Kafka API を使用してメッセージのパブリッシュとコンシュームと行うための [Message Hub](https://developer.ibm.com/messaging/message-hub) インスタンスとの通信を可能にするパッケージ。
{: shortdesc}

## IBM MessageHub インスタンスを listen するトリガーの作成
{: #openwhisk_catalog_message_hub_trigger}

Message Hub インスタンスにメッセージがポストされると反応するトリガーを作成するためには、`/messaging/messageHubFeed` という名前のフィードを使用する必要があります。 このフィード・アクションは、以下のパラメーターをサポートします。

|名前|タイプ|説明|
|---|---|---|
|kafka_brokers_sasl|JSON ストリング配列|このパラメーターは、Message Hub インスタンス内のブローカーからなる `<host>:<port>` ストリングの配列です。|
|user|ストリング|Message Hub ユーザー名|
|password|ストリング|Message Hub パスワード|
|topic|ストリング|トリガーが listen するようにしたいトピック|
|kafka_admin_url|URL ストリング|Message Hub 管理 REST インターフェースの URL|
|isJSONData|ブール (オプション - デフォルトは false)|`true` に設定されている場合、プロバイダーは、メッセージ値をトリガー・ペイロードとして渡す前に JSON として構文解析しようとします。|
|isBinaryKey|ブール (オプション - デフォルトは false)|`true` に設定されている場合、プロバイダーは、キー値をトリガー・ペイロードとして渡す前に Base64 としてエンコードします。|
|isBinaryValue|ブール (オプション - デフォルトは false)|`true` に設定されている場合、プロバイダーは、メッセージ値をトリガー・ペイロードとして渡す前に Base64 としてエンコードします。|

このパラメーター・リストは難しそうに見えるかもしれませんが、package refresh CLI コマンドを使用して自動的に設定できます。

1. OpenWhisk 用に使用している現行の組織およびスペースの下に、Message Hub サービスのインスタンスを作成します。

2. listen したいトピックが Message Hub にあることを検証するか、新規トピック (例: `mytopic`) を作成します。

3. 名前空間内のパッケージをリフレッシュします。このリフレッシュにより、作成した Message Hub サービス・インスタンス用のパッケージ・バインディングが自動的に作成されます。

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  これで、パッケージ・バインディングには、Message Hub インスタンスと関連付けられた資格情報が含まれるようになります。

4. 残っている作業は、新規メッセージが Message Hub トピックにポストされたら起動されるトリガーを作成することだけです。

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## {{site.data.keyword.Bluemix_notm}} 外部での Message Hub パッケージのセットアップ

Message Hub を {{site.data.keyword.Bluemix_notm}} の外部でセットアップしたい場合は、Message Hub サービス用のパッケージ・バインディングを手動で作成する必要があります。Message Hub サービス資格情報と接続情報が必要です。

1. Message Hub サービス用に構成されたパッケージ・バインディングを作成します。

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 次に、新規パッケージを使用して、新規メッセージが Message Hub トピックにポストされたら起動されるトリガーを作成できます。

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## メッセージの listen
{: #openwhisk_catalog_message_hub_listen}

トリガーが作成されると、システムはメッセージング・サービス内の指定されたトピックをモニターします。新規メッセージがポストされると、トリガーが起動されます。

そのトリガーのペイロードには、`messages` フィールドが含まれています。これは、トリガーの前回の起動以降にポストされたメッセージの配列です。配列内の各メッセージ・オブジェクトには以下のフィールドが含まれています。
- topic
- partition
- offset
- key
- value

これらのフィールドは Kafka 用語では自明です。ただし、`key` には、`key` がバイナリー・データを伝送するのを許可する `isBinaryKey` というフィーチャーがあります。また、`value` には特別な考慮が必要です。JSON メッセージおよびバイナリー・メッセージを処理するために `isJSONData` フィールドおよび `isBinaryValue` フィールドが使用可能です。`isJSONData` フィールドと `isBinaryValue` フィールドを同時に使用することはできません。

一例として、トリガーが作成されたときに `isBinaryKey` が `true` に設定された場合、`key` は、起動されたトリガーのペイロードから返されるときに Base64 ストリングとしてエンコードされます。

`isBinaryKey` を `true` に設定して `Some key` という `key` がポストされる場合、トリガー・ペイロードは次の例のようになります。

```json
{
    "messages": [
        {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```

トリガーが作成されたときに `isJSONData` パラメーターが `false` に設定された (またはまったく設定されなかった) 場合、`value` フィールドは、ポストされたメッセージの未加工値です。しかし、トリガーが作成されたときに `isJSONData` が `true` に設定された場合、システムはこの値をできる限り JSON オブジェクトとして構文解析しようとします。構文解析が成功すると、その結果の JSON オブジェクトがトリガー・ペイロード内の `value` になります。

`isJSONData` を `true` に設定して、`{"title": "Some string", "amount": 5, "isAwesome": true}` というメッセージがポストされた場合、トリガー・ペイロードは次の例のようになります。

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": {
          "amount": 5,
          "isAwesome": true,
          "title": "Some string"
      }
    }
  ]
}
```

しかし、`isJSONData` を `false` に設定して、同じメッセージ内容がポストされた場合、トリガー・ペイロードは次の例のようになります。

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```

`isJSONData` と同様に、トリガーが作成されたときに `isBinaryValue` が `true` に設定された場合、トリガー・ペイロード内の結果の `value` は Base64 ストリングとしてエンコードされます。

`isBinaryValue` を `true` に設定して `Some data` という `value` がポストされた場合、トリガー・ペイロードは次の例のようになります。

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```

`isBinaryData` を `true` に設定せずに、同じメッセージがポストされた場合、トリガー・ペイロードは次の例のようになります。

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```

### メッセージのバッチ処理
トリガー・ペイロードにはメッセージの配列が含まれることに注意してください。これらのメッセージがメッセージング・システムに対して迅速に生成される場合、フィードは、ポストされたメッセージを単一のトリガー起動にまとめようとします。このバッチ処理によって、メッセージがトリガーにポストされるのが、より高速かつ効率的になります。

トリガーによって起動されるアクションをコーディングするときには、ペイロード内のメッセージの数は技術的には無制限であっても、常に 0 より大きいことに留意してください。以下に、バッチ処理されたメッセージの例を示します (*offset* 値の変化に注意してください)。
 
 ```json
 {
   "messages": [
       {
         "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
             "amount": 5
         }
       },
       {
         "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
             "amount": 1
         }
       },
       {
         "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
             "amount": 999
         }
       }
   ]
 }
 ```

## Message Hub へのメッセージの生成
Message Hub へのメッセージの生成を OpenWhisk アクションを使用して便利に行いたい場合、`/messaging/messageHubProduce` アクションを使用できます。このアクションには以下のパラメーターがあります。

|名前|タイプ|説明|
|---|---|---|
|kafka_brokers_sasl|JSON ストリング配列|このパラメーターは、Message Hub インスタンス内のブローカーからなる `<host>:<port>` ストリングの配列です。|
|user|ストリング|Message Hub ユーザー名|
|password|ストリング|Message Hub パスワード|
|topic|ストリング|トリガーが listen するようにしたいトピック|
|value|ストリング|生成したいメッセージの値|
|key|ストリング (オプション)|生成したいメッセージのキー|

最初の 3 つのパラメーターは `wsk package refresh` を使用して自動的にバインドできますが、すべての必要なパラメーターを指定してアクションを呼び出す以下の例を参照してください。

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## 例

### OpenWhisk と IBM Message Hub、Node Red、IBM Watson IoT、IBM Object Storage、および IBM Data Science Experience の統合
OpenWhisk と IBM Message Hub、Node Red、IBM Watson IoT、IBM Object Storage、IBM Data Science Experience (Spark) サービスを統合する例は、[ここにあります](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0)。

## 参照
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)

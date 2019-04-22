---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-19"

keywords: message hub, event, trigger, messages, batch, listen

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Event Streams イベント・ソース
{: #openwhisk_catalog_message_hub}

{{site.data.keyword.messagehub_full}} インスタンスにメッセージがポストされたときに反応するトリガーを、フィードを使用して作成できます。 ここでは、{{site.data.keyword.Bluemix}} を使用して、または使用せずに、{{site.data.keyword.messagehub}} トリガーを作成する方法、メッセージを listen する方法、およびメッセージを一括処理する方法について説明します。
{: shortdesc}

## {{site.data.keyword.messagehub}} パッケージ 

`/messaging/messageHubProduce` アクションは非推奨であり、将来削除されます。 東京地域では既に削除されています。最適なパフォーマンスを維持するために、`/messaging/messageHubProduce` アクションの使用をマイグレーションして、Message Hub/Kafka へのデータの生成が行われる場合は持続接続を使用するようにしてください。
{: deprecated}

このパッケージは、ネイティブのハイパフォーマンス Kafka API を使用してメッセージのパブリッシュとコンシュームを行うための [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) インスタンスとの通信を可能にします。 {{site.data.keyword.messagehub}} パッケージの詳細、パッケージのセットアップ方法、およびメッセージの生成方法については、[{{site.data.keyword.messagehub}} パッケージ](/docs/openwhisk?topic=cloud-functions-catalog_message_hub)のトピックを参照してください。

## {{site.data.keyword.messagehub}} インスタンスを listen するトリガーの作成
{: #create_message_hub_trigger}

{{site.data.keyword.messagehub}} インスタンスにメッセージがポストされると反応するトリガーを作成するためには、`/messaging/messageHubFeed` という名前のフィードを使用する必要があります。 フィード・アクションは、以下のパラメーターをサポートします。

|名前|タイプ|説明|
|---|---|---|
|kafka_brokers_sasl|JSON ストリング配列|このパラメーターは、{{site.data.keyword.messagehub}} インスタンス内のブローカーからなる `<host>:<port>` ストリングの配列です。|
|user|ストリング|{{site.data.keyword.messagehub}} ユーザー名。|
|password|ストリング|{{site.data.keyword.messagehub}} パスワード。|
|topic|ストリング|トリガーが listen するようにしたいトピック。|
|kafka_admin_url|URL ストリング|{{site.data.keyword.messagehub}} 管理 REST インターフェースの URL。|
|isJSONData|ブール (オプション - デフォルトは false)|`true` に設定されている場合、プロバイダーは、メッセージ値をトリガー・ペイロードとして渡す前に JSON として構文解析しようとします。|
|isBinaryKey|ブール (オプション - デフォルトは false)|`true` に設定されている場合、プロバイダーは、キー値をトリガー・ペイロードとして渡す前に Base64 としてエンコードします。|
|isBinaryValue|ブール (オプション - デフォルトは false)|`true` に設定されている場合、プロバイダーは、メッセージ値をトリガー・ペイロードとして渡す前に Base64 としてエンコードします。|

このパラメーター・リストは難しそうに見えるかもしれませんが、`ibmcloud fn package refresh` CLI プラグイン・コマンドを使用して自動的に設定できます。

1. {{site.data.keyword.openwhisk}} 用に使用している現行の組織およびスペースの下に、{{site.data.keyword.messagehub}} サービスのインスタンスを作成します。

2. listen したいトピックが {{site.data.keyword.messagehub}} にあることを検証するか、新規トピック (例: **mytopic**) を作成します。

3. 名前空間でパッケージを最新表示します。 最新表示により、作成した {{site.data.keyword.messagehub}} サービス・インスタンスのパッケージ・バインディングが自動的に作成されます。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  出力例:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. 名前空間内のパッケージをリストして、パッケージ・バインディングが使用可能になったことを示します。
  ```
  ibmcloud fn package list
  ```
  {: pre}

  出力例:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  これで、パッケージ・バインディングには、{{site.data.keyword.messagehub}} インスタンスと関連付けられた資格情報が含まれるようになります。

5. 残っている作業は、新規メッセージが {{site.data.keyword.messagehub}} トピックにポストされたら起動されるトリガーを作成することだけです。
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## {{site.data.keyword.Bluemix_notm}} 外部の {{site.data.keyword.messagehub}} パッケージ用のトリガーの作成
{: #create_message_hub_trigger_outside}

{{site.data.keyword.messagehub}} を {{site.data.keyword.Bluemix_notm}} の外部でセットアップしたい場合は、{{site.data.keyword.messagehub}} サービス用のパッケージ・バインディングを手動で作成する必要があります。 {{site.data.keyword.messagehub}} サービス資格情報と接続情報が必要です。

1. {{site.data.keyword.messagehub}} サービス用に構成されるパッケージ・バインディングを作成します。
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. 次に、新規パッケージを使用して、新規メッセージが {{site.data.keyword.messagehub}} トピックにポストされたら起動されるトリガーを作成できます。
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## メッセージの listen
{: #message_hub_listen}

トリガーが作成されると、システムはメッセージング・サービス内の指定されたトピックをモニターします。 新規メッセージがポストされると、トリガーが起動されます。

そのトリガーのペイロードには、`messages` フィールドが含まれています。これは、トリガーの前回の起動以降にポストされたメッセージの配列です。 配列内の各メッセージ・オブジェクトには以下のフィールドが含まれています。
- topic
- partition
- offset
- key
- value

これらのフィールドは Kafka 用語では自明です。 ただし、`key` には、`key` がバイナリー・データを伝送するのを許可する `isBinaryKey` というフィーチャーがあります。 また、`value` には特別な考慮が必要です。 JSON メッセージおよびバイナリー・メッセージを処理するために `isJSONData` フィールドおよび `isBinaryValue` フィールドが使用可能です。 `isJSONData` フィールドと `isBinaryValue` フィールドを同時に使用することはできません。

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
{: codeblock}

トリガーが作成されたときに `isJSONData` パラメーターが `false` に設定された (またはまったく設定されなかった) 場合、`value` フィールドは、ポストされたメッセージの未加工値です。 しかし、トリガーが作成されたときに `isJSONData` が `true` に設定された場合、システムはこの値をできる限り JSON オブジェクトとして構文解析しようとします。 構文解析が成功すると、その結果の JSON オブジェクトがトリガー・ペイロード内の `value` になります。

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
{: codeblock}

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
{: codeblock}

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
{: codeblock}

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
{: codeblock}

### メッセージのバッチ処理
トリガー・ペイロードにはメッセージの配列が含まれることに注意してください。 これらのメッセージがメッセージング・システムに対して迅速に生成される場合、フィードは、ポストされたメッセージを単一のトリガー起動にまとめようとします。 このバッチ処理によって、メッセージがトリガーにポストされるのが、より高速かつ効率的になります。

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

## 例
{: #examples}

### OpenWhisk と {{site.data.keyword.messagehub}}、Node Red、IBM Watson IoT、{{site.data.keyword.cos_full_notm}}、および IBM Data Science Experience の統合
OpenWhisk と {{site.data.keyword.messagehub}}、Node Red、IBM Watson IoT、{{site.data.keyword.cos_full}}、IBM Data Science Experience (Spark) サービスを統合する例は、[ここにあります](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0)。

## 参照
{: #message_references}
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org)

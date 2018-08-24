---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}} パッケージ 

{: #openwhisk_catalog_message_hub}

ネイティブのハイパフォーマンス Kafka API を使用してメッセージのパブリッシュとコンシュームと行うための [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) インスタンスとの通信を可能にするパッケージ。
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}} を使用した {{site.data.keyword.messagehub}} パッケージのセットアップ
{: #create_message_hub_ibm}

1. {{site.data.keyword.openwhisk}} 用に使用している現行の組織およびスペースの下に、{{site.data.keyword.messagehub}} サービスのインスタンスを作成します。

2. listen するトピックが {{site.data.keyword.messagehub}} にあることを確認するか、新規トピックを (例えば、**mytopic** というタイトルで) 作成します。

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

## {{site.data.keyword.Bluemix_notm}} 外部での {{site.data.keyword.messagehub}} パッケージのセットアップ

{{site.data.keyword.messagehub}} を {{site.data.keyword.Bluemix_notm}} の外部でセットアップしたい場合は、{{site.data.keyword.messagehub}} サービス用のパッケージ・バインディングを手動で作成する必要があります。 {{site.data.keyword.messagehub}} サービス資格情報と接続情報が必要です。

{{site.data.keyword.messagehub}} サービス用に構成されるパッケージ・バインディングを作成します。
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## イベントを使用したメッセージの listen

{{site.data.keyword.messagehub}} 内でトリガーを使用して、メッセージを listen する方法について詳しくは、
[{{site.data.keyword.messagehub}} イベント・ソース](./openwhisk_messagehub.html)のトピックを参照してください。そこでは、以下のタスクがカバーされています。
* [{{site.data.keyword.messagehub}} インスタンスを listen するトリガーの作成](./openwhisk_messagehub.html#create_message_hub_trigger)
* [{{site.data.keyword.Bluemix_notm}} の外部にある {{site.data.keyword.messagehub}} パッケージ用のトリガーの作成](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [メッセージの listen](./openwhisk_messagehub.html#message_hub_listen)
* [Examples](./openwhisk_messagehub.html#examples)

## {{site.data.keyword.messagehub}} へのメッセージの生成
{: #producing_messages}

`/messaging/messageHubProduce` アクションは非推奨であり、将来削除されます。 最適なパフォーマンスを維持するために、`/messaging/messageHubProduce` アクションの使用をマイグレーションして、データが {{site.data.keyword.messagehub}}/Kafka に生成されるときには持続接続を使用するようにしてください。
{: tip}

{{site.data.keyword.messagehub}} へのメッセージを容易に生成するために {{site.data.keyword.openwhisk_short}} アクションの使用を望む場合、`/messaging/messageHubProduce` アクションを使用できます。 このアクションには以下のパラメーターがあります。

|名前|タイプ|説明|
|---|---|---|
|kafka_brokers_sasl|JSON ストリング配列|このパラメーターは、{{site.data.keyword.messagehub}} インスタンス内のブローカーからなる `<host>:<port>` ストリングの配列です。|
|user|ストリング|{{site.data.keyword.messagehub}} ユーザー名。|
|password|ストリング|{{site.data.keyword.messagehub}} パスワード。|
|topic|ストリング|トリガーが listen するようにしたいトピック。|
|value|ストリング|生成したいメッセージの値。|
|key|ストリング (オプション)|生成したいメッセージのキー。|

最初の 3 つのパラメーターは `ibmcloud fn package refresh` を使用して自動的にバインドできますが、すべての必要なパラメーターを指定してアクションを呼び出す以下の例を参照してください。
```
ibmcloud fn action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## 参照
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)

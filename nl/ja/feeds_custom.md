---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: feeds, serverless

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


# カスタム・イベント・プロバイダー・フィードの作成
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} はオープンな API をサポートしています。この API では、どのユーザーでもパッケージ内のフィードとしてイベント・プロデューサー・サービスを公開できます。
{: shortdesc}


## フィードのアーキテクチャー
{: #feeds_arch}

フィードの作成には、3 つのアーキテクチャー・パターンがあります。**フック**、**ポーリング**、**接続**です。

### フック

フック・パターンでは、別のサービスで公開されている [Webhook](https://en.wikipedia.org/wiki/Webhook) を使用して、フィードをセットアップします。この戦略では、URL に直接 POST してトリガーを起動するように、外部サービスで Webhook が構成されます。 この方法は、頻度が低いフィードを実装するのに適した、非常に簡単で最も魅力的な選択肢です。

例えば、[Github パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_github)と [プッシュ通知パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications)では Webhook が使用されています。


### ポーリング

ポーリング・パターンでは、定期的にエンドポイントをポーリングして新しいデータをフェッチするように {{site.data.keyword.openwhisk_short}} アクションを準備します。このパターンは、比較的簡単に構築できますが、イベントの頻度はポーリング間隔によって制限されます。

### 接続

接続パターンでは、独立したサービスがフィード・ソースに対する持続的な接続を維持します。接続ベースの実装では、長いポーリング間隔を使用してサービス・エンドポイントと通信したり、プッシュ通知をセットアップしたりします。

例えば、[{{site.data.keyword.cloudant}} パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_cloudant)では接続パターンが使用されています。



##  フィード・アクションの実装
{: #feeds_actions}

フィード・アクションは、アクションの 1 つであり、以下のパラメーターを受け入れます。
* **lifecycleEvent**: 「CREATE」、「READ」、「UPDATE」、「DELETE」、「PAUSE」、または「UNPAUSE」。
* **triggerName**: このフィードから生成されたイベントを含むトリガーの完全修飾名。
* **authKey**: トリガーの所有者である {{site.data.keyword.openwhisk_short}} ユーザーの基本認証資格情報。

フィード・アクションは、フィードを管理するために必要な他のパラメーターも受け入れることができます。 例えば、{{site.data.keyword.cloudant}} の changes フィード・アクションは、`dbname` と `username` を含むパラメーターを受け取ることを予期します。

ユーザーが `--feed` パラメーターを指定して CLI からトリガーを作成すると、該当するパラメーターでフィード・アクションが自動的に呼び出されます。

例えば、ユーザーが、バインドされたパラメーターとしてユーザー名とパスワードを指定して **cloudant** パッケージに `mycloudant` バインディングを作成します。ユーザーが CLI から次のコマンドを実行するとします。
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

すると、以下のコマンドに相当する処理が実行されます。
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

*changes* という名前のフィード・アクションは、これらのパラメーターを受け取り、{{site.data.keyword.cloudant_short_notm}} からのイベントのストリームをセットアップするために必要なアクションを実行すると予期されます。 フィード・アクションは適切な構成を使用することによって発生し、それがトリガーに送信されます。

{{site.data.keyword.cloudant_short_notm}} *changes* フィードの場合は、アクションが、接続ベースのアーキテクチャーで実装された *{{site.data.keyword.cloudant_short_notm}} トリガー*・サービスと直接対話します。

`ibmcloud fn trigger delete`、`ibmcloud fn trigger update` および `ibmcloud fn trigger get` でも同様のフィード・アクション・プロトコルが発生します。

## フックを使用したフィードの実装
{: #feeds_hooks}

イベント・プロデューサーが Webhook/コールバック機能をサポートしている場合は、フックを使用してフィードをセットアップします。

この方法では、{{site.data.keyword.openwhisk_short}} の外部に永続サービスを維持する必要がありません。すべてのフィード管理は、サード・パーティーの Webhook API と直接ネゴシエーションするステートレス {{site.data.keyword.openwhisk_short}} *フィード・アクション* を介して自然に発生します。

`CREATE` を指定して呼び出されると、フィード・アクションは、単に他のサービスのための Webhook をインストールし、リモート・サービスに {{site.data.keyword.openwhisk_short}} の該当する `fireTrigger` URL に通知を POST するよう求めます。

その Webhook は、次の例のように、URL に通知を送信するよう指示されます。

`POST /namespaces/{namespace}/triggers/{triggerName}`

POST 要求を含むこの形式は、トリガー・イベントのパラメーターを定義する JSON 文書として解釈されます。 {{site.data.keyword.openwhisk_short}} ルールは、これらのトリガー・パラメーターを、イベントの結果として起動するすべてのアクションに渡します。

## ポーリングを使用したフィードの実装
{: #feeds_polling}

永続接続や外部サービスを維持することなく、{{site.data.keyword.openwhisk_short}} 内だけで、フィード・ソースをポーリングするようにアクションをセットアップできます。

Webhook は使用できないが、大量の処理も低い応答待ち時間も必要としないフィードの場合、ポーリングを使用できます。

ポーリング・ベースのフィードをセットアップするため、フィード・アクションは `CREATE` のために呼び出されたときに以下のステップを実行します。

1. フィード・アクションは、`whisk.system/alarms` フィードを使用して、適切な頻度の定期的トリガーをセットアップします。
2. フィード開発者は、リモート・サービスをポーリングして新規イベントがあれば返す `pollMyService` アクションを作成します。
3. フィード・アクションは、*ルール* *T -> pollMyService* をセットアップします。

この手順は、別個のサービスをまったく必要とせずに全面的に {{site.data.keyword.openwhisk_short}} アクションを使用することによって、ポーリング・ベースのトリガーを実装します。

## 接続を使用したフィードの実装
{: #feeds_connections}

上記 2 つのアーキテクチャーは、単純で実装が簡単です。 しかし、高性能なフィードが必要な場合は、持続的な接続とロング・ポーリング、あるいは類似した技法に代わるものはありません。

{{site.data.keyword.openwhisk_short}} アクションは短時間実行でなければならないため、アクションはサード・パーティーへの永続的な接続を維持できません。 代わりに、常時実行する*プロバイダー・サービス* と呼ばれる別個のサービスを {{site.data.keyword.openwhisk_short}} の外部に立ち上げることができます。 プロバイダー・サービスは、ロング・ポーリングまたは他の接続ベースの通知をサポートする、サード・パーティー・イベント・ソースへの接続を維持できます。

プロバイダー・サービスには、{{site.data.keyword.openwhisk_short}} *フィード・アクション* がフィードを制御するのを可能にする REST API があります。 プロバイダー・サービスは、イベント・プロバイダーと {{site.data.keyword.openwhisk_short}} の間のプロキシーとして機能します。 　サード・パーティーからイベントを受け取ると、トリガーを起動することによってそれらを {{site.data.keyword.openwhisk_short}} に送信します。

{{site.data.keyword.cloudant_short_notm}} *changes* フィードは規範的な例です。これが立ち上げる `cloudanttrigger` サービスは、持続的な接続を介して {{site.data.keyword.cloudant_short_notm}} 通知と {{site.data.keyword.openwhisk_short}} トリガーの間を仲介します。


*alarm* フィードは、同様のパターンで実装されます。

接続ベースのアーキテクチャーは、パフォーマンスが最も高い選択肢ですが、ポーリングおよびフックのアーキテクチャーに比べると、オペレーションにかかるオーバーヘッドは多くなります。



---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# カスタム・イベント・プロバイダー
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} はオープンな API をサポートしています。この API では、どのユーザーでもパッケージ内のフィードとしてイベント・プロデューサー・サービスを公開できます。 このセクションでは、ユーザー独自のカスタム・フィードを提供する際に選択できる、アーキテクチャーや実装に関するいくつかのオプションについて説明します。
{: shortdesc}

この資料は、独自のフィードを公開しようとしている上級 {{site.data.keyword.openwhisk_short}} ユーザーを対象としています。 ほとんどの {{site.data.keyword.openwhisk_short}} ユーザーの場合、以下のアーキテクチャー・セクションをスキップしても問題ありません。

## フィードのアーキテクチャー

フィードの作成には、少なくとも 3 つのアーキテクチャー・パターンがあります。**フック**、**ポーリング**、および**接続**です。

### フック
*フック*・パターンでは、別のサービスによって公開されている [Webhook](https://en.wikipedia.org/wiki/Webhook) 機能を使用してフィードがセットアップされます。   この戦略では、URL に直接 POST してトリガーを起動するように、外部サービスで Webhook が構成されます。 この方法は、頻度が低いフィードを実装するのに適した、非常に簡単で最も魅力的な選択肢です。

<!-- The github feed is implemented using webhooks.  Put a link here when we have the open repo ready -->

### ポーリング
ポーリング・パターンでは、定期的にエンドポイントをポーリングして新しいデータをフェッチするよう {{site.data.keyword.openwhisk_short}} アクションが調整されます。 このパターンは、比較的簡単に構築できますが、イベントの頻度はポーリング間隔によって制限されます。

### 接続
接続パターンでは、分離したサービスがフィード・ソースへの持続的な接続を保持します。 接続ベースの実装は、長いポーリング間隔を使用して、または、プッシュ通知をセットアップするため、サービス・エンドポイントと対話する可能性があります。

<!-- Our cloudant changes feed is connection based.  Put a link here to
an open repo -->

<!-- What is the foundation for the Message Hub feed? If it is "connections" then lets put a link here as well -->

## フィードとトリガーの違い

フィードとトリガーは密接に関連していますが、技術的には別個の概念です。   

- {{site.data.keyword.openwhisk_short}} はシステムに流入してくる**イベント**を処理します。

- **トリガー**は、技術的には、複数のイベントからなるクラスの名称です。 各イベントは 1 つのトリガーにのみ属します。トリガーは、トピック・ベースのパブリッシュ/サブスクライブ・システムにおける*トピック* に似ています。 **ルール** *T -> A* は、「トリガー *T* からイベントが到着するたびに、トリガー・ペイロードを使用してアクション *A* を呼び出す」という意味です。

- **フィード**は、あるトリガー *T* にいずれも属している複数のイベントからなるストリームです。フィードは**フィード・アクション**によって制御され、フィード・アクションが、フィードを形成しているイベントのストリームの作成、削除、休止、および再開を処理します。 通常、フィード・アクションは、通知を管理する REST API を使用して、イベントを生成する外部サービスと対話します。

##  フィード・アクションの実装

*フィード・アクション* は、通常の {{site.data.keyword.openwhisk_short}} *アクション* であり、以下のパラメーターを受け入れます。
* **lifecycleEvent**: 「CREATE」、「READ」、「UPDATE」、「DELETE」、「PAUSE」、または「UNPAUSE」のいずれか。
* **triggerName**: このフィードから生成されたイベントを含むトリガーの完全修飾名。
* **authKey**: トリガーの所有者である {{site.data.keyword.openwhisk_short}} ユーザーの基本認証資格情報。

フィード・アクションは、フィードを管理するために必要な他のパラメーターも受け入れることができます。 例えば、{{site.data.keyword.cloudant}} の changes フィード・アクションは、*「dbname」*、*「username」* などのパラメーターを受け取ることを予期しています。

ユーザーが **--feed** パラメーターを指定して CLI からトリガーを作成すると、システムは自動的に適切なパラメーターを指定してフィード・アクションを呼び出します。

例えば、ユーザーが、バインドされたパラメーターとしてユーザー名およびパスワードを指定して **cloudant** パッケージ用に `mycloudant` バインディングを作成すると想定します。 ユーザーが CLI から次のコマンドを実行するとします。
```
ibmcloud wsk trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

そうすると、システムは以下のコマンドに相当する処理を暗黙的に実行します。
```
ibmcloud wsk action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

*changes* という名前のフィード・アクションは、これらのパラメーターを受け取り、{{site.data.keyword.cloudant_short_notm}} からのイベントのストリームをセットアップするために必要なアクションを実行すると予期されます。 フィード・アクションは適切な構成を使用することによって発生し、それがトリガー *T* に送信されます。

{{site.data.keyword.cloudant_short_notm}} *changes* フィードの場合は、アクションが、接続ベースのアーキテクチャーで実装された *{{site.data.keyword.cloudant_short_notm}} トリガー*・サービスと直接対話するようになっています。

`ibmcloud wsk trigger delete`、`ibmcloud wsk trigger update` および `ibmcloud wsk trigger get` でも同様のフィード・アクション・プロトコルが発生します。

## フックを使用したフィードの実装

イベント・プロデューサーが Webhook/コールバック機能をサポートしている場合、フックを使用することによってフィードを簡単にセットアップできます。

この方法では、{{site.data.keyword.openwhisk_short}} の外部に永続サービスを立ち上げる_必要はありません_。 すべてのフィード管理は、サード・パーティーの Webhook API と直接ネゴシエーションするステートレス {{site.data.keyword.openwhisk_short}} *フィード・アクション* を介して自然に発生します。

`CREATE` を指定して呼び出されると、フィード・アクションは、単に他のサービスのための Webhook をインストールし、リモート・サービスに {{site.data.keyword.openwhisk_short}} の該当する `fireTrigger` URL に通知を POST するよう求めます。

その Webhook は、次の例のように、URL に通知を送信するよう指示されます。

`POST /namespaces/{namespace}/triggers/{triggerName}`

POST 要求を含むこの形式は、トリガー・イベントのパラメーターを定義する JSON 文書として解釈されます。 {{site.data.keyword.openwhisk_short}} ルールは、これらのトリガー・パラメーターを、イベントの結果として起動するすべてのアクションに渡します。

## ポーリングを使用したフィードの実装

永続接続および外部サービスを何も立ち上げる必要なく、{{site.data.keyword.openwhisk_short}} 内だけで、フィード・ソースをポーリングするよう {{site.data.keyword.openwhisk_short}} *アクション* をセットアップすることが可能です。

Webhook は使用できないが、大量の処理も低い応答待ち時間も必要としないフィードの場合、ポーリングは魅力的な選択肢です。

ポーリング・ベースのフィードをセットアップするため、フィード・アクションは `CREATE` のために呼び出されたときに以下のステップを実行します。

1. フィード・アクションは、`whisk.system/alarms` フィードを使用して、適切な頻度の定期的トリガー (*T*) をセットアップします。
2. フィード開発者は、リモート・サービスをポーリングして新規イベントがあれば返す `pollMyService` アクションを作成します。
3. フィード・アクションは、*ルール* *T -> pollMyService* をセットアップします。

この手順は、別個のサービスをまったく必要とせずに全面的に {{site.data.keyword.openwhisk_short}} アクションを使用することによって、ポーリング・ベースのトリガーを実装します。

## 接続を使用したフィードの実装

上記 2 つのアーキテクチャーは、単純で実装が簡単です。 しかし、高性能なフィードが必要な場合は、持続的な接続とロング・ポーリング、あるいは類似した技法に代わるものはありません。

{{site.data.keyword.openwhisk_short}} アクションは短時間実行でなければならないため、アクションはサード・パーティーへの永続的な接続を維持できません。 代わりに、常時実行する*プロバイダー・サービス* と呼ばれる別個のサービスを {{site.data.keyword.openwhisk_short}} の外部に立ち上げることができます。 プロバイダー・サービスは、ロング・ポーリングまたは他の接続ベースの通知をサポートする、サード・パーティー・イベント・ソースへの接続を維持できます。

プロバイダー・サービスには、{{site.data.keyword.openwhisk_short}} *フィード・アクション* がフィードを制御するのを可能にする REST API があります。 プロバイダー・サービスは、イベント・プロバイダーと {{site.data.keyword.openwhisk_short}} の間のプロキシーとして機能します。 　サード・パーティーからイベントを受け取ると、トリガーを起動することによってそれらを {{site.data.keyword.openwhisk_short}} に送信します。

{{site.data.keyword.cloudant_short_notm}} *changes* フィードは規範的な例です。これが立ち上げる `cloudanttrigger` サービスは、持続的な接続を介して {{site.data.keyword.cloudant_short_notm}} 通知と {{site.data.keyword.openwhisk_short}} トリガーの間を仲介します。
<!-- TODO: add a reference to the open source implementation -->

*alarm* フィードは、同様のパターンで実装されます。

接続ベースのアーキテクチャーは、パフォーマンスが最も高い選択肢ですが、ポーリングおよびフックのアーキテクチャーに比べると、オペレーションにかかるオーバーヘッドは多くなります。

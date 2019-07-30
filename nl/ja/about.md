---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: platform architecture, openwhisk, couchdb, kafka, functions

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


# {{site.data.keyword.openwhisk_short}} の動作
{: #about}

{{site.data.keyword.openwhisk}} は、サーバーレス・コンピューティングまたは Function as a Service (FaaS) とも呼ばれるイベント・ドリブンの・コンピュート・プラットフォームであり、イベントまたは直接起動に応えてコードを実行します。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} テクノロジー
{: #about_technology}

{{site.data.keyword.openwhisk_short}} を支えるテクノロジーの基本的な概念について説明します。

**アクションとは**

アクションとは、呼び出し可能な小規模なコード断片、またはイベントの応答として自動的に実行されるように設定した小規模なコード断片です。 いずれの場合も、実行されるたびに、固有のアクティベーション ID で識別されるレコードが生成されます。 アクションの入力と結果は、キーと値のペアで示されます。 キーはストリングで、値は有効な JSON 値です。 アクションは任意の言語で作成できます。これはソース・コードまたは Docker イメージとしてサービスに渡されます。 アクション・コードは、Cloud Functions API、CLI、または iOS SDK によって直接呼び出されると実行されます。 IBM Cloud またはサード・パーティーのサービスからのイベントの応答としてアクションを自動的に実行させることもできます。

**アクションを使用するとよい理由**

アクションを使用することで、コードの実行時間が限定されるため、コストが削減されます。

例えば、アクションを使用して、画像内の顔を検出したり、データベースの変更に応答したり、一連の API 呼び出しを集約したり、ツイートを投稿したりできます。

**一度に複数のアクションを使用できますか?**

はい。 複数のアクションを使用して複数の他のアクションを呼び出したり、複数のアクションをつなげて[シーケンスを作成](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)したりできます。 そのように機能させるには、あるアクションの出力が別のアクションの入力となり、次いでそのアクションの出力が別のアクションのトリガーとして使用される、などといった処理が必要になります。 作成する複数のアクションのグループを束ねて、1 つのパッケージを形成することさえ可能です。 パッケージにすると、よく使用する複数のアクションやシーケンスをパッケージを呼び出すことで再利用できるので、アクションやシーケンスを何度も構成する必要がなくなります。

## {{site.data.keyword.openwhisk_short}} の用語

<dl>
  <dt>名前空間</dt>
    <dd>[名前空間](/docs/openwhisk?topic=cloud-functions-namespaces)は、アクションやトリガーなどの {{site.data.keyword.openwhisk_short}} エンティティーを入れるものであり、リソース・グループに属しています。 名前空間に対するアクセス権限をユーザーに付与すると、そのユーザーが {{site.data.keyword.openwhisk_short}} エンティティーにアクセスできるようになります。</dd>
  <dt>アクション</dt>
    <dd>[アクション](/docs/openwhisk?topic=cloud-functions-actions)は、1 つの特定のタスクを実行する 1 片のコードです。 アクションは、任意の言語で書くことができます。例えば、JavaScript や Swift コードの小さなスニペットにしたり、Docker コンテナーに組み込まれたカスタム・バイナリー・コードにしたりできます。 ユーザーは、ソース・コードまたは Docker イメージとして Cloud Functions にアクションを提供します。
    <br><br>アクションは、{{site.data.keyword.openwhisk_short}} API、CLI、または iOS SDK を使用して直接呼び出されたときに処理を実行します。 また、アクションは、{{site.data.keyword.cloud_notm}} サービスおよびサード・パーティー・サービスからのイベントに、トリガーを使用して自動的に応答することもできます。</dd>
  <dt>シーケンス</dt>
    <dd>一連のアクションをチェーニングして、コードを記述することなく、[シーケンス](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)にまとめることができます。 シーケンスは、順番に呼び出されるアクションのチェーンであり、1 つのアクションの出力が次のアクションへの入力として渡されます。 これにより、既存のアクションを組み合わせて、素早く簡単に再利用できます。 シーケンスは、アクションと同様に、REST API から呼び出したり、イベントに応答して自動的に呼び出したりできます。
  </dd>
  <dt>イベント</dt>
    <dd>イベントの例には、データベース・レコードへの変更、IoT センサーによる一定の気温を超えたことの感知、GitHub リポジトリーへの新規コードのコミット、Web アプリまたはモバイル・アプリからの単純な HTTP 要求などがあります。 外部および内部イベント・ソースからのイベントは、トリガーを介して送信され、ルールによってアクションがそうしたイベントに反応できます。</dd>
  <dt>トリガー</dt>
    <dd>[トリガー](/docs/openwhisk?topic=cloud-functions-triggers)は、ある種のイベントに対して指定されたチャネルです。 トリガーとは、ユーザーから起動されるか、イベント・ソースによって起動されるかに関わらず、特定のタイプのイベントに対応するための宣言です。</dd>
  <dt>ルール</dt>
    <dd>[ルール](/docs/openwhisk?topic=cloud-functions-rules)は、トリガーをアクションと関連付けます。 トリガーが起動するたびに、ルールはトリガー・イベントを入力として使用し、関連付けられているアクションを呼び出します。 適切なルール・セットを使用して、単一のトリガーのイベントで複数のアクションを呼び出すことも、複数のトリガーのイベントに対して 1 つのアクションを応答として呼び出すこともできます。</dd>
  <dt>フィード</dt>
    <dd>[フィード](/docs/openwhisk?topic=cloud-functions-triggers#triggers_feeds) は、{{site.data.keyword.openwhisk_short}} で消費可能なトリガー・イベントを起動するように、外部イベント・ソースを構成するための便利な方法です。 例えば、Git フィードでは、Git リポジトリーへのあらゆるコミットに対してトリガー・イベントを起動することがあります。</dd>
  <dt>パッケージ</dt>
    <dd>サービスおよびイベント・プロバイダーとの統合をパッケージで追加することができます。 [パッケージ](/docs/openwhisk?topic=cloud-functions-pkg_ov)は、フィードおよびアクションのバンドルです。 フィードは、トリガー・イベントを起動するように外部イベント・ソースを構成するコードです。 例えば、{{site.data.keyword.cloudant}} 変更フィードで作成されるトリガーは、{{site.data.keyword.cloudant_short_notm}} データベースで文書が変更されるか追加されるたびにそのトリガーを起動するようにサービスを構成します。 パッケージ内のアクションは、再使用可能なロジックを表します。サービス・プロバイダーがアクションを利用可能にすることによって、開発者はそのサービスをイベント・ソースとして使用し、そのサービスの API を呼び出すことができます。
    <br><br>既存のパッケージ・カタログを利用すると、素早く簡単に、有用な機能でアプリケーションを強化したり、エコシステム内の外部サービスにアクセスしたりできます。 {{site.data.keyword.openwhisk_short}} パッケージ対応の外部サービスの例として、{{site.data.keyword.cloudant_short_notm}}、The Weather Company、Slack、GitHub があります。</dd>
</dl>

### 次の作業
{: #quiz}
[クイズに答えて](https://ibmcloud-quizzes.mybluemix.net/functions/terms_quiz/quiz.php){: external}知識を試してみましょう!


## OpenWhisk の内部処理の仕組み
{: #about_internal}

すべてのコンポーネントをさらに詳細に説明するために、{{site.data.keyword.openwhisk_short}} システムからのアクションの呼び出しをトレースしてみます。 呼び出しによって、ユーザーがシステムにフィードしたコードが実行され、その実行結果が返されます。 次の図は、{{site.data.keyword.openwhisk_short}} アーキテクチャーの概要を示したものです。

![{{site.data.keyword.openwhisk_short}} アーキテクチャー](./images/OpenWhisk.png)

### OpenWhisk ではそれぞれの場面の背後で何が起こっているのでしょうか?
{: #about_scenes}

OpenWhisk は、NGINX、Kafka、Docker、CouchDB などのコンポーネントを結合して、サーバーレスのイベント・ベースのプログラミング・サービスを形成するオープン・ソース・プロジェクトです。

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="OpenWhisk の背後で行われる処理の内部フロー" style="width:550px; border-style: none"/>

#### 1. システムに入る: NGINX
{: #about_ngnix}

まず、ユーザーに面している OpenWhisk の API は、HTTP ベースであり、RESTful 設計に従っています。 したがって、CLI を介して送信されるコマンドは、OpenWhisk システムに対する HTTP 要求です。 特定のコマンドは、おおよそ次のように変換されます。
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

ここで *$userNamespace* 変数に注意してください。 1 人のユーザーは少なくとも 1 つの名前空間にアクセスできます。 単純化するため、*myAction* が入れられた名前空間をユーザーが所有していると想定します。

システムへの最初のエントリー・ポイントは、HTTP およびリバース・プロキシー・サーバーである **NGINX** を通るものです。 これは、SSL 終端処理のためと、適切な HTTP 呼び出しを次のコンポーネントに転送するために使用されます。

#### 2. システムに入る: Controller
{: #about_controller}


NGINX は、HTTP 要求を **Controller** (OpenWhisk を介したパスの次のコンポーネント) に転送します。 これは、実際の REST API (**Akka** および **Spray** に基づく) の Scala ベースの実装であるため、ユーザーが実行できるすべての処理のインターフェースの役割を果たします。 これには、OpenWhisk のエンティティーに対する[作成、取得、更新、削除](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete){: external}要求やアクションの呼び出しがあります。

Controller は、最初に、ユーザーが行おうとしていることのあいまいさを解消します。 これは HTTP 要求内で使用される HTTP メソッドに基づいて行われます。 ユーザーが POST 要求を既存のアクションに発行して、Controller がこの要求を**アクションの呼び出し**に変換します。

Controller の中心的役割 (この名前の由来) を考えると、Controller は以下のすべてのステップにある程度関与します。

#### 3. 認証および許可: CouchDB
{: #about_auth}

次に、Controller は、ユーザーが誰なのかを検証 (*認証*) し、そのエンティティーで実行しようとしていることを行う特権をユーザーが持っているかどうかを検証 (*許可*) します。 要求に組み込まれている資格情報が、**CouchDB** インスタンス内のいわゆる**サブジェクト**・データベースに照らして検証されます。

ここでは、ユーザーが OpenWhisk のデータベース内に存在していること、およびアクション *myAction* (ユーザーが所有している名前空間内のアクションであると想定) を呼び出すための特権を備えていることが検査されます。 ユーザーが名前空間を所有していることにより、実質上、ユーザーはアクションを呼び出すための特権を備えることになります。

すべてが正常に進んでいるため、次の処理ステージへのゲートが開きます。

#### 4. アクションを取得する: ここでも CouchDB
{: #about_couchdb}

Controller は、ユーザーがアクションを呼び出すための許可と特権を持っていることを確認できたので、このアクション (この例では *myAction*) を CouchDB 内の **whisks** データベースからロードします。

アクションのレコードに主として含まれるのは、実行するコードと、アクションに渡すデフォルト・パラメーター (実際の呼び出し要求に組み込んだパラメーターとマージされる) です。 また、実行時に課されるリソース制限 (例えば、消費が許可されるメモリー量) も含まれます。

この特定の例では、アクションはパラメーターを取りません (関数のパラメーター定義は空リストです)。 したがって、アクションの特定のパラメーターも含め、デフォルト・パラメーターは設定されません。そのため、この観点から見て最も簡素な例になります。


#### 5. アクションを呼び出す担当者: Load Balancer
{: #about_lb}

Load Balancer は、Controller の一部であり、実行プログラムの正常性状況を継続的にチェックすることにより、システム内で使用可能な実行プログラムの全体図を把握します。 それらの実行プログラムを **Invoker** と呼びます。 使用可能な Invoker を把握している Load Balancer は、その中から、要求されたアクションを起動するInvoker を選択します。

#### 6. 整列させる: Kafka
{: #about_kafka}

これ以降、送信した呼び出し要求に関して起こり得る望ましくない事態として、主に次の 2 つが考えられます。

1. システムがクラッシュして呼び出しが失われる可能性があります。
2. まず他の呼び出しが完了するのを待機しなければならないような重い負荷がシステムにかかる可能性があります。

両方に対する解答が、高スループットの分散パブリッシュ/サブスクライブ・メッセージング・システムである **Kafka** です。 Controller と Invoker は、Kafka によってバッファーに入れられて永続化されるメッセージを介してのみ通信します。 Kafka により、*OutOfMemoryException* のリスクがあるメモリー内でのバッファリングという負担から Controller と Invoker の両方が解放され、システムがクラッシュしてもメッセージが失われないことも確実になります。

次にアクションを呼び出すために、Controller はメッセージを Kafka にパブリッシュします。このメッセージには、呼び出すアクション、およびそのアクションに渡すパラメーター (この例ではゼロ個) が含まれます。 このメッセージの宛先は、Controller が Consul から取得したリストから選択した Invoker です。

Kafka がメッセージを取得したことを確認すると、ユーザーへの HTTP 要求は**アクティベーション ID** を伴って応答されます。ユーザーは後でそれを使用して、この特定の呼び出しの結果にアクセスできます。 これは非同期呼び出しモデルであり、HTTP 要求はアクションを呼び出す要求をシステムが受け入れると終了します。 同期モデル (ブロッキング呼び出しと呼ばれます) も使用可能ですが、ここでは説明しません。

#### 7. コードを呼び出す: Invoker
{: #about_invoker}

**Invoker** は OpenWhisk の中心です。 Invoker の責務は、アクションを呼び出すことです。 これも Scala で実装されます。 しかし、これにはそれ以上のものがあります。 隔離して安全にアクションを実行するために、**Docker** を使用します。

Docker の使用によって、隔離され制御された方法で素早くアクションが実行されるよう、呼び出すアクションごとに自己包含環境 (*コンテナー* と呼ばれます) が新しくセットアップされます。 アクションの呼び出しごとに、Docker コンテナーが spawn され、アクション・コードが注入されます。 その後、渡されたパラメーターを使用してコードが実行され、結果が取得され、コンテナーが破棄されます。 この段階でパフォーマンスの最適化を行って、保守要件を削減し、短い応答時間を可能にすることができます。

この例では、*Node.js* ベースのアクションが用意されており、Invoker が Node.js コンテナーを開始します。次に、*myAction* からコードを注入し、パラメーターなしで実行して、結果を抽出し、ログを保存してから、Node.js コンテナーを再度破棄します。

#### 8. 結果を保管する: ここでも CouchDB
{: #about_storing}

結果が Invoker によって取得されると、結果は **whisks** データベースに、アクティベーション ID の下でアクティベーションとして保管されます。**whisks** データベースは **CouchDB** 内にあります。

ここで使用している特定の例では、Invoker は結果の JSON オブジェクトをアクションから返され、Docker によって書き込まれたログを取得し、それらをすべてアクティベーション・レコードに入れ、そのレコードをデータベースに保管します。 以下の例を参照してください。
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

返された結果と書き込まれたログの両方がどのようにレコードに含まれているのかに注意してください。 また、アクションの呼び出しの開始時刻および終了時刻も含まれています。 アクティベーション・レコードには、より多くのフィールドが含まれていますが、この例では分かりやすくするために単純化されています。

次に、REST API を再度使用して (再びステップ 1 から開始して)、アクティベーションを取得し、アクションの結果を取得できます。 そのために、以下のコマンドを実行します。

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### まとめ
{: #about_summary}

単純な **ibmcloud fn action invoked myAction** がどのように {{site.data.keyword.openwhisk_short}} システムのさまざまなステージを通過していくのかが分かりました。 システム自体は、主に 2 つのみのカスタム・コンポーネント、**Controller** と **Invoker** からなります。 他のすべては、オープン・ソース・コミュニティーの多くの人によって開発され、既に存在しています。

{{site.data.keyword.openwhisk_short}} に関する追加情報が以下のトピックに記載されています。

* [エンティティー名](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)
* [アクションのセマンティクス](/docs/openwhisk?topic=cloud-functions-limits#limits_semantics)
* [制限](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)
* [REST API リファレンス](/apidocs/functions)





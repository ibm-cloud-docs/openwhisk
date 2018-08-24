---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# システムの詳細および制限
{: #openwhisk_reference}

以下のセクションでは、{{site.data.keyword.openwhisk}} システムに関する技術的な詳細と、制限設定について説明します。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} のエンティティー
{: #openwhisk_entities}

### 名前空間とパッケージ
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} のアクション、トリガー、およびルールは、名前空間に属し、場合によってはパッケージに属します。

パッケージは、アクションおよびフィードを含むことができます。 パッケージに別のパッケージを含めることはできないため、パッケージのネスティングはできません。 また、エンティティーは必ずしもパッケージに入れる必要はありません。

{{site.data.keyword.Bluemix_notm}} では、組織とスペースのペアが {{site.data.keyword.openwhisk_short}} 名前空間に対応します。 例えば、組織 `BobsOrg` とスペース `dev` は、{{site.data.keyword.openwhisk_short}} の名前空間 `/BobsOrg_dev` に対応します。

名前空間の作成権限があれば、独自の名前空間を作成することもできます。 名前空間 `/whisk.system` は、{{site.data.keyword.openwhisk_short}} システムと共に配布されるエンティティー用に予約済みです。

### 完全修飾名
{: #openwhisk_entities_fullyqual}

エンティティーの完全修飾名は、`/namespaceName[/packageName]/entityName` です。 名前空間、パッケージ、およびエンティティーを区切るには、`/` が使用されます。 また、名前空間には接頭部 `/` を付ける必要があります。

利便性のため、ユーザーの*デフォルト名前空間* の場合は名前空間を省略できます。

例えば、ユーザーのデフォルト名前空間が `/myOrg` であるとします。 以下に、いくつかのエンティティーの完全修飾名と別名の例を示します。

| 完全修飾名 | 別名 | 名前空間 | パッケージ | 名前 |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

中でも特に {{site.data.keyword.openwhisk_short}} CLI を使用する場合、この命名体系を使用できます。

### エンティティー名
{: #openwhisk_entities_names}

アクション、トリガー、ルール、パッケージ、および名前空間を含めて、すべてのエンティティーの名前は、次の形式の文字列です。

* 先頭文字は、英数字または下線でなければなりません。
* 後続の文字には、英数字、スペース、または `_`、`@`、`.`、`-` のどの値でも使用できます。
* 最後の文字をスペースにすることはできません。

より正確に言うと、名前は次の (Java メタキャラクター構文で表した) 正規表現に一致する必要があります。`&#xa5;A([&#xa5;w]|[&#xa5;w][&#xa5;w@ .-]*[&#xa5;w@.-]+)&#xa5;z`。

## アクションのセマンティクス
{: #openwhisk_semantics}

以下のセクションでは、{{site.data.keyword.openwhisk_short}} アクションについて詳しく説明します。

### ステートレス
{: #openwhisk_semantics_stateless}

アクションの実装はステートレス、つまり*べき等* です。 システムはこの特性を強制しませんが、アクションによって保持された状態が呼び出しをまたがって使用可能である保証はありません。

さらに、1 つのアクションについて複数のインスタンス化が存在し、それぞれが独自の状態であることもあります。 アクション呼び出しは、これらのインスタンス化のうち任意のものにディスパッチされる可能性があります。

### 呼び出しの入力および出力
{: #openwhisk_semantics_invocationio}

アクションの入力および出力は、キーと値のペアのディクショナリーです。 キーはストリングで、値は有効な JSON 値です。

### アクションの呼び出しの順序付け
{: #openwhisk_ordering}

アクションの呼び出しは順序付けられません。 ユーザーがコマンド・ラインまたは REST API からアクションを 2 回呼び出すと、2 番目の呼び出しが最初の呼び出しよりも前に実行される可能性があります。 それらのアクションに副次作用がある場合、副次作用は任意の順序で発現する可能性があります。

また、アクションが自動的に実行されることは保証されません。 2 つのアクションが並行して実行され、それらのアクションの副次作用が混ざり合う可能性があります。 OpenWhisk は、副次作用に対する特定の並行整合性モデルを保証していません。 並行性の副次作用は実装に依存します。

### アクション実行の保証
{: #openwhisk_atmostonce}

システムは、呼び出し要求を受け取ると、その要求を記録し、アクティベーションをディスパッチします。

システムは、受け取ったことを確認するアクティベーション ID を (非ブロッキング呼び出しと共に) 返します。
ユーザーが HTTP 応答を受け取る前にネットワーク障害またはその他の障害があった場合、{{site.data.keyword.openwhisk_short}} が要求を受け取って処理した可能性があります。

システムがアクションを 1 回呼び出そうとした場合の結果は、次の 4 つのいずれかになります。
- *success*: アクション呼び出しは正常に完了しました。
- *application error*: アクション呼び出しは成功しましたが、アクションは、例えば引数の前提条件が満たされなかったなどの理由で、意図的にエラーを返しました。
- *action developer error*: アクションが呼び出されましたが、異常終了しました。例えば、アクションが例外を検出しなかったり、構文エラーが存在したりしました。
- *whisk internal error*: システムはアクションを呼び出すことができませんでした。
結果は、以下のセクションに示されているように、アクティベーション・レコードの `status` フィールドに記録されます。

正常に受信され、ユーザーに課金される可能性のあるすべての呼び出しには、アクティベーション・レコードがあります。

結果が*action developer error* の場合、アクションは部分的に実行され、外部的に可視の副次作用を生成することがあります。 そういった副次作用が起こったかどうかを確認し、必要であれば再試行ロジックを実行するのは、ユーザーの責任です。 一部の *whisk internal errors* は、アクションが実行を開始したが、アクションが完了を記録する前に失敗したことを示します。

## アクティベーション・レコード
{: #openwhisk_ref_activation}

アクション呼び出しおよびトリガー起動のたびに、アクティベーション・レコードができます。

アクティベーション・レコードには、以下のフィールドが含まれています。

- *activationId* : アクティベーション ID。
- *start* および *end* : アクティベーションの開始と終了を記録するタイム・スタンプ。 値は、[UNIX の時刻形式](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15)です。
- *namespace* および `name`: エンティティーの名前空間および名前。
- *logs*: アクションによってアクティベーション中に生成されるログのストリング配列。 各配列エレメントは、アクションによる `stdout` または `stderr` への行出力に対応し、時刻およびログ出力ストリームを含みます。 構造は `TIMESTAMP STREAM: LOG_OUTPUT` です。
- *response*: キー `success`、`status`、および `result` を定義するディクショナリー。
  - *status*: アクティベーション結果。「success (成功)」、「application error (アプリケーション・エラー)」、「action developer error (アクション開発者エラー)」、「whisk internal error (whisk 内部エラー)」のいずれかの値です。
  - *success*: 状況が `success` の場合のみ `true` です。
- *result*: アクティベーション結果を含むディクショナリー。 アクティベーションが成功した場合、result にはアクションによって返された値が入ります。 アクティベーションが成功しなかった場合、`result` には `error` キーが含まれます。これには通常、失敗の説明が伴います。

## JavaScript アクション
{: #openwhisk_ref_javascript}

### 関数プロトタイプ
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript アクションは Node.js ランタイムで実行されます。

JavaScript で記述されたアクションは、単一ファイルに収容しなければなりません。 そのファイルには複数の関数を含めることができますが、規則により `main` という名前の関数が存在しなければならず、これが、アクションを起動したときに呼び出されます。 例えば、以下の例は、複数の関数があるアクションを示しています。
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

アクションの入力パラメーターは、1 つの JSON オブジェクトのパラメーターとして、`main` 関数に渡されます。 成功したアクティベーションの結果も JSON オブジェクトですが、どのように返されるのかは、以下のセクションで説明されているように、アクションが同期か非同期化によって異なります。

### 同期と非同期の動作
{: #openwhisk_ref_javascript_synchasynch}

JavaScript 関数が、戻った後でもコールバック関数で実行を続行することはよくあります。 この動作に対応するため、JavaScript アクションのアクティベーションは、*同期* にすることも*非同期* にすることもできます。

JavaScript アクションのアクティベーションは、以下のいずれかの状態で main 関数が終了する場合は**同期** です。

- main 関数が `return` ステートメントを実行せずに終了する。
- main 関数が Promise *以外* の値を返す `return` ステートメントを実行して終了する。

同期アクションの例を以下に示します。

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

main 関数が Promise を返して終了する場合、JavaScript アクションのアクティベーションは**非同期** です。 この場合、Promise が完了するか、拒否されるまで、システムはアクションがまだ実行中であると見なします。
まず、新規の Promise オブジェクトをインスタンス化して、それをコールバック関数に渡します。 コールバックは、resolve と reject という 2 つの引数を使用します。これらはどちらも関数です。 すべての非同期コードが、そのコールバックに入っていきます。

以下の例は、resolve 関数を呼び出すことによって Promise を完了する方法を示します。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
                }, 100);
             })
}
```
{: codeblock}

次の例は、reject 関数を呼び出すことによって Promise を拒否する方法を示します。
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

以下の例に示すように、入力によって同期であったり非同期であったりするアクションがあり得ます。
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     }  else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}

アクティベーションが同期か非同期かに関係なく、アクションの呼び出しはブロッキングであることも非ブロッキングであることも可能です。

### JavaScript グローバル whisk オブジェクトの削除

グローバル・オブジェクト `whisk` は削除されました。代替方法を使用するように nodejs アクションをマイグレーションしてください。
関数 `whisk.invoke()` および `whisk.trigger()` では、既にインストール済みのクライアント・ライブラリー [openwhisk](https://www.npmjs.com/package/openwhisk) を使用します。
`whisk.getAuthKey()` では、環境変数 `__OW_API_KEY` から API キー値を取得できます。
`whisk.error()` では、拒否された Promise (つまり Promise.reject) を返すことができます。

### JavaScript ランタイム環境
{: #openwhisk_ref_javascript_environments}

JavaScript アクションは、Node.js バージョン 6 または Node.js バージョン 8 で実行できます。
現在、アクションはデフォルトでは Node.js バージョン 6 環境で実行されます。
### アクションと npm パッケージのパッケージ化
Node.js 環境にプリインストールされていない任意の `npm` パッケージを、アクションを作成または更新するときに依存関係としてバンドルすることができます。

詳しくは、[Node.js モジュールとしてのアクションのパッケージ化](./openwhisk_actions.html#openwhisk_js_packaged_action)または[単一バンドルとしてのアクションのパッケージ化](./openwhisk_actions.html#openwhisk_js_webpack_action)を参照してください。

### IBM SDK のある Node.js バージョン 8 環境
{: #openwhisk_ref_javascript_environments_8}
アクションの作成または更新時に `--kind` フラグが明示的に指定され、値が `nodejs:8` である場合、Node.js バージョン 8.11.3 環境が使用されます。

Node.js バージョン 8.11.3 環境では、以下のパッケージがプリインストールされます。
  - [amqplib v0.5.2](https://www.npmjs.com/package/amqplib) - Node.JS 用の AMQP 0-9-1 クライアントを作成するためのライブラリー。
  - [apn v2.2.0](https://www.npmjs.com/package/apn) - Apple Push Notification サービスとのインターフェースのための Node.js モジュール。
  - [async v2.6.1](https://www.npmjs.com/package/async) - 非同期関数での作業のための機能を提供します。
  - [bent v1.1.0](https://www.npmjs.com/package/bent) - Node.js w/ async/await 用の機能 HTTP クライアント。
  - [body-parser v1.18.3](https://www.npmjs.com/package/body-parser) - Node.js 本体構文解析ミドルウェア。 req.body プロパティーで入手可能な着信要求本体をハンドラーの前にミドルウェアで構文解析します。
  - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - ブラウザーの btoa 関数のポート。
  - [cassandra-driver v3.5.0](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra 用の DataStax Node.js ドライバー。
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - これは Node.js 用の公式 Cloudant ライブラリーです。
  - [@cloudant/cloudant v2.3.0](https://www.npmjs.com/package/cloudant) - これは Node.js 用の公式 Cloudant ライブラリーです。
  - [commander v2.15.1](https://www.npmjs.com/package/commander) - node.js コマンド・ライン・インターフェースの完全なソリューション。
  - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Scylla データベース用の Compose UI または API からのアドレス変換機構。
  - [consul v0.33.1](https://www.npmjs.com/package/consul) - サービス・ディスカバリーおよび構成を含む Consul 用のクライアント。
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Cookie ヘッダーを構文解析し、Cookie 名がキーになったオブジェクトを req.cookies に取り込みます。
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - キャッシングを行う高水準な Node.js 用 CouchDB クライアント。
  - [elasticsearch v15.0.0](https://www.npmjs.com/package/elasticsearch) - Node.js 用の公式の低レベル Elasticsearch クライアント。
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - 開発専用エラー・ハンドラー・ミドルウェア。
  - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - Protocol Buffer ベースの etcdv3 API 用の高品質で実動向けのクライアント。
  - [express v4.16.3](https://www.npmjs.com/package/express) - Node 用の高速で軽量でオープンな Web フレームワーク。
  - [express-session v1.15.6](https://www.npmjs.com/package/express-session) - Express アプリ内からのセッション処理を許可します。
  - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - フォーム・データ、特にファイル・アップロードの構文解析のための Node.js モジュール。
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - シェルが使用するパターン (星印など) を使用してファイルのマッチングを行います。
  - [gm v1.23.1](https://www.npmjs.com/package/gm) - Node 用の GraphicsMagick および ImageMagick。
  - [ibm-cos-sdk v1.2.1](https://www.npmjs.com/package/ibm-cos-sdk) - Node.js 用の {{site.data.keyword.cos_full}} SDK。
  - [ibm_db v2.4.0](https://www.npmjs.com/package/ibm_db) - IBM DB2 および IBM Informix への node.js 用の非同期/同期インターフェース。
  - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - IBM Watson IoT プラットフォームとの対話を単純化するために使用される node.js クライアント。
  - [iconv-lite v0.4.23](https://www.npmjs.com/package/iconv-lite) - Pure JS 文字エンコード変換
  - [jsdom v11.10.0](https://www.npmjs.com/package/jsdom) - jsdom は、多くの Web 標準 (特に、WHATWG DOM および HTML 標準) の pure-JavaScript 実装です。
  - [jsonwebtoken v8.3.0](https://www.npmjs.com/package/jsonwebtoken) - JSON Web トークンの実装。
  - [lodash v4.17.10](https://www.npmjs.com/package/lodash) - Node.js モジュールとしてエクスポートされる Lodash ライブラリー。
  - [log4js v2.9.0](https://www.npmjs.com/package/log4js) - これは、Node と連動するための log4js フレームワークの変換です。
  - [marked v0.4.0](https://www.npmjs.com/package/marked) - JavaScript で記述されたフル機能のマークダウン・パーサーおよびコンパイラー。 スピードのために作成。
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - 複数のオブジェクトを 1 つにマージし、オプションで新しい複製オブジェクトを作成します。
  - [moment v2.22.2](https://www.npmjs.com/package/moment) - 日付を構文解析、検証、操作、およびフォーマット設定するための軽量 JavaScript 日付ライブラリー。
  - [mongodb v3.0.10](https://www.npmjs.com/package/mongodb) - Node.js 用の公式 MongoDB ドライバー。
  - [mysql v2.15.0](https://www.npmjs.com/package/mysql) - これは mysql 用の node.js ドライバーです。
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - mustache.js は、JavaScript での mustache テンプレート・システムの実装です。
  - [nano v6.4.4](https://www.npmjs.com/package/nano) - Node.js 用の最小主義の couchdb ドライバー。
  - [nodemailer v4.6.7](https://www.npmjs.com/package/nodemailer) - Node.js からの E メールの送信 – とても簡単です。
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - Node.js で Express を使用して OAuth2 サーバー/プロバイダーを実装するための、完全な、準拠する、十分にテスト済みのモジュール。
  - [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk) - OpenWhisk プラットフォーム用の JavaScript クライアント・ライブラリー。 OpenWhisk API のラッパーを提供します。
  - [path-to-regex v2.2.1](https://www.npmjs.com/package/path-to-regexp) - パス・ストリング (例えば /user/:name) を、URL パスとの突き合わせに使用できる正規表現に変換します。
  - [pg v7.4.3](https://www.npmjs.com/package/pg) - Node.js 用の非ブロッキング PostgreSQL クライアント。 Pure JavaScript およびオプションのネイティブ libpq バインディング。
  - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'); 他のモジュールと同様。
  - [pug v2.0.3](https://www.npmjs.com/package/pug) - Pug テンプレート言語を実装します。
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - これは、完全で機能が豊富な Node.js 用 Redis クライアントです。
  - [request v2.87.0](https://www.npmjs.com/package/request) - request は、HTTP 呼び出しを行う最も単純な方法であるように設計されています。
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - Promise サポートのある単純化された HTTP 要求クライアント 'request'。 Bluebird で稼働。
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - ノード用の UNIX コマンド rm -rf。
  - [semver v5.5.0](https://www.npmjs.com/package/semver) - Nodejs 用のセマンティック・バージョニング
  - [@sendgrid/mail@6.3.1](https://www.npmjs.com/package/@sendgrid/mail) - SendGrid API を介して E メールをサポートします。
  - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - favicon に関するサービスを提供するための Node.js ミドルウェア。
  - [superagent v3.8.3](https://www.npmjs.com/package/superagent) - SuperAgent は小さな漸進的クライアント・サイド HTTP 要求ライブラリーであり、同じ API を持つ Node.js モジュールであり、多くの高水準な HTTP クライアント機能をサポートします。
  - [twilio v3.17.3](https://www.npmjs.com/package/twilio) - 音声、ビデオ、およびメッセージングに関連した、Twilio API 用のラッパー。
  - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - underscore.js は、コア JavaScript オブジェクトをどれも拡張することなく、通常はあると思われる機能 (each、map、reduce、filter 等) をサポートする、JavaScript 用の便利なライブラリーです。
  - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - regex ストリング・マッチャーを使用するよりも簡単に、パス・パラメーターの URL を構文解析します。
  - [uuid v3.2.1](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS の単純で高速な生成。
  - [validator v10.3.0](https://www.npmjs.com/package/validator) - ストリングのバリデーターおよびサニタイザーのライブラリー。
  - [watson-developer-cloud v3.5.0](https://www.npmjs.com/package/watson-developer-cloud) - コグニティブ・コンピューティングを使用して複雑な問題を解決する API の集合である Watson Developer Cloud サービスを使用するための Node.js クライアント・ライブラリー。
  - [when v3.7.8](https://www.npmjs.com/package/when) - when.js は、完全な ES6 Promise shim を含む、厳しい試験を経た強固な Promises/A+ および when() 実装です。
  - [winston v3.0.0](https://www.npmjs.com/package/winston) - node.js 用のマルチ・トランスポート非同期ロギング・ライブラリー。 「落ち着いて! ログに入れたから。」
  - [ws v5.2.0](https://www.npmjs.com/package/ws) - ws は、使用が簡単で高速な、完全にテスト済みの、WebSocket クライアントおよびサーバーの実装です。
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - XML から JavaScript オブジェクトへの単純なコンバーター。 双方向変換をサポートします。
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest は、ブラウザー XMLHttpRequest オブジェクトをエミュレートするための、組み込み http クライアント用のラッパーです。
  - [yauzl v2.9.2](https://www.npmjs.com/package/yauzl) - ノード用の別の unzip ライブラリー。

### Node.js バージョン 6 環境
{: #openwhisk_ref_javascript_environments_6}
アクションの作成または更新時に `--kind` フラグが明示的に指定され、値が `nodejs:6` である場合、Node.js 6.14.3 環境が使用されます。

Node.js 6.14.3 環境では以下のパッケージが使用可能です。

- [apn v2.1.2](https://www.npmjs.com/package/apn) - Apple Push Notification サービスとのインターフェースのための Node.js モジュール。
- [async v2.1.4](https://www.npmjs.com/package/async) - 非同期関数での作業のための機能を提供します。
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - ブラウザーの btoa 関数のポート。
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - サーバー専用に設計されたコア jQuery の高速で柔軟かつ効率的な実装。
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - Node.js 用の公式 Cloudant ライブラリー。
- [commander v2.9.0](https://www.npmjs.com/package/commander) - Node.js コマンド・ライン・インターフェースの完全なソリューション。
- [consul v0.27.0](https://www.npmjs.com/package/consul) - サービス・ディスカバリーおよび構成を含む Consul 用のクライアント。
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Cookie ヘッダーを構文解析し、Cookie 名がキーになったオブジェクトを req.cookies に取り込みます。
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - キャッシングを行う高水準な Node.js 用 CouchDB クライアント。
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - 開発専用エラー・ハンドラー・ミドルウェア。
- [glob v7.1.1](https://www.npmjs.com/package/glob) - シェルが使用するパターン (星印など) を使用してファイルのマッチングを行います。
- [gm v1.23.0](https://www.npmjs.com/package/gm) - Node 用の GraphicsMagick および ImageMagick。
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - Node.js モジュールとしてエクスポートされる Lodash ライブラリー。
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - Node と共に機能するように設計された log4js フレームワークの変換。
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - Pure JS 文字エンコード変換
- [marked v0.3.6](https://www.npmjs.com/package/marked) - JavaScript で記述されたフル機能のマークダウン・パーサーおよびコンパイラー。 スピードのために作成。
- [merge v1.2.0](https://www.npmjs.com/package/merge) - 複数のオブジェクトを 1 つにマージして、新しい複製オブジェクトを作成します。
- [moment v2.17.0](https://www.npmjs.com/package/moment) - 日付を構文解析、検証、操作、およびフォーマット設定するための軽量 JavaScript 日付ライブラリー。
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - Node.js 用の公式 MongoDB ドライバー。
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js は、JavaScript での mustache テンプレート・システムの実装です。
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Node.js 用の最小主義の couchdb ドライバー。
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - 非推奨 UUID パッケージ。
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - Node.js からの E メールの送信 – とても簡単です。
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - Node.js で Express を使用して OAuth2 サーバー/プロバイダーを実装するための、完全な、準拠する、十分にテスト済みのモジュール。
- [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk) - OpenWhisk プラットフォーム用の JavaScript クライアント・ライブラリー。 OpenWhisk API のラッパーを提供します。
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud は、複数のクラウド・プロバイダー間の相違を取り除く、Node.js 用の標準ライブラリーです。
- [process v0.11.9](https://www.npmjs.com/package/process) - require('process'); 他のモジュールと同様。
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Pug テンプレート言語を実装します。
- [redis v2.6.3](https://www.npmjs.com/package/redis) - 完全で機能が豊富な Node.js 用 Redis クライアント。
- [request v2.79.0](https://www.npmjs.com/package/request) - 要求は HTTP 呼び出しを行う最も単純な方法です。
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - Promise サポートのある単純化された HTTP 要求クライアント 'request'。 Bluebird で稼働。
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - ノード用の UNIX コマンド rm -rf。
- [semver v5.3.0](https://www.npmjs.com/package/semver) - セマンティック・バージョン管理をサポートします。
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - SendGrid API を使用した E メールのサポートを提供します。
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - favicon に関するサービスを提供するための Node.js ミドルウェア。
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO は、リアルタイムで双方向のイベント・ベースの通信を可能にします。
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Socket.IO のクライアント・サイドのサポート。
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent は小さな漸進的クライアント・サイド HTTP 要求ライブラリーであり、同じ API を持つ Node.js モジュールであり、多くの高水準な HTTP クライアント機能をサポートします。
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - API を文書化する方法である、Swagger で機能する関連したツール群。
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - Node.js 用の単純な一時ファイルおよびディレクトリーの作成機能。
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - 音声、ビデオ、およびメッセージングに関連した、Twilio API 用のラッパー。
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js は、コア JavaScript オブジェクトをどれも拡張することなく、通常はあると思われる機能 (each、map、reduce、filter 等) をサポートする、JavaScript 用のユーティリティー・ベルト・ライブラリーです。
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS の単純で高速な生成。
- [validator v6.1.0](https://www.npmjs.com/package/validator) - ストリングのバリデーターおよびサニタイザーのライブラリー。
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - コグニティブ・コンピューティングを使用して複雑な問題を解決する API の集合である Watson Developer Cloud サービスを使用するための Node.js クライアント・ライブラリー。
- [when v3.7.7](https://www.npmjs.com/package/when) - when.js は、完全な ES6 Promise shim を含む、厳しい試験を経た強固な Promises/A+ および when() 実装です。
- [winston v2.3.0](https://www.npmjs.com/package/winston) - node.js 用のマルチ・トランスポート非同期ロギング・ライブラリー。 「落ち着いて! ログに入れたから。」
- [ws v1.1.1](https://www.npmjs.com/package/ws) - ws は、使用が簡単で高速な、完全にテスト済みの、WebSocket クライアントおよびサーバーの実装です。
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - XML から JavaScript オブジェクトへの単純なコンバーター。 双方向変換をサポートします。
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest は、ブラウザー XMLHttpRequest オブジェクトをエミュレートするための、組み込み http クライアント用のラッパーです。
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - ノード用の別の unzip ライブラリー。


## Python ランタイム環境
{: #openwhisk_ref_python_environments}

OpenWhisk は、異なる 2 つのランタイム・バージョンを使用した Python アクションの実行をサポートします。

### Python 3 アクション (Jessie ベース)
{: #openwhisk_ref_python_environments_jessie}

Python 3 アクションは Python 3.6.5 で実行されます。 このランタイムを使用するには、アクションを作成または更新するときに `wsk` CLI パラメーター `--kind python-jessie:3` を指定します。
virtualenv を使用して python アクションを作成する場合は、Docker イメージ `ibmfunctions/action-python-v3` を使用します。
このランタイムには、Python 3.6 標準ライブラリーに加えて Python アクションで使用できる IBM Cloud サービス用 SDK パッケージが含まれています。

Python バージョン:
- [3.6.5](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Python パッケージ:
- asn1crypto==0.24.0
- attrs==17.4.0
- Automat==0.6.0
- beautifulsoup4==4.6.0
- botocore==1.9.4
- cassandra-driver==3.14.0
- certifi==2018.1.18
- cffi==1.11.5
- chardet==3.0.4
- click==6.7
- cloudant==2.9.0
- constantly==15.1.0
- cryptography==2.1.4
- cssselect==1.0.3
- docutils==0.14
- elasticsearch==6.2.0
- Flask==1.0.2
- gevent==1.2.2
- greenlet==0.4.13
- httplib2==0.11.3
- hyperlink==18.0.0
- ibm-cos-sdk==2.1.1
- ibm-cos-sdk-core==2.1.1
- ibm-cos-sdk-s3transfer==2.1.1
- ibm-db==2.0.8a0
- ibmcloudsql==0.2.13
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.10
- jmespath==0.9.3
- kafka-python==1.4.3
- lxml==4.2.1
- MarkupSafe==1.0
- numpy==1.14.5
- pandas==0.23.1
- parsel==1.4.0
- pika==0.12.0
- Pillow==5.1.0
- psycopg2==2.7.5
- pyasn1==0.4.2
- pyasn1-modules==0.2.1
- pycparser==2.18
- PyDispatcher==2.0.5
- pymongo==3.6.1
- pyOpenSSL==17.5.0
- pysolr==3.7.0
- python-dateutil==2.7.3
- pytz==2018.3
- queuelib==1.4.2
- redis==2.10.6
- requests==2.19.1
- scikit-learn==0.19.1
- scipy==1.1.0
- Scrapy==1.5.0
- service-identity==17.0.0
- simplejson==3.15.0
- six==1.11.0
- tornado==5.0.2
- Twisted==18.4.0
- urllib3==1.22
- virtualenv==16.0.0
- w3lib==1.19.0
- watson-developer-cloud==1.4.0
- Werkzeug==0.14.1
- zope.interface==4.4.3

### Python 3 アクション (Alpine ベース)
{: #openwhisk_ref_python_environments_alpine}

Python 3 アクションは Python 3.6.1 で実行されます。 このランタイムを使用するには、アクションを作成または更新するときに `wsk` CLI パラメーター `--kind python:3` を指定します。
virtualenv を使用して python アクションを作成する場合は、Docker イメージ `openwhisk/python3action` を使用します。
Python 3.6 標準ライブラリーに加えて、以下のパッケージが Python アクションで使用できます。

Python パッケージ:
- asn1crypto==0.23.0
- attrs==17.3.0
- Automat==0.6.0
- beautifulsoup4==4.5.3
- cffi==1.11.2
- click==6.7
- constantly==15.1.0
- cryptography==2.1.3
- cssselect==1.0.1
- Flask==0.12
- gevent==1.2.1
- greenlet==0.4.12
- httplib2==0.10.3
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.4
- lxml==3.7.3
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.5
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.6.0
- queuelib==1.4.2
- requests==2.13.0
- Scrapy==1.3.3
- service-identity==17.0.0
- simplejson==3.10.0
- six==1.11.0
- Twisted==17.1.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

### Python 2 アクション

アクションを作成または更新するときに `--kind` フラグを指定しない場合、Python 2 アクションは、Python アクションのデフォルトのランタイムである Python 2.7.12 で実行されます。 このランタイムを明示的に選択するには、`--kind python:2` を使用します。
virtualenv を使用して python アクションを作成する場合は、Docker イメージ `openwhisk/python2action` を使用します。
Python 2.7 標準ライブラリーに加えて、以下のパッケージが Python 2 アクションで使用できます。

Python パッケージ:
- asn1crypto==0.23.0
- attrs==17.2.0
- beautifulsoup4==4.5.1
- cffi==1.11.1
- click==6.7
- cryptography==2.0.3
- cssselect==1.0.1
- enum34==1.1.6
- Flask==0.11.1
- gevent==1.1.2
- greenlet==0.4.12
- httplib2==0.9.2
- idna==2.6
- ipaddress==1.0.18
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.1
- lxml==3.6.4
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.4
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.5.3
- queuelib==1.4.2
- requests==2.11.1
- Scrapy==1.1.2
- service-identity==17.0.0
- simplejson==3.8.2
- six==1.11.0
- Twisted==16.4.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

## Swift アクション
{: #swift-actions}

### Swift 3
Swift 3 アクションは Swift 3.1.1 `--kind swift:3.1.1` で実行されます。 前のバージョンの Swift はサポートされないため、常に kind `swift:3.1.1` を指定してください。

kind `swift:3.1.1` を使用するようにすべての Swift アクションをマイグレーションする必要があります。 ベスト・プラクティスとして、アクションを作成または更新するときは常に特定の kind を指定してください。
{: tip}

単一の Swift ソース・ファイルを使用する場合、Swift 3.1.1 アクションは以下のパッケージを使用できます。
- KituraNet バージョン 1.7.6、https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON バージョン 15.0.1、https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK バージョン 0.16.0、https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 アクションは、Swift 4.1 `--kind swift:4.1` を使用して実行されます。

Package.swift を使用して依存関係を組み込むには、[パッケージされた swift アクション](./openwhisk_actions.html#packaging-an-action-as-a-swift-executable)の指示に従ってください。

単一の Swift ソース・ファイルを使用する場合、Swift 4.1 アクションは以下のパッケージを使用できます。
- Watson Developer Cloud SDK バージョン 0.28.0、https://github.com/watson-developer-cloud/swift-sdk

### Swift 3.1.1 から Swift 4.1 へのマイグレーション

#### 単一のソース・アクション・ファイルを使用する SwiftyJSON
コンパイルされていない `swift:3.1.1` アクションがある場合、**SwiftyJSON** パッケージを使用するソース・ファイルと同様に、アクションをプリコンパイルし、`swift:4.1` kind アクションに使用したい SwiftyJSON のバージョンを指定する必要があります。 Swift 4.1 以降、JSON データをネイティブに管理するためのより良いサポートがあることを考慮に入れてください。

## PHP アクション
{: #openwhisk_ref_php}

PHP アクションは PHP 7.1.18 で実行されます。このランタイムを使用するには、アクションを作成または更新するときに `wsk` CLI パラメーター `--kind php:7.1` を指定します。 この動作は、`.php` 拡張子を持つファイルでアクションを作成する際のデフォルトです。

標準の PHP 拡張子に加えて、以下の PHP 拡張子が使用可能です。

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### コンポーザー・パッケージ
以下のコンポーザー・パッケージも使用可能です。

- guzzlehttp/guzzle       v6.7.3
- ramsey/uuid             v3.7.3

## Docker アクション
{: #openwhisk_ref_docker}

Docker アクションは、Docker コンテナー内のユーザー提供バイナリーを実行します。 バイナリーは、[python:2.7.12-alpine](https://hub.docker.com/r/library/python) に基づく Docker イメージで実行されるため、バイナリーはこのディストリビューションと互換でなければなりません。

Docker スケルトンは、OpenWhisk 互換の Docker イメージをビルドするための便利な方法です。 `ibmcloud wsk sdk install docker` CLI プラグイン・コマンドでスケルトンをインストールできます。

メイン・バイナリー・プログラムはコンテナー内部の `/action/exec` に置かれる必要があります。 実行可能バイナリーは、`JSON` オブジェクトとしてデシリアライズ可能な単一コマンド・ライン引数ストリングから、入力引数を受け取ります。 また、シリアライズされた `JSON` の単一行ストリングとして `stdout` を使用して、結果を返す必要があります。

`dockerSkeleton` 内に含まれている `Dockerfile` を変更して、コンパイル・ステップや依存関係を組み込むことができます。

## REST API
{: #openwhisk_ref_restapi}
{{site.data.keyword.openwhisk_short}} REST API に関する情報は [REST API リファレンス](https://console.bluemix.net/apidocs/functions)にあります。

## システム限度
{: #openwhisk_syslimits}

### アクション
{{site.data.keyword.openwhisk_short}} には、1 つのアクションが使用できるメモリー量や、1 分当たりに許可されるアクション呼び出し回数など、いくつかのシステム限度があります。

次の表に、アクションのデフォルト限度を示します。

| 限度 | 説明 | デフォルト | 最小 | 最大 |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | アクション・コードの最大サイズ (MB)。 | 48 | 1 | 48 |
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) | 実行中または実行用にキューに入れられる、サブミットできるアクティベーションは名前空間当たり N 個までです。 | 1000 | 1 | 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | 1 つのコンテナーが N MB を超えて stdout に書き込むことは許可されません。 | 10 | 0 | 10 |
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory) | 1 つのコンテナーが N MB を超えるメモリーを割り振ることは許可されません。 | 256 | 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | 分当たり、名前空間当たりにサブミットできるアクティベーションは N 個までです。 | 5000 | 1 | 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | アクション当たりのオープン・ファイルの最大数。 | 1024 | 0 | 1024 |
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) | 付加できるパラメーターの最大サイズ (MB)。 | 1 | 0 | 1 |
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | 1 つのアクションに使用可能なプロセスの最大数。 | 1024 | 0 | 1024 |
| [result](openwhisk_reference.html#openwhisk_syslimits_result) | アクション呼び出し結果の最大サイズ (MB)。 | 1 | 0 | 1 |
| [sequenceMaxActions](openwhisk_reference.html#openwhisk_syslimits_sequencemax) | 1 つのシーケンスを構成するアクションの最大数。 | 50 | 0 | 50* |
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout) | 1 つのコンテナーが N ミリ秒より長く実行することは許可されません。 | 60000 | 100 | 600000 |

### 固定限度の引き上げ
{: #increase_fixed_limit}

末尾に (*) が付いている限度値は固定ですが、値を大きくしても安全であるとビジネス・ケースで判断できる場合は増やすことができます。 限度値を増やしたい場合、IBM [{{site.data.keyword.openwhisk_short}} Web コンソール](https://console.bluemix.net/openwhisk/)からチケットを直接オープンすることによって、IBM サポートに連絡してください。
  1. **「サポート」**を選択します。
  2. ドロップダウン・メニューから**「チケットの追加」**を選択します。
  3. チケット・タイプに**「技術的」**を選択します。
  4. サポートの技術的領域に**「機能」**を選択します。

#### codeSize (MB) (固定: 48 MB)
{: #openwhisk_syslimits_codesize}
* 　アクションの最大コード・サイズは 48 MB です。
* JavaScript アクションの場合は、ツールを使用して、依存関係を含むすべてのソース・コードを連結して単一のバンドルされたファイルにすることが推奨されます。
* この限度は固定されており、変更することはできません。

#### concurrent (固定: 1000*)
{: #openwhisk_syslimits_concurrent}
* 実行中または実行用にキューに入れられるアクティベーションの数は名前空間当たり 1000 個を超えることはできません。
* この限度値は固定ですが、値を大きくしても安全であるとビジネス・ケースで判断できる場合は増やすことができます。 この限度を増やす方法について詳しくは、『[固定限度の引き上げ](openwhisk_reference.html#increase_fixed_limit)』セクションを参照してください。

#### logs (MB) (デフォルト: 10 MB)
{: #openwhisk_syslimits_logs}
* ログ限度 N は [0 MB..10 MB] の範囲内であり、アクション当たりで設定されます。
* ユーザーは、アクションが作成または更新されるときにアクション・ログ限度を変更できます。
* 設定された限度を超えるログは切り捨てられるため、新しいログ項目は無視されます。また、設定されたログ限度をアクティベーションが超えたことを示す警告がアクティベーションの最終出力として追加されます。

#### memory (MB) (デフォルト: 256 MB)
{: #openwhisk_syslimits_memory}
* メモリー限度 M は [128 MB..512 MB] の範囲内であり、アクション当たりの MB 数で設定されます。
* ユーザーは、アクションが作成されるときにメモリー限度を変更できます。
* コンテナーは、限度によって割り振られた量を超えるメモリーを使用することはできません。

#### minuteRate (固定: 5000*)
{: #openwhisk_syslimits_minuterate}
* レート限度 N は 5000 に設定され、1 分の枠内のアクション呼び出し数を制限します。
* この限度を超える CLI 呼び出しまたは API 呼び出しは、HTTP 状況コード `429: TOO MANY REQUESTS` に対応するエラー・コードを受け取ります。
* この限度値は固定ですが、値を大きくしても安全であるとビジネス・ケースで判断できる場合は増やすことができます。 この限度を増やす方法について詳しくは、『[固定限度の引き上げ](openwhisk_reference.html#increase_fixed_limit)』セクションを参照してください。

#### openulimit (固定: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* アクション当たりのオープン・ファイルの最大数は 1024 です (ハード制限とソフト制限の両方)。
* この限度は固定されており、変更することはできません。
* アクションが呼び出されると、docker run コマンドは、引数 `--ulimit nofile=1024:1024` を使用して `openulimit` 値を設定します。
* 詳しくは、[docker run](https://docs.docker.com/engine/reference/commandline/run) コマンド・ライン解説書を参照してください。

#### parameters (固定: 1 MB)
{: #openwhisk_syslimits_parameters}
* アクション/パッケージ/トリガーの作成または更新の全パラメーターのサイズ限度は 1 MB です。
* パラメーターが大き過ぎるエンティティーを作成または更新しようとすると拒否されます。
* この限度は固定されており、変更することはできません。

#### proculimit (固定: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* アクション・コンテナーに使用可能なプロセスの最大数は 1024 です。
* この限度は固定されており、変更することはできません。
* アクションが呼び出されると、docker run コマンドは、引数 `--pids-limit 1024` を使用して `proculimit` 値を設定します。
* 詳しくは、[docker run](https://docs.docker.com/engine/reference/commandline/run) コマンド・ライン解説書を参照してください。

#### result (固定: 1 MB)
{: #openwhisk_syslimits_result}
* アクション呼び出し結果の最大出力サイズ (MB)。
* この限度は固定されており、変更することはできません。

#### sequenceMaxActions (固定: 50*)
{: #openwhisk_syslimits_sequencemax}
* 1 つのシーケンスを構成するアクションの最大数。
* この限度は固定されており、変更することはできません。

#### timeout (ms) (デフォルト: 60s)
{: #openwhisk_syslimits_timeout}
* タイムアウト限度 N は [100 ms..600000 ms] の範囲内であり、アクション当たりのミリ秒で設定されます。
* ユーザーは、アクションが作成されるときにタイムアウト限度を変更できます。
* N ミリ秒を超えて実行したコンテナーは、終了されます。

### トリガー

次の表に示すように、トリガーには分当たりの起動レートの制限が課されます。

| 限度 | 説明 | デフォルト | 最小 | 最大 |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | 分当たり、名前空間当たりに起動できるトリガーは N 個までです。 | 5000* | 5000* | 5000* |

### 固定限度の引き上げ
{: #increase_fixed_tlimit}

末尾に (*) が付いている限度値は固定ですが、値を大きくしても安全であるとビジネス・ケースで判断できる場合は増やすことができます。 限度値を増やしたい場合、IBM [{{site.data.keyword.openwhisk_short}} Web コンソール](https://console.bluemix.net/openwhisk/)からチケットを直接オープンすることによって、IBM サポートに連絡してください。
  1. **「サポート」**を選択します。
  2. ドロップダウン・メニューから**「チケットの追加」**を選択します。
  3. チケット・タイプに**「技術的」**を選択します。
  4. サポートの技術的領域に**「機能」**を選択します。

#### minuteRate (固定: 5000*)
{: #openwhisk_syslimits_tminuterate}

* レート限度 N は 5000 に設定され、1 分の枠内にユーザーが起動できるトリガーの数を制限します。
* ユーザーはトリガーが作成されるときにトリガー限度を変更することはできません。
* この限度を超える CLI 呼び出しまたは API 呼び出しは、HTTP 状況コード `429: TOO MANY REQUESTS` に対応するエラー・コードを受け取ります。
* この限度値は固定ですが、値を大きくしても安全であるとビジネス・ケースで判断できる場合は増やすことができます。 この限度を増やす方法について詳しくは、『[固定限度の引き上げ](openwhisk_reference.html#increase_fixed_tlimit)』セクションを参照してください。

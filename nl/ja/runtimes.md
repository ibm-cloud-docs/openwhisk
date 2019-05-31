---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

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

# ランタイム
{: #runtimes}
アプリは、Javascript や Python などのプログラミング言語でコーディングして実行できます。{{site.data.keyword.openwhisk_short}} では、デフォルトで多くのランタイムを使用できます。
{: shortdesc}

IBM Cloud Functions で使用できるランタイムを地域別に確認してください。次のリンクは JSON 応答を返します。

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

応答の `runtimes` セクションには、使用可能なランタイムのセットが含まれています。

`image` セクションには、[Docker Hub](https://hub.docker.com/) 上のランタイム・イメージの名前と、使用されているタグが含まれています。


以下の例は、イメージ `ibmfunctions/action-nodejs-v10` および `openwhisk/nodejs8action` を指し示しています。
タグは、`1.9.0` のようなバージョン番号、または `b99d71e` のような、Git コミット・ハッシュの簡易書式とすることができます。

イメージ・フィールドの例。
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

ランタイムは、定期的に更新されます。これらの更新には、セキュリティー・フィックスやランタイム内のパッケージに対するマイナー・バージョン・アップデートなどがあります。 マイナー・バージョン・アップデートにより後方互換性が中断されることがあります。 ランタイムの更新によりアクションが影響を受けることがあります。 ランタイムを実行するアクションを、更新して新しいバージョンに移行する必要があります。

非推奨のランタイムで実行されるアプリは、ランタイムがサポート対象のものに更新されるまで、正常に実行できません。失敗したアクションのトラブルシューティングをするとき、ランタイムが非推奨のものかどうかを見分けるには、照会応答に `deprecated=true` があるかどうかを調べます。 ランタイムを更新する方法については、[アクション・ランタイムの変更](/docs/openwhisk?topic=cloud-functions-actions#actions_update)を参照してください

以下のランタイムは非推奨です。
<ul>
  <li><code>nodejs:6</code> (非推奨)</li>
  <li><code>php:7.1</code> (非推奨)</li>
  <li><code>php:7.2</code> (非推奨)</li>
  <li><code>swift:3</code> (非推奨)</li>
  <li><code>swift:3.1.1</code> (非推奨)</li>
  <li><code>swift:4.1</code> (非推奨)</li>
  <li><code>ballerina:0.990</code> (非推奨)</li>
</ul>






## JavaScript ランタイム
{: #openwhisk_ref_javascript_environments}

JavaScript アクションは、Node.js バージョン 8 または 10 で実行できます。 デフォルトでは、すべての Node.js アクションはバージョン 10 環境で実行されます。



### IBM SDK のある Node.js バージョン 10 環境
{: #openwhisk_ref_javascript_environments_10}
アクションの作成または更新時に `--kind` フラグが明示的に指定され、値が `nodejs:10` である場合、Node.js バージョン 10 環境が使用されます。

#### `nodejs:8` から `nodejs:10` へのマイグレーション
- `ibm_db` npm パッケージは、`nodejs:10` では使用できません。 `ibm_db` パッケージは Node.js 10 をサポートしていません。 進捗は記事 [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541) で追跡できます。
- `cloudant` npm パッケージは `nodejs:10` では使用できません。このパッケージは非推奨です。nodejs モジュールをインポートするときは、公式の npm パッケージ [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 を使用する必要があります (つまり `require('@cloudant/cloudant')`)。また、[v3.x だけが Promises を返します](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)。
- `cradle` npm パッケージは、`nodejs:10` では使用できません。
- `log4js` npm パッケージは、`nodejs:10` では使用できません。 この問題は [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805) で追跡できます
- `watson-developer-cloud` npm パッケージは、`nodejs:10` では使用できません。 新しいバージョンに関する進捗は、記事 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780) で追跡できます。

nodejs バージョン 10 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md) を参照してください。

#### Node.js 10.15 パッケージ

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 NPM パッケージ:
   - [amqplib](https://www.npmjs.com/package/amqplib) - Node.JS 用の AMQP 0-9-1 クライアントを作成するためのライブラリー。
   - [apn](https://www.npmjs.com/package/apn) - Apple Push Notification サービスとのインターフェースのための Node.js モジュール。
   - [async](https://www.npmjs.com/package/async) - 非同期関数での作業のための機能を提供します。
   - [bent](https://www.npmjs.com/package/bent) - async および await を使用する Node.js 用の機能 HTTP クライアント。
   - [bodyparser](https://www.npmjs.com/package/body-parser) - req.body プロパティーで入手可能な着信要求本体をハンドラーの前にミドルウェアで構文解析します。
   - [btoa](https://www.npmjs.com/package/btoa) - ブラウザーの btoa 関数のポート。
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra 用の DataStax Node.js ドライバー。
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) - これは Node.js 用の公式 Cloudant ライブラリーです。
   - [commander](https://www.npmjs.com/package/commander) - node.js コマンド・ライン・インターフェースの完全なソリューション。
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Scylla データベース用の Compose UI または API からのアドレス変換機構。
   - [consul](https://www.npmjs.com/package/consul) - サービス・ディスカバリーおよび構成を含む Consul 用のクライアント。
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Cookie ヘッダーを構文解析し、Cookie 名がキーになったオブジェクトを req.cookies に取り込みます。
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Node.js 用の公式の低レベル Elasticsearch クライアント。
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - 開発専用エラー・ハンドラー・ミドルウェア。
   - [etcd3](https://www.npmjs.com/package/etcd3) - Protocol Buffer ベースの etcdv3 API 用の高品質で実動向けのクライアント。
   - [formidable](https://www.npmjs.com/package/formidable) - フォーム・データ、特にファイル・アップロードの構文解析のための Node.js モジュール。
   - [glob](https://www.npmjs.com/package/glob) - シェルが使用するパターン (星印など) を使用してファイルのマッチングを行います。
   - [gm](https://www.npmjs.com/package/gm) - Node 用の GraphicsMagick および ImageMagick。
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - Node.js 用の {{site.data.keyword.cos_full}} SDK。
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - IBM DB2 および IBM Informix への node.js 用の非同期/同期インターフェース。
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - IBM Watson IoT プラットフォームとの対話を単純化するために使用される node.js クライアント。
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Pure JS 文字エンコード変換
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom は、多くの Web 標準 (特に、WHATWG DOM および HTML 標準) の pure-JavaScript 実装です。
   - [jsforce](https://www.npmjs.com/package/jsforce) - JavaScript アプリケーションの Salesforce API ライブラリー。
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JSON Web トークンの実装。
   - [lodash](https://www.npmjs.com/package/lodash) - Node.js モジュールとしてエクスポートされる Lodash ライブラリー。
   - [marked](https://www.npmjs.com/package/marked) - JavaScript で記述されたフル機能のマークダウン・パーサーおよびコンパイラー。 スピードのために作成。
   - [merge](https://www.npmjs.com/package/merge) - 複数のオブジェクトを 1 つにマージし、オプションで新しい複製オブジェクトを作成します。
   - [moment](https://www.npmjs.com/package/moment) - 日付を構文解析、検証、操作、およびフォーマット設定するための軽量 JavaScript 日付ライブラリー。
   - [mongodb](https://www.npmjs.com/package/mongodb) - Node.js 用の公式 MongoDB ドライバー。
   - [mysql](https://www.npmjs.com/package/mysql) - これは mysql 用の node.js ドライバーです。
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js は、JavaScript での mustache テンプレート・システムの実装です。
   - [nano](https://www.npmjs.com/package/nano) - Node.js 用の最小主義の couchdb ドライバー。
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Node.js からの E メールの送信 - とても簡単です。
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Node.js で Express を使用して OAuth2 サーバー/プロバイダーを実装するための、完全な、準拠する、十分にテスト済みのモジュール。
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - OpenWhisk プラットフォーム用の JavaScript クライアント・ライブラリー。 OpenWhisk API のラッパーを提供します。
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - パス・ストリング (例えば /user/:name) を、URL パスとの突き合わせに使用できる正規表現に変換します。
   - [pg](https://www.npmjs.com/package/pg) - Node.js 用の非ブロッキング PostgreSQL クライアント。 Pure JavaScript およびオプションのネイティブ libpq バインディング。
   - [process](https://www.npmjs.com/package/process) - require('process'); 他のモジュールと同様。
   - [pug](https://www.npmjs.com/package/pug) - Pug テンプレート言語を実装します。
   - [redis](https://www.npmjs.com/package/redis) - これは、完全で機能が豊富な Node.js 用 Redis クライアントです。
   - [request](https://www.npmjs.com/package/request) - request は、HTTP 呼び出しを行う最も単純な方法であるように設計されています。
   - [request-promise](https://www.npmjs.com/package/request-promise) - Promise サポートのある単純化された HTTP 要求クライアント 'request'。 Bluebird で稼働。
   - [rimraf](https://www.npmjs.com/package/rimraf) - ノード用の UNIX コマンド rm -rf。
   - [semver](https://www.npmjs.com/package/semver) - Nodejs 用のセマンティック・バージョニング
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - SendGrid API を介して E メールをサポートします。
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - エラーをプレーン・オブジェクトとしてシリアライズします。
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - favicon に関するサービスを提供するための Node.js ミドルウェア。
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO は、リアルタイムで双方向のイベント・ベースの通信を可能にします。
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - socket.io のリアルタイム・アプリケーション・フレームワーク。
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent は小さな漸進的クライアント・サイド HTTP 要求ライブラリーであり、同じ API を持つ Node.js モジュールであり、多くの高水準な HTTP クライアント機能をサポートします。
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Swagger と統合および対話するための各種ツールを提供するパッケージ。
   - [twilio](https://www.npmjs.com/package/twilio) - 音声、ビデオ、およびメッセージングに関連した、Twilio API 用のラッパー。
   - [underscore](https://www.npmjs.com/package/underscore) - underscore.js は、コア JavaScript オブジェクトをどれも拡張することなく、通常はあると思われる機能 (each、map、reduce、filter 等) をサポートする、JavaScript 用の便利なライブラリーです。
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - regex ストリング・マッチャーを使用するよりも簡単に、パス・パラメーターの URL を構文解析します。
   - [uuid](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS の単純で高速な生成。
   - [validator](https://www.npmjs.com/package/validator) - ストリングのバリデーターおよびサニタイザーのライブラリー。
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - IBM Cloud が提供する VCAP_SERVICES 環境変数のサービス資格情報を解析して返します。
   - [when](https://www.npmjs.com/package/when) - when.js は、完全な ES6 Promise shim を含む、厳しい試験を経た強固な Promises/A+ および when() 実装です。
   - [winston](https://www.npmjs.com/package/winston) - node.js 用のマルチ・トランスポート非同期ロギング・ライブラリー。 「落ち着いて! ログに入れたから。」
   - [ws](https://www.npmjs.com/package/ws) - ws は、使用が簡単で高速な、完全にテスト済みの、WebSocket クライアントおよびサーバーの実装です。
   - [xlsx](https://www.npmjs.com/package/xlsx) - 各種スプレッドシート形式のパーサーおよびライター。
   - [xml2js](https://www.npmjs.com/package/xml2js) - XML から JavaScript オブジェクトへの単純なコンバーター。 双方向変換をサポートします。
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest は、ブラウザー XMLHttpRequest オブジェクトをエミュレートするための、組み込み http クライアント用のラッパーです。
   - [yauzl](https://www.npmjs.com/package/yauzl) - ノード用の別の抽出ライブラリー。

### IBM SDK のある Node.js バージョン 8 環境
{: #openwhisk_ref_javascript_environments_8}
アクションの作成または更新時に `--kind` フラグが明示的に指定され、値が `nodejs:8` である場合、Node.js バージョン 8 環境が使用されます。

Node.js バージョン 8 は保守モードになっていて、2019 年 12 月まで使用可能です。 [Node.js のリリース・スケジュール](https://github.com/nodejs/Release)を参照してください。
{: deprecated}

#### Node.js 8.15 パッケージ

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib) - Node.JS 用の AMQP 0-9-1 クライアントを作成するためのライブラリー。
   - [apn](https://www.npmjs.com/package/apn) - Apple Push Notification サービスとのインターフェースのための Node.js モジュール。
   - [async](https://www.npmjs.com/package/async) - 非同期関数での作業のための機能を提供します。
   - [bent](https://www.npmjs.com/package/bent) - Node.js w/ async/await 用の機能 HTTP クライアント。
   - [bodyparser](https://www.npmjs.com/package/body-parser) - req.body プロパティーで入手可能な着信要求本体をハンドラーの前にミドルウェアで構文解析します。
   - [btoa](https://www.npmjs.com/package/btoa) - ブラウザーの btoa 関数のポート。
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - Apache Cassandra 用の DataStax Node.js ドライバー。
   - [cloudant](https://www.npmjs.com/package/cloudant) - これは Node.js 用の公式 Cloudant ライブラリーです。
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant) - これは Node.js 用の公式 Cloudant ライブラリーです。
   - [commander](https://www.npmjs.com/package/commander) - node.js コマンド・ライン・インターフェースの完全なソリューション。
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Scylla データベース用の Compose UI または API からのアドレス変換機構。
   - [consul](https://www.npmjs.com/package/consul) - サービス・ディスカバリーおよび構成を含む Consul 用のクライアント。
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Cookie ヘッダーを構文解析し、Cookie 名がキーになったオブジェクトを req.cookies に取り込みます。
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - Node.js 用の公式の低レベル Elasticsearch クライアント。
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - 開発専用エラー・ハンドラー・ミドルウェア。
   - [etcd3](https://www.npmjs.com/package/etcd3) - Protocol Buffer ベースの etcdv3 API 用の高品質で実動向けのクライアント。
   - [formidable](https://www.npmjs.com/package/formidable) - フォーム・データ、特にファイル・アップロードの構文解析のための Node.js モジュール。
   - [glob](https://www.npmjs.com/package/glob) - シェルが使用するパターン (星印など) を使用してファイルのマッチングを行います。
   - [gm](https://www.npmjs.com/package/gm) - Node 用の GraphicsMagick および ImageMagick。
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - Node.js 用の {{site.data.keyword.cos_full}} SDK。
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - IBM DB2 および IBM Informix への node.js 用の非同期/同期インターフェース。
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - IBM Watson IoT プラットフォームとの対話を単純化するために使用される node.js クライアント。
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Pure JS 文字エンコード変換
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom は、多くの Web 標準 (特に、WHATWG DOM および HTML 標準) の pure-JavaScript 実装です。
   - [jsforce](https://www.npmjs.com/package/jsforce) - JavaScript アプリケーションの Salesforce API ライブラリー。
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JSON Web トークンの実装。
   - [lodash](https://www.npmjs.com/package/lodash) - Node.js モジュールとしてエクスポートされる Lodash ライブラリー。
   - [log4js](https://www.npmjs.com/package/log4js) - これは、Node と連動するための log4js フレームワークの変換です。
   - [marked](https://www.npmjs.com/package/marked) - JavaScript で記述されたフル機能のマークダウン・パーサーおよびコンパイラー。 スピードのために作成。
   - [merge](https://www.npmjs.com/package/merge) - 複数のオブジェクトを 1 つにマージし、オプションで新しい複製オブジェクトを作成します。
   - [moment](https://www.npmjs.com/package/moment) - 日付を構文解析、検証、操作、およびフォーマット設定するための軽量 JavaScript 日付ライブラリー。
   - [mongodb](https://www.npmjs.com/package/mongodb) - Node.js 用の公式 MongoDB ドライバー。
   - [mysql](https://www.npmjs.com/package/mysql) - これは mysql 用の node.js ドライバーです。
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js は、JavaScript での mustache テンプレート・システムの実装です。
   - [nano](https://www.npmjs.com/package/nano) - Node.js 用の最小主義の couchdb ドライバー。
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Node.js からの E メールの送信 - とても簡単です。
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Node.js で Express を使用して OAuth2 サーバー/プロバイダーを実装するための、完全な、準拠する、十分にテスト済みのモジュール。
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - OpenWhisk プラットフォーム用の JavaScript クライアント・ライブラリー。 OpenWhisk API のラッパーを提供します。
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - パス・ストリング (例えば /user/:name) を、URL パスとの突き合わせに使用できる正規表現に変換します。
   - [pg](https://www.npmjs.com/package/pg) - Node.js 用の非ブロッキング PostgreSQL クライアント。 Pure JavaScript およびオプションのネイティブ libpq バインディング。
   - [process](https://www.npmjs.com/package/process) - require('process'); 他のモジュールと同様。
   - [pug](https://www.npmjs.com/package/pug) - Pug テンプレート言語を実装します。
   - [redis](https://www.npmjs.com/package/redis) - これは、完全で機能が豊富な Node.js 用 Redis クライアントです。
   - [request](https://www.npmjs.com/package/request) - request は、HTTP 呼び出しを行う最も単純な方法であるように設計されています。
   - [request-promise](https://www.npmjs.com/package/request-promise) - Promise サポートのある単純化された HTTP 要求クライアント 'request'。 Bluebird で稼働。
   - [rimraf](https://www.npmjs.com/package/rimraf) - ノード用の UNIX コマンド rm -rf。
   - [semver](https://www.npmjs.com/package/semver) - Nodejs 用のセマンティック・バージョニング
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - SendGrid API を介して E メールをサポートします。
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - エラーをプレーン・オブジェクトとしてシリアライズします。
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - favicon に関するサービスを提供するための Node.js ミドルウェア。
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO は、リアルタイムで双方向のイベント・ベースの通信を可能にします。
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - socket.io のリアルタイム・アプリケーション・フレームワーク。
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent は小さな漸進的クライアント・サイド HTTP 要求ライブラリーであり、同じ API を持つ Node.js モジュールであり、多くの高水準な HTTP クライアント機能をサポートします。
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Swagger と統合および対話するための各種ツールを提供するパッケージ。
   - [twilio](https://www.npmjs.com/package/twilio) - 音声、ビデオ、およびメッセージングに関連した、Twilio API 用のラッパー。
   - [underscore](https://www.npmjs.com/package/underscore) - underscore.js は、コア JavaScript オブジェクトをどれも拡張することなく、通常はあると思われる機能 (each、map、reduce、filter 等) をサポートする、JavaScript 用の便利なライブラリーです。
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - regex ストリング・マッチャーを使用するよりも簡単に、パス・パラメーターの URL を構文解析します。
   - [uuid](https://www.npmjs.com/package/uuid) - RFC4122 UUIDS の単純で高速な生成。
   - [validator](https://www.npmjs.com/package/validator) - ストリングのバリデーターおよびサニタイザーのライブラリー。
   - [vcap_services](https://www.npmjs.com/package/vcap_services) - IBM Cloud が提供する VCAP_SERVICES 環境変数のサービス資格情報を解析して返します。
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud) - コグニティブ・コンピューティングを使用して複雑な問題を解決する API の集合である Watson Developer Cloud サービスを使用するための Node.js クライアント・ライブラリー。
   - [when](https://www.npmjs.com/package/when) - when.js は、完全な ES6 Promise shim を含む、厳しい試験を経た強固な Promises/A+ および when() 実装です。
   - [winston](https://www.npmjs.com/package/winston) - node.js 用のマルチ・トランスポート非同期ロギング・ライブラリー。 「落ち着いて! ログに入れたから。」
   - [ws](https://www.npmjs.com/package/ws) - ws は、使用が簡単で高速な、完全にテスト済みの、WebSocket クライアントおよびサーバーの実装です。
   - [xml2js](https://www.npmjs.com/package/xml2js) - XML から JavaScript オブジェクトへの単純なコンバーター。 双方向変換をサポートします。
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest は、ブラウザー XMLHttpRequest オブジェクトをエミュレートするための、組み込み http クライアント用のラッパーです。
   - [yauzl](https://www.npmjs.com/package/yauzl) - ノード用の別の抽出ライブラリー。

Node.js バージョン 8 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md) を参照してください。


## Python ランタイム
{: #openwhisk_ref_python_environments}

Python アクションでは 2 つの異なるランタイム・バージョンから選択できます。デフォルトでは、すべての Python アクションはバージョン 2 環境で実行されます。


### Python 3.7 のアクション (Debian Stretch ベース)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 のアクションは Python 3.7.x で実行されます。 このランタイムを使用するには、アクションを作成または更新するときに CLI パラメーター `--kind python:3.7` を指定します。

このランタイムには、Python 3.7 標準ライブラリーに加えて Python アクションで使用できる IBM Cloud サービス用 SDK パッケージが含まれています。

#### Python 3.7.2 パッケージ

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Python パッケージ:
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Python 3.7 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md) を参照してください。

### Python 3.6.8 のアクション (Debian Jessie ベース)
{: #openwhisk_ref_python_environments_3.6}

Python 3 アクションは Python 3.6.x で実行されます。 このランタイムを使用するには、アクションを作成または更新するときに CLI パラメーター `--kind python:3.6` を指定します。

このランタイムには、Python 3.6 標準ライブラリーに加えて Python アクションで使用できる IBM Cloud サービス用 SDK パッケージが含まれています。

#### Python 3.6.8 パッケージ

Python バージョン:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Python パッケージ:
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Python 3.6 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md) を参照してください。

### Python 2 アクション

アクションを作成または更新するときに `--kind` フラグを指定しない場合、Python 2 のアクションは、Python 2.7.15 で実行されます。

virtualenv を使用して python アクションを作成する場合は、Docker イメージ `openwhisk/python2action` を使用します。
Python 2.7 標準ライブラリーに加えて、以下のパッケージが Python 2 アクションで使用できます。

#### Python 2 パッケージ

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Python 2 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md) を参照してください。


## Swift ランタイム
{: #swift-actions}

デフォルトでは、すべての Swift アクションはバージョン 4.2 環境で実行されます。

Swift 4.x のアクション・ランタイムにはパッケージが組み込まれていません。Package.swift を使用して依存項目を組み込むには、[パッケージされた swift アクション](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single)の指示に従ってください。

単一の Swift ソース・ファイルを使用する場合、Swift 4.2 アクションは以下のパッケージを使用できます。
- Watson Developer Cloud SDK バージョン 1.2.0、https://github.com/watson-developer-cloud/swift-sdk


### 単一のソース・アクション・ファイルを使用する SwiftyJSON
コンパイルされていないアクションがあり、**SwiftyJSON** パッケージを使用する場合は、アクションをプリコンパイルし、`swift:4.2` の kind アクションで使用したい SwiftyJSON のバージョンを指定する必要があります。


## PHP ランタイム
{: #openwhisk_ref_php}

デフォルトでは、すべての PHP アクションはバージョン 7.3 環境で実行されます。

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

## Docker ランタイム
{: #openwhisk_ref_docker}

Docker アクションは、Docker コンテナー内のユーザー提供バイナリーを実行します。 バイナリーは、[python:3.6-alpine](https://hub.docker.com/r/library/python) に基づく Docker イメージで実行されるため、バイナリーはこのディストリビューションと互換でなければなりません。

Docker スケルトンは、OpenWhisk 互換の Docker イメージをビルドするための便利な方法です。 `ibmcloud fn sdk install docker` CLI プラグイン・コマンドでスケルトンをインストールできます。

メイン・バイナリー・プログラムはコンテナー内部の `/action/exec` に置かれる必要があります。 実行可能バイナリーは、`JSON` オブジェクトとしてデシリアライズ可能な単一コマンド・ライン引数ストリングから、入力引数を受け取ります。 また、シリアライズされた `JSON` の単一行ストリングとして `stdout` を使用して、結果を返す必要があります。

`dockerSkeleton` 内に含まれている `Dockerfile` を変更して、コンパイル・ステップや依存関係を組み込むことができます。



## Go ランタイム
{: #runtimes_go}

デフォルトでは、すべての Go アクションはバージョン 1.11 環境で実行されます。



## Java ランタイム
{: #runtimes_java}

デフォルトでは、すべての Java アクションはバージョン 8 環境で実行されます。



## Ruby ランタイム
{: #runtimes_ruby}

デフォルトでは、すべての Ruby アクションはバージョン 2.5 環境で実行されます。



## .NET Core ランタイム
{: #runtimes_dotnet}

デフォルトでは、すべての .NET Core アクションはバージョン 2.2 環境で実行されます。

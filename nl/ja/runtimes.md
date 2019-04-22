---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# ランタイム

## アクションのランタイム
アクションは複数のプログラミング言語 (Javascript、Python など) でコーディングして実行することができます。使用可能なランタイム環境を以下のセクションに記載します。

以下のリンクは、各地域で使用可能な IBM Cloud Functions のランタイムを示す JSON 応答を返します。

応答の `runtimes` セクションには、使用可能なランタイムのセットが含まれています。

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

`image` セクションには、[DockerHub](https://hub.docker.com/) 上のランタイム・イメージの名前と、使用されるタグが含まれています。 

以下の例は、イメージ `ibmfunctions/action-nodejs-v10` および `openwhisk/nodejs6action` を指し示しています。
タグは、`1.9.0` のようなバージョン番号、または `b99d71e` のような、Git コミット・ハッシュの簡易書式とすることができます。

イメージ・フィールドの例。
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- アクション・ランタイムは定期的に更新されます。これらの更新には、セキュリティー・フィックスやランタイム内のパッケージに対するマイナー・バージョン・アップデートなどがあります。マイナー・バージョン・アップデートにより後方互換性が中断されることがあります。ランタイムの更新によりアクションが影響を受けることがあります。同じタイプのより新しいランタイムへの自動マイグレーション機能はありません。
- 非推奨のランタイムに依存するアクションは、ランタイムがサポート対象のものに更新されるまで、正常に完了しません。失敗したアクションのトラブルシューティングをするとき、ランタイムが非推奨のものかどうかを見分けるには、照会応答に `deprecated=true` があるかどうかを調べます。ランタイムを更新する方法については、[アクション・ランタイムの変更](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)を参照してください

## JavaScript ランタイム環境
{: #openwhisk_ref_javascript_environments}

JavaScript アクションは、Node.js バージョン 8 または 10 で実行できます。 

Node.js バージョン 6 はデフォルトのバージョンですが、2018 年 12 月 6 日から非推奨となっています。JavaScript アクションの使用を続けるには、Node.js バージョン 8 または 10 に更新してください。
{: deprecated}

### IBM SDK のある Node.js バージョン 10 環境
{: #openwhisk_ref_javascript_environments_10}
アクションの作成または更新時に `--kind` フラグが明示的に指定され、値が `nodejs:10` である場合、Node.js バージョン 10 環境が使用されます。

#### `nodejs:8` から `nodejs:10` へのマイグレーション
- `ibm_db` npm パッケージは、`nodejs:10` では使用できません。`ibm_db` パッケージは Node.js 10 をサポートしていません。進捗は記事 [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541) で追跡できます。
- `cloudant` npm パッケージは `nodejs:10` では使用できません。このパッケージは非推奨です。nodejs モジュールをインポートするときは、公式の npm パッケージ [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 を使用する必要があります (つまり `require('@cloudant/cloudant')`)。また、[v3.x だけが Promises を返します](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)。
- `cradle` npm パッケージは、`nodejs:10` では使用できません。
- `log4js` npm パッケージは、`nodejs:10` では使用できません。この問題は [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805) で追跡できます
- `watson-developer-cloud` npm パッケージは、`nodejs:10` では使用できません。新しいバージョンに関する進捗は、記事 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780) で追跡できます。

nodejs バージョン 10 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md) を参照してください。

### IBM SDK のある Node.js バージョン 8 環境
{: #openwhisk_ref_javascript_environments_8}
アクションの作成または更新時に `--kind` フラグが明示的に指定され、値が `nodejs:8` である場合、Node.js バージョン 8 環境が使用されます。

Node.js バージョン 8 は保守モードになっていて、2019 年 12 月まで使用可能です。[Node.js のリリース・スケジュール](https://github.com/nodejs/Release)を参照してください。
{: deprecated}
 
Node.js バージョン 8 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md) を参照してください。

### Node.js バージョン 6 環境 (非推奨)
{: #openwhisk_ref_javascript_environments_6}
Node.js バージョン 6 はデフォルトのバージョンですが、非推奨となっています。JavaScript アクションの使用を続けるには、Node.js バージョン 8 または 10 に更新してください。
{: deprecated}

nodejs バージョン 6 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md) を参照してください。

## Python ランタイム環境
{: #openwhisk_ref_python_environments}

OpenWhisk は、異なる 2 つのランタイム・バージョンを使用した Python アクションの実行をサポートします。

### Python 3.7 のアクション (Debian Stretch ベース)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 のアクションは Python 3.7.x で実行されます。このランタイムを使用するには、アクションを作成または更新するときに `wsk` CLI パラメーター `--kind python:3.7` を指定します。

このランタイムには、Python 3.7 標準ライブラリーに加えて Python アクションで使用できる IBM Cloud サービス用 SDK パッケージが含まれています。

Python 3.7 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md) を参照してください。

### Python 3.6 のアクション (Debian Jessie ベース)
{: #openwhisk_ref_python_environments_3.6}

Python 3 アクションは Python 3.6.x で実行されます。 このランタイムを使用するには、アクションを作成または更新するときに `wsk` CLI パラメーター `--kind python:3.6` を指定します。

このランタイムには、Python 3.6 標準ライブラリーに加えて Python アクションで使用できる IBM Cloud サービス用 SDK パッケージが含まれています。

Python 3.6 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md) を参照してください。

### Python 2 アクション

アクションを作成または更新するときに `--kind` フラグを指定しない場合、Python 2 のアクションは、Python 2.7.15 で実行されます。このランタイムを明示的に選択するには、`--kind python:2` を使用します。

virtualenv を使用して python アクションを作成する場合は、Docker イメージ `openwhisk/python2action` を使用します。
Python 2.7 標準ライブラリーに加えて、以下のパッケージが Python 2 アクションで使用できます。

Python 2 ランタイム環境の詳細情報については、[CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md) を参照してください。

## Swift アクション
{: #swift-actions}

Swift 3.1.1 ランタイムと 4.1 ランタイムは非推奨であり、2019 年 2 月 28 日まで使用可能です。
`swift:4.2` の種類と新しいコンパイル・プロセスを使用して、新規アクションを開始するか、既存のアクションを Swift 4.2 ランタイムにマイグレーションしてください。
{: tip}

### Swift 3
Swift 3 アクションは Swift 3.1.1 `--kind swift:3.1.1` で実行されます。 前のバージョンの Swift はサポートされないため、常に kind `swift:3.1.1` を指定してください。

kind `swift:3.1.1` を使用するようにすべての Swift アクションをマイグレーションする必要があります。 ベスト・プラクティスとして、アクションを作成または更新するときは常に特定の kind を指定してください。
{: tip}

単一の Swift ソース・ファイルを使用する場合、Swift 3.1.1 アクションは以下のパッケージを使用できます。
- KituraNet バージョン 1.7.6、https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON バージョン 15.0.1、https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK バージョン 0.16.0、https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 のアクションは、Swift 4.1 または 4.2 を使用して実行できます。そのためには、それぞれ `--kind swift:4.1` または `--kind swift:4.2` を使用します。
デフォルトの `--kind swift:default` は Swift 4.2 です。

Swift 4.x のアクション・ランタイムにはパッケージが組み込まれていません。Package.swift を使用して依存項目を組み込むには、[パッケージされた swift アクション](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable)の指示に従ってください。

単一の Swift ソース・ファイルを使用する場合、Swift 4.1 アクションは以下のパッケージを使用できます。
- Watson Developer Cloud SDK バージョン 0.38.1、https://github.com/watson-developer-cloud/swift-sdk

単一の Swift ソース・ファイルを使用する場合、Swift 4.2 アクションは以下のパッケージを使用できます。
- Watson Developer Cloud SDK バージョン 1.2.0、https://github.com/watson-developer-cloud/swift-sdk

### Swift 3.1.1 から Swift 4.1 へのマイグレーション

#### 単一のソース・アクション・ファイルを使用する SwiftyJSON
コンパイルされていない `swift:3.1.1` のアクションがあり、それが **SwiftyJSON** パッケージを使用する場合、アクションをプリコンパイルして、`swift:4.2` の種類のアクションに使用する SwiftyJSON のバージョンを指定する必要があります。Swift 4.1 以降、JSON データを管理するための改良があることを考慮に入れてください。

## PHP アクション
{: #openwhisk_ref_php}

PHP 7.1 および 7.2 は、2019 年 1 月 11 日から非推奨になっています。PHP のアクションの使用を続けるには、PHP 7.3 に更新してください。
{: deprecated}

PHP のアクションは、PHP 7.3.0 を使用して実行されます。このランタイムを使用するには、アクションを作成または更新するときに `wsk` CLI パラメーター `--kind php:7.3` を指定します。 この動作は、`.php` 拡張子を持つファイルでアクションを作成する際のデフォルトです。

PHP 7.1 と 7.2 のランタイムは非推奨になっています。待ち時間の改善とエンドツーエンドの実行の高速化のために、すべてのアクションを PHP 7.3 にマイグレーションしてください。

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

## Docker アクション
{: #openwhisk_ref_docker}

Docker アクションは、Docker コンテナー内のユーザー提供バイナリーを実行します。 バイナリーは、[python:3.6-alpine](https://hub.docker.com/r/library/python) に基づく Docker イメージで実行されるため、バイナリーはこのディストリビューションと互換でなければなりません。

Docker スケルトンは、OpenWhisk 互換の Docker イメージをビルドするための便利な方法です。 `ibmcloud fn sdk install docker` CLI プラグイン・コマンドでスケルトンをインストールできます。

メイン・バイナリー・プログラムはコンテナー内部の `/action/exec` に置かれる必要があります。 実行可能バイナリーは、`JSON` オブジェクトとしてデシリアライズ可能な単一コマンド・ライン引数ストリングから、入力引数を受け取ります。 また、シリアライズされた `JSON` の単一行ストリングとして `stdout` を使用して、結果を返す必要があります。

`dockerSkeleton` 内に含まれている `Dockerfile` を変更して、コンパイル・ステップや依存関係を組み込むことができます。

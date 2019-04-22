---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: annotations, annotate, package, parameters, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# アノテーション
{: #openwhisk_annotations}

{{site.data.keyword.openwhisk}} のアクション、トリガー、ルール、およびパッケージ (これらを総称してエンティティーと呼びます) には、アノテーションを含めることができます。
{: shortdesc}

アノテーションは、パラメーターのようにエンティティーに付加されます。 アノテーションは、名前を定義する `key` と値を定義する `value` で構成されています。 アノテーションが最もよく使用されるのは、アクションとパッケージを文書化する場合です。 {{site.data.keyword.openwhisk_short}} カタログ内のパッケージの多くは、アクションが提供している機能の説明、パッケージのバインド時に使用するパラメーター、呼び出し時のパラメーター、あるいはパラメーターが秘密かどうかといったアノテーションを持っています。 アノテーションは、例えば UI 統合を可能にするためなど、必要に応じて作成されます。

エンティティーは、`--annotation` フラグ、または `-a` フラグを使用することで、CLI から文書化できます。

## アクションのアノテーション
{: #action}

アクションについて説明するアノテーションには、以下のものがあります。

- `description`: アクションの簡潔な説明。
- `parameters`: アクションの実行に必要なアクションについて説明する配列。
- `sampleInput`: 典型的な値を使用して入力スキーマを示す例。
- `sampleOutput`: 出力スキーマを示す例 (通常、`sampleInput` に対するもの)。



以下のコードは、`echo` という、入力引数を変更せずに返すアクションに関するアノテーション・セットの例です。 このアクションは、例えばシーケンスやルールの一部として、入力パラメーターをログに記録する場合などに有用です。

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Web アクションのアノテーション
{: #annotations-specific-to-web-actions}

API の対話機能を有効にするには、以下の Web アクションのアノテーションを明示的に `true` に設定する必要があります。

- `web-export`: アクションに適用すると、そのアクションは [Web アクション](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions)になります。 このアクションは、認証を行わずに REST 呼び出しからアクセス可能になるため、ユーザーはブラウザーからアクションにアクセスできるようになります。
    * **注**: Web アクションの所有者は、システム内でのアクションの実行コストを負担します。 つまり、アクションの所有者は、アクティベーション・レコードも所有することになります。
- `final`: アクションに適用されると、以前に定義されたアクション・パラメーターはすべて変更不能になります。 このパラメーターを、呼び出し中に指定されたパラメーターでオーバーライドすることはできません。
- `raw-http`: `web-export` アノテーションを持つアクションに適用されると、HTTP 要求の照会パラメーターおよび本体パラメーターが、予約済みプロパティーとしてそのアクションに渡されます。
- `web-custom-options`: Web アクションが、カスタマイズされたヘッダーを持つ OPTIONS 要求に応答できるようにします。 それ以外の場合は、[デフォルトの CORS 応答](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#options-requests)が適用されます。
- `require-whisk-auth`: Web アクションは、適切な認証資格情報を提供する要求によってのみ呼び出すことができます。
    * ブール値に設定すると、要求の基本認証値が認証されるかどうかが制御されます。 値が `true` の場合は、資格情報の認証が行われ、値が `false` の場合は、認証なしでアクションを呼び出します。
    * 整数またはストリングに設定する場合、その値は、要求の `X-Require-Whisk-Auth` ヘッダー値と一致しなければなりません。
    * **注**: Web アクションの所有者は、システム内でのアクションの実行コストを負担します。 つまり、アクションの所有者は、アクティベーション・レコードも所有することになります。

## パッケージのアノテーション
{: #package}

パッケージについて説明するアノテーションには、以下のものがあります。

- `description`: パッケージの簡潔な説明。
- `parameters`: パッケージのスコープにあるパラメーターについて説明する配列。

## パラメーターのアノテーション
{: #parameter}

パラメーターについて説明するアノテーションには、以下のものがあります。

- `name`: パラメーターの名前。
- `description`: パラメーターの簡潔な説明。
- `doclink`: パラメーターの追加資料へのリンク (OAuth トークンの場合に有用)。
- `required`: 必須パラメーターの場合は true、オプション・パラメーターの場合は false。
- `bindTime`: パッケージのバインド時にパラメーターが指定される場合は true。
- `type`: パラメーターのタイプ (`password`、`array` のいずれか (もっと広範囲に使用可能))。

## アクティベーションのアノテーション
{: #activation}

アクティベーション・レコードは、以下のアノテーションを使用して文書化できます。

- `path`: アクティベーションを生成したアクションの完全修飾パス名。 なお、当該アクティベーションがパッケージのバインディングにおけるアクションの結果として生じた場合、path は、親パッケージを指します。
- `kind`: 実行されたアクションの種類であり、サポートされる OpenWhisk ランタイムの種類のいずれか。
- `limits`: 当該アクティベーションに課された時間、メモリー、およびログの制限。

シーケンス関連のアクティベーションについては、システムは、以下のアノテーションを生成します。

- `topmost`: これは、最外部のシーケンス・アクションの場合にのみ存在します。
- `causedBy`: これは、シーケンスに含まれているアクションの場合にのみ存在します。

パフォーマンスの透過性を提供するために、アクティベーションによって以下も記録されます。

- `waitTime`: 内部 OpenWhisk システムで待機に費やされた時間。 これはおおよそ、Controller がアクティベーション要求を受け取ってから、Invoker がアクションのためにコンテナーのプロビジョンを終えるまでに費やされた時間です。 現在、この値は、非シーケンス関連のアクティベーションの場合にのみ存在します。 シーケンスの場合、この情報は、`topmost` シーケンス・アクティベーション・レコードから得ることができます。
- `initTime`: 関数の初期化に費やされた時間。 この値が存在する場合、アクションで初期化が必要であったことが分かり、またコールド・スタートであったことを示します。 ウォーム・アクティベーションは初期化をスキップするため、その場合、このアノテーションは生成されません。

以下に、アクティベーション・レコードに現れることがあるアノテーションの例を示します。

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}

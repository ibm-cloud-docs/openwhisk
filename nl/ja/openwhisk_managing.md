---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# アクションの管理
{: #managing_actions}
{: #openwhisk_managing}

アクション出力のモニター、アクションに関する特定の情報の取得、アクションの削除を行うことで、アクションを管理します。
{: shortdec}

## アクションの取得
{: #getting-actions}

アクションの作成後に、アクションに関する詳細情報を取得し、名前空間内のアクションをリストすることができます。
{: shortdesc}

作成したすべてのアクションをリストするには、次のようにします。
```
ibmcloud fn action list
```
{: pre}

作成したアクションが多くなるほど、関連するアクションを[パッケージ](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)にグループ化すると役立つことがあります。 アクションのリストをフィルターに掛けて、特定のパッケージ内のアクションのみにするには、次のようにします。
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

特定のアクションについて説明するメタデータを取得するには、次のようにします。

```
ibmcloud fn action get hello
```
{: pre}

出力例:
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption><code>action get</code> コマンド出力の説明</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>action get</code> コマンド出力の説明</th>
</thead>
<tbody>
<tr>
<td><code>名前空間 (namespace)</code></td>
<td>このアクションが含まれている名前空間。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>アクションの名前。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>アクションのセマンティック・バージョン。</td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code>: アクションのタイプ。 指定できる値は、nodejs:6、nodejs:8、php:7.1、python:3、python-jessie:3、swift:3.1.1、swift:4.1、java、blackbox、および sequence です。</li>
<li><code>code</code>: kind が nodejs または swift の場合に実行する Javascript コードまたは Swift コード。</li>
<li><code>components</code>: kind が sequence の場合のシーケンス内のアクション。 アクションは順番にリストされます。</li>
<li><code>image</code>: kind が blackbox の場合のコンテナー・イメージ名。</li>
<li><code>init</code> kind が nodejs の場合のオプションの zip ファイル参照。</li>
<li><code>binary</code>: アクションがバイナリー実行可能ファイルにコンパイルされるかどうか。</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>このアクションのアノテーション。 使用可能なアノテーションのリストについては、[アクション・アノテーション](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#action)および [Web アクション・アノテーション](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#annotations-specific-to-web-actions)の参照トピックを参照してください。</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>: アクションに対して設定される、アクションが終了するまでのタイムアウト (ミリ秒)。 デフォルト: 6000</li>
<li><code>memory</code>: アクションに対して設定される、最大メモリー制限 (MB)。 デフォルト値 : 256</li>
<li><code>logs</code>: アクションに対して設定される、最大ログ・サイズ制限 (MB)。 デフォルト: 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>アクションが公式に公開されるかどうか。</td>
</tr>
</tbody></table>

## アクティベーション詳細の表示
{: #activation_details}

{{site.data.keyword.openwhisk_short}} アクションは、他のユーザーによって、各種イベントに対する応答として、あるいはアクション・シーケンスの一部として呼び出すことができます。 アクションが呼び出されるたびに、その呼び出しのアクティベーション・レコードが作成されます。 アクション呼び出しの結果に関する情報を取得するには、アクティベーションに関する詳細を取得します。

名前空間内のすべてのアクティベーション・レコード ID を取得するには、次のようにします。
```
ibmcloud fn activation list
```
{: pre}

アクション呼び出しの結果作成された特定のアクティベーション・レコードに関する詳細を取得するには、次のようにします。
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

出力例:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "myNamespace/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption><code>activation get</code> コマンド出力の説明</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>activation get</code> コマンド出力の説明</th>
</thead>
<tbody>
<tr>
<td><code>名前空間 (namespace)</code></td>
<td>このアクティベーションが含まれている名前空間。 これは、アクションが含まれている名前空間とは異なる場合があります。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>アクションの名前。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>アクションのセマンティック・バージョン。</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>項目をアクティブ化したユーザー・アカウント。</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>このアクティベーション・レコードの ID。</td>
</tr>
<tr>
<td><code> start  </code></td>
<td>アクティベーションが開始された時刻。</td>
</tr>
<tr>
<td><code>end</code></td>
<td>アクティベーションが完了した時刻。</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>アクティベーションの完了にかかった時間 (ミリ秒)。</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: アクティベーションの終了状況。</li>
<li><code>statusCode</code>: 状況コード。 アクションでエラーが発生した場合は、HTTP エラー・コード。</li>
<li><code>success</code>: アクションが正常に完了したかどうか。</li>
<li><code>result</code>: アクティベーションからの戻り値。</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>このアクティベーションのログ。</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>このアクションのアノテーション。 使用可能なアクティベーション・アノテーションのリストについては、[アノテーションの参照トピック](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#activation)を参照してください。</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>アクションが公式に公開されるかどうか。</td>
</tr>
</tbody></table>

## アクション本体の中のアクション・メタデータへのアクセス
{: #accessing-action-metadata-within-the-action-body}

アクション環境は、実行中のアクションに固有のいくつかのプロパティーを含んでいます。 これらのプロパティーによって、アクションは REST API を介して OpenWhisk アセットをプログラマチックに処理したり、アクションに割り当てられた時間を使い切ってしまいそうなときに内部アラームを設定したりできます。 OpenWhisk Docker スケルトンを使用している場合、すべてのサポートされるランタイム (Node.js、Python、Swift、Java、および Docker) のシステム環境内でこれらのプロパティーにアクセスできます。

| プロパティー | 説明 |
| -------- | ----------- |
| `__OW_API_HOST` | このアクションを実行している OpenWhisk デプロイメントの API ホスト。 |
| `__OW_API_KEY` | アクションを呼び出すサブジェクトの API キー。このキーは制限付き API キーである場合もあり、明示的に要求されていなければ存在しません。[アノテーション](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#openwhisk_annotations)を参照してください。|
| `__OW_NAMESPACE` | アクティベーションの名前空間。 この名前空間は、アクションの名前空間とは同じではない場合があります。 |
| `__OW_ACTION_NAME` | 実行中のアクションの完全修飾名。 |
| `__OW_ACTIVATION_ID` | 実行中のアクション・インスタンスのアクティベーション ID。 |
| `__OW_DEADLINE` | このアクションが期間割り当て量全体を消費する概算時間 (エポック・ミリ秒)。 |

## アクション URL の取得
{: #get-action-url}

アクションは、HTTPS 要求を介して REST インターフェースを使用して呼び出すことができます。
{: shortdesc}

アクション URL を取得するには、次のようにします。
```
ibmcloud fn action get actionName --url
```
{: pre}

標準アクションの出力例:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

[Web アクション](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions)の出力例:
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**注:** 標準アクションの場合、HTTPS 要求を介してアクションを呼び出すときに認証を提供する必要があります。 REST インターフェースを使用したアクションの呼び出しについて詳しくは、[REST API リファレンス (REST API reference)](https://cloud.ibm.com/apidocs/functions) を参照してください。

## アクション・コードの保存
{: #save-action}

既存のアクションに関連付けられたコードを取得したり、ローカルに保存したりすることができます。 シーケンスおよび Docker アクションを除くすべてのアクションのコードを保存できます。
{: shortdesc}

現行作業ディレクトリー内に、既存のアクション名に対応するファイル名でアクション・コードが保存されます。
```
ibmcloud fn action get actionName --save
```
{: pre}

アクションの種類に対応するファイル拡張子が使用されます。 zip ファイルであるアクション・コードには .zip の拡張子が使用されます。 出力例:
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

代わりに、`--save-as` フラグを使用して、カスタムのファイル・パス、ファイル名、および拡張子を指定できます。
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

出力例:
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## アクション・ログのモニター
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}} アクションは、他のユーザーによって、各種イベントに対する応答として、あるいはアクション・シーケンスの一部として呼び出すことができます。 アクションがいつ呼び出されたかと、何が出力されたかに関する情報を取得するには、アクション・ログをモニターすると役立つことがあります。

{{site.data.keyword.openwhisk_short}} CLI を使用して、呼び出されたアクションの出力を監視できます。

1. アクティベーションからのログを継続的にチェックするポーリング・ループを開始します。
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. 別のウィンドウに切り替えて、アクションを呼び出します。
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    出力例:
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. ポーリング・ウィンドウでは、アクティベーション・ログを確認できます。
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    {{site.data.keyword.openwhisk_short}} でユーザーに代わり実行されたすべてのアクションのログをリアルタイムで確認することもできます。
    
## アクション・ランタイムの変更
{: #changing-action-runtime}

ランタイム `kind` を変更して、より新しいバージョンのアクション・ランタイムにマイグレーションできます。例えば、Node.js バージョン 8 は保守モードになっているので、ランタイムを Node.js 10 に切り替えることもできます。以下の手順を使用して、アクション・ランタイムを変更できます。**注:** 新しいランタイム・バージョンに準拠するように、`actionName.js` 内のコードを変更する必要が生じる場合があります。これは、そのような変更がランタイムの切り替えによって必要となるかどうかによって異なります。ほとんどの場合、ランタイム・バージョンには互換性があります。

1. アクション・コードをファイルに保存します。

  ```
  ibmcloud fn action get actionName --save
  ```
  {: pre}

2. 新しいランタイムを指定してアクションを更新します。

  ```
  ibmcloud fn action update actionName actionName.js --kind nodejs:10
  ```
  {: pre}

使用可能なランタイムのリストについては、[ランタイム](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)を参照してください

## 大規模アクションの管理
{: #large-app-support}

アクションの最大コード・サイズは 48 MB です。 多くのサード・パーティーのモジュール、ネイティブ・ライブラリー、外部ツールが含まれたアプリケーションでは、この制限に達する可能性があります。 48 MB より大きい .zip パッケージまたは .jar パッケージのアクションを作成する場合は、依存関係を含めてランタイム・イメージを拡張し、それから、単一のソース・ファイルか 48MB より小さなアーカイブを使用します。

例えば、必要な共有ライブラリーが含まれているカスタム Docker ランタイムをビルドすれば、依存関係はアーカイブ・ファイルに存在する必要がありません。 プライベート・ソース・ファイルは、引き続き、アーカイブにバンドルし、実行時に注入できます。

## アクションの削除
{: #deleting-actions}

使用しないアクションを削除することによって、クリーンアップできます。

1. アクションを削除します。
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    出力例:
    ```
    ok: deleted hello
    ```
    {: screen}

2. 当該アクションがアクションのリストに表示されなくなっていることを確認します。
    ```
    ibmcloud fn action list
    ```
    {: pre}

    出力例:
    ```
    actions
    ```
    {: screen}

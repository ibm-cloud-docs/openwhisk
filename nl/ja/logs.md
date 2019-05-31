---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

# ログの表示
{: #logs}

{{site.data.keyword.openwhisk}} では、問題をトラブルシューティングできるように、ロギングが自動的に有効になります。また、{{site.data.keyword.cloudaccesstraillong}} サービスを使用して、ユーザーおよびアプリケーションが {{site.data.keyword.openwhisk_short}} サービスとどのように対話するのかを追跡することもできます。


## アクション・ログのリアルタイム表示
{: #logs_poll}

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
<td>このアクションのアノテーション。 使用可能なアクティベーション・アノテーションのリストについては、[アノテーションの参照トピック](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation)を参照してください。</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>アクションが公式に公開されるかどうか。</td>
</tr>
</tbody></table>



## {{site.data.keyword.loganalysisfull_notm}} でのログの表示
{: #logs_view}

{{site.data.keyword.openwhisk_short}}の「モニタリング」ダッシュボードからアクティベーション・ログを直接表示できます。 また、ログは [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) に転送されます。ここでは、ログは索引付けられて、生成されたすべてのメッセージを対象にフルテキスト検索を行ったり、特定のフィールドに基づいて簡単に照会したりすることができます。
{:shortdesc}

**注**: ロギングは米国東部地域では利用できません。

1. [{{site.data.keyword.openwhisk_short}} の「モニタリング」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/dashboard) を開きます。

2. オプション : 特定のアクションについてのみログを表示するには、モニタリング・サマリーをそのアクションに制限します。 「フィルター・オプション」セクションで、**「制限」**ドロップダウン・リストからアクション名を選択します。

3. 左側のナビゲーションで、**「ログ」**をクリックします。 {{site.data.keyword.loganalysisshort_notm}} Kibana ページが開きます。

4. オプション: 古いログを表示するには、右上隅にある**「直近 15 分 (Last 15 minutes)」**をクリックして、異なる時間フレームを選択することで、デフォルトの時間フレーム値である 15 分を変更します。

### ログの照会
{: #logs_query}

Kibana の照会構文を使用することで、[{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) で特定のアクティベーション・ログを検出することができます。

以下の照会例は、エラーのデバッグに役立ちます。
  * すべてのエラー・ログを検出します。
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * 「myAction」によって生成されるすべてのエラー・ログを検出します。
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### 結果の照会
{: #logs_query_results}

ログ行に加えて、[{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) では、{{site.data.keyword.openwhisk_short}} によって生成される結果 (アクティベーション・レコード) にも索引付けされます。 結果には、アクティベーション期間やアクティベーション結果コードなどのアクティベーション・メタデータが含まれています。 結果フィールドを照会すると、{{site.data.keyword.openwhisk_short}} アクションがどのように動作しているのかを理解するのに役立ちます。

Kibana の照会構文を使用することで、特定のアクティベーション・ログを検出できます。 以下の照会例は、エラーのデバッグに役立ちます。

* 失敗したすべてのアクティベーションを検出します。
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    この結果では、`0` は正常に終了したアクションを示し、これ以外の値はすべてエラーを示します。

* 特定のエラーで失敗したすべてのアクティベーションを検出します。
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}


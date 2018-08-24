---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# ロギングおよびモニタリングのアクティビティー
{: #openwhisk_logs}

ロギングおよびモニタリングは {{site.data.keyword.openwhisk_short}} で自動的に有効になり、問題をトラブルシューティングして、アクションの正常性とパフォーマンスを向上させることができます。

## ログの表示
{: #view-logs}

{{site.data.keyword.openwhisk_short}}の「モニタリング」ダッシュボードからアクティベーション・ログを直接表示できます。また、ログは [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) に転送されます。ここでは、ログは索引付けられて、生成されたすべてのメッセージを対象にフルテキスト検索を行ったり、特定のフィールドに基づいて簡単に照会したりすることができます。
{:shortdesc}

1. [{{site.data.keyword.openwhisk_short}} の「モニタリング」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/dashboard/) を開きます。

2. オプション : 特定のアクションについてのみログを表示するには、モニタリング・サマリーをそのアクションに制限します。「フィルター・オプション」セクションで、**「制限」**ドロップダウン・リストからアクション名を選択します。

3. 左側のナビゲーションで、**「ログ」**をクリックします。{{site.data.keyword.loganalysisshort_notm}} Kibana ページが開きます。

4. オプション: 古いログを表示するには、右上隅にある**「直近 15 分 (Last 15 minutes)」**をクリックして、異なる時間フレームを選択することで、デフォルトの時間フレーム値である 15 分を変更します。

### ログの照会
{: #query-logs}

Kibana の照会構文を使用することで、[{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) で特定のアクティベーション・ログを検出することができます。

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
{: #query-results}

ログ行に加えて、[{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) では、{{site.data.keyword.openwhisk_short}} によって生成される結果 (アクティベーション・レコード) にも索引付けされます。結果には、アクティベーション期間やアクティベーション結果コードなどのアクティベーション・メタデータが含まれています。結果フィールドを照会すると、{{site.data.keyword.openwhisk_short}} アクションがどのように動作しているのかを理解するのに役立ちます。

Kibana の照会構文を使用することで、特定のアクティベーション・ログを検出できます。以下の照会例は、エラーのデバッグに役立ちます。

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

## アクティビティーのモニタリング
{: #openwhisk_monitoring}

[{{site.data.keyword.openwhisk_short}} ダッシュボード](https://console.bluemix.net/openwhisk/dashboard/)は、アクティビティーのグラフィック・サマリーを提供します。 ダッシュボードを使用して、{{site.data.keyword.openwhisk_short}} アクションのパフォーマンスおよび正常性を判別できます。
{:shortdesc}

どのアクション・ログを表示するのかを選択することでログをフィルターに掛けて、ログに記録されたアクティビティーの時間フレームを選択することができます。これらのフィルターは、ダッシュボード上のすべてのビューに適用されます。 いつでも**「再ロード」**をクリックして、最新のアクティベーション・ログ・データでダッシュボードを更新できます。

### アクティビティー・サマリー
{: #summary}

**「アクティビティー・サマリー」**ビューは、{{site.data.keyword.openwhisk_short}} 環境の概要レベルのサマリーを示します。 このビューを使用して、{{site.data.keyword.openwhisk_short}} 対応サービスの全体的な正常性とパフォーマンスをモニターします。このビューのメトリックから、以下のことが行えます。
* サービスの {{site.data.keyword.openwhisk_short}} 対応アクションの使用率を、それらのアクションが呼び出された回数を表示することで判別します。
* すべてのアクションで発生した障害の全体的な比率を判別します。 エラーが表示された場合、**「アクティビティー・ヒストグラム」**ビューを表示することによって、エラーが発生したサービスまたはアクションを切り分けることができます。さらに、**「アクティビティー・ログ」**を表示することによって、エラーそのものを切り分けます。
* 各アクションに関連付けられている平均完了時間を表示することによって、アクションの実行状況を判別します。

### アクティビティー・タイムライン
{: #timeline}

**「アクティビティー・タイムライン」**ビューには、過去および現在のアクションのアクティビティーを示す縦棒グラフが表示されます。 赤は、特定のアクションでのエラーを示します。 エラーについてもっと詳しく調べるには、このビューを**「アクティビティー・ログ」**と相互に関連付けます。

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### アクティビティー・ログ
{: #log}

この**「アクティビティー・ログ」**ビューは、アクティベーション・ログのフォーマット済みバージョンを表示します。 このビューには、各アクティベーションの詳細が表示されますが、1 分に 1 回ポーリングして新規のアクティベーションがないかを確認します。いずれかのアクションをクリックすると、詳細ログが表示されます。

アクティビティー・ログに表示される出力を CLI を使用して取得するには、以下のコマンドを使用します。
```
ibmcloud fn activation poll
```
{: pre}

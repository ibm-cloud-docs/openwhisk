---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# IBM Cloud でのアクティベーション・ログの表示
{: #openwhisk_logs}

[{{site.data.keyword.openwhisk}} の「モニタリング」ページ](https://console.bluemix.net/openwhisk/dashboard/)から直接、アクティベーション・ログを見ることができます。また、ログは [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) にも転送されます。ここでは、ログは索引付けられて、生成されたすべてのメッセージを対象にフルテキスト検索を行ったり、特定のフィールド (ログ・レベルなど) に基づいて簡易に照会したりできます。
{:shortdesc}

## ログの照会
{: #query-logs}

[IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) でホストされる Kibana を使用すると、簡単にログを照会できます。Kibana の照会構文を使用して、探しているログを検出できます。

{{site.data.keyword.openwhisk_short}} UI によって、Kibana でのログおよびアクションの結果に直接ナビゲートできます。[{{site.data.keyword.openwhisk}} の「モニタリング」ページ](https://console.bluemix.net/openwhisk/dashboard/)の内側の左にあるナビゲーションに、**「ログ」**リンクがあります。特定のアクションの詳細ページにアクセスしている場合、**「ログ」**リンクによって、その特定のアクションの結果 (アクティベーション・レコード) に進むことができます。ログを表示する時間フレームのデフォルト値は 15 分に設定されます。それより古いレコードを表示したい場合は、Kibana の右上隅でこの値を直接変更できます。

以下に、エラーのデバッグに役立つ照会の例をいくつか示します。

### すべてのエラー・ログの検出:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### 「myAction」によって生成されるすべてのエラー・ログの検出:
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## 結果の照会
{: #query-results}

[IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) は、ログ行に加えて、{{site.data.keyword.openwhisk_short}} によって生成された結果 (アクティベーション・レコード) も索引付けます。結果には、アクティベーションに関連する豊富なメタデータ (アクティベーションの期間や結果コード (成功、エラー) など) が含まれています。すべてのフィールドが照会可能であり、{{site.data.keyword.openwhisk_short}} アクションがどのように動作しているのかを理解するのに役立ちます。

Kibana の照会構文を使用して、探しているアクティベーションを検出できます。以下に、エラーのデバッグに役立つ照会の例をいくつか示します。

### 失敗したすべてのアクティベーションの検出:
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

UNIX コマンドの場合と同様に、「`0`」は正常終了したアクションを示し、それ以外はすべてエラーであると見なされます。

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### 特定のエラーで失敗したすべてのアクティベーションの検出:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}

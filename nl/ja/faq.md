---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-12"

keywords: faq, runtimes, actions, memory, monitoring

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:faq: data-hd-content-type='faq'}


# FAQ
{: #faq}

この FAQ では、{{site.data.keyword.openwhisk_short}} サービスに関する、よくある質問の答えを記載しています。
{: shortdesc}


## どの言語のランタイムがサポートされていますか?
{: #runtimes}
{: faq}

以下の言語がサポートされています。

<table>
  <tr>
    <th id="language-col">言語</th>
    <th id="kind-identifier-col">種類識別子</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col">Node.js</td>
    <td headers="kind-identifier-col language-col-nodejs"><code>nodejs:6</code>、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td id="language-col-python" headers="language-col">Python</td>
    <td headers="kind-identifier-col language-col-python"><code>python:3.7</code>、<code>python:3.6</code></td>
  </tr>
  <tr>
    <td id="language-col-swift" headers="language-col">Swift</td>
    <td headers="kind-identifier-col language-col-swift"><code>swift:4.1</code>、<code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td id="language-col-php" headers="language-col">PHP</td>
    <td headers="kind-identifier-col language-col-php"><code>php:7.2</code>、<code>php:7.1</code></td>
  </tr>
  <tr>
    <td id="language-col-ruby" headers="language-col">Ruby</td>
    <td headers="kind-identifier-col language-col-ruby"><code>ruby:2.5</code></td>
  </tr>
  <tr>
    <td id="language-col-java" headers="language-col">Java</td>
    <td headers="kind-identifier-col language-col-java"><code>java (JDK 8)</code></td>
  </tr>
  <tr>
    <td headers="language-col" colspan="2">Docker アクションを使用すると、他の言語がサポートされます。</td>
  </tr>
</table>
{: caption="表 1. サポートされるランタイム" caption-side="top"}


## 機能を実行できる最大時間はどれほどですか?
{: #max-runtime}
{: faq}

最大タイムアウト値は 10 分です。デフォルト値は 1 分に設定されていますが、CLI で `--timeout` フラグを使用して新しい値 (ミリ秒単位) を指定することで変更できます。また、GUI のアクション詳細セクションで値を変更することもできます。


## 機能で使用可能な最大メモリーはどれほどですか?
{: #max-memory}
{: faq}

1 機能あたり最大 2048MB のメモリーを使用できます。デフォルトは 256MB に設定されていますが、`--memory` フラグを使用して変更するか、GUI のアクション詳細セクションで変更することができます。


## アクションと Web アクションの違いは何ですか?
{: #difference}
{: faq}

アクションと Web アクションの主な違いは、応答出力オブジェクトにあります。[Web アクション](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions)の場合、結果は HTTP 応答となり、少なくとも JSON 出力に `body` フィールドが含まれている必要があります。オプションで、statusCode とヘッダーも含まれる可能性があります。

## アクションのログをどのようにして参照できますか?
{: #logs}
{: faq}

メトリックが収集された後、[{{site.data.keyword.loganalysislong_notm}} サービス](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs)を使用してログを参照できます。


## モニターはどのように機能しますか?
{: #monitor}
{: faq}

{{site.data.keyword.monitoringlong}} を使用すると、{{site.data.keyword.openwhisk_short}} によってデプロイされたアクションのパフォーマンスに関する洞察を得ることができます。また、ダッシュボードを使用してアクティビティーに関するグラフィカルなサマリーを表示することで、アクションの正常性とパフォーマンスをモニターすることもできます。



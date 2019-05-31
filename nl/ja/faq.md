---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: faq, runtimes, actions, memory, monitoring

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
{:faq: data-hd-content-type='faq'}


# FAQ
{: #faq}

この FAQ では、{{site.data.keyword.openwhisk_short}} サービスに関する、よくある質問の答えを記載しています。
{: shortdesc}


## どの言語のランタイムがサポートされていますか?
{: #supported-runtimes}

以下の言語がサポートされています。

<table>
  <tr>
    <th>言語</th>
    <th>種類識別子</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (デフォルト)、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code>、<code>python:2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (デフォルト)</td>
  </tr>
  <tr>
    <td>Docker アクションを使用すると、他の言語がサポートされます。</td>
  </tr>
</table>
{: caption="表 1. サポートされるランタイム" caption-side="top"}


## 機能を実行できる最大時間はどれほどですか?
{: #max-runtime}

最大タイムアウト値は 10 分です。 デフォルト値は 1 分に設定されていますが、CLI で `--timeout` フラグを使用して新しい値 (ミリ秒単位) を指定することで変更できます。 また、GUI のアクション詳細セクションで値を変更することもできます。


## 機能で使用可能な最大メモリーはどれほどですか?
{: #max-memory}

1 機能あたり最大 2048MB のメモリーを使用できます。 デフォルトは 256MB に設定されていますが、`--memory` フラグを使用して変更するか、GUI のアクション詳細セクションで変更することができます。



## アクションと Web アクションの違いは何ですか?
{: #difference}

アクションと Web アクションの主な違いは、応答出力オブジェクトにあります。 [Web アクション](/docs/openwhisk?topic=cloud-functions-actions_web)の場合、結果は HTTP 応答となり、少なくとも JSON 出力に `body` フィールドが含まれている必要があります。 オプションで、statusCode とヘッダーも含まれる可能性があります。

## アクションのログをどのようにして参照できますか?
{: #logs_faq}

メトリックが収集された後、[{{site.data.keyword.loganalysislong_notm}} サービス](/docs/openwhisk?topic=cloud-functions-logs)を使用してログを参照できます。


## モニターはどのように機能しますか?
{: #monitor_faq}

{{site.data.keyword.monitoringlong}} を使用すると、{{site.data.keyword.openwhisk_short}} によってデプロイされたアクションのパフォーマンスに関する洞察を得ることができます。 また、ダッシュボードを使用してアクティビティーに関するグラフィカルなサマリーを表示することで、アクションの正常性とパフォーマンスをモニターすることもできます。





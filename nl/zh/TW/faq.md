---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: faq, runtimes, actions, memory, monitoring, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:faq: data-hd-content-type='faq'}



# 常見問題
{: #faq}

此常見問題提供 {{site.data.keyword.openwhisk_short}} 服務常見問題的答案。
{: shortdesc}


## 支援哪些語言運行環境？
{: #supported-runtimes}

以下是支援的語言：

<table>
  <tr>
    <th>語言</th>
    <th>類型 ID</th>
  </tr>
  <tr>
    <td> Node.js </td>
    <td> <code>nodejs:10</code>（預設值）、<code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code>、<code>python:2</code>（預設值）</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>（預設值）</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>（預設值）</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>（預設值）</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>（預設值）</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>（預設值）</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>（預設值）</td>
  </tr>
  <tr>
    <td>其他語言是使用 Docker 動作支援的。</td>
  </tr>
</table>
{: caption="表 1. 受支援的運行環境" caption-side="top"}


## 函數最多可以執行多長時間？
{: #max-runtime}

逾時上限為 10 分鐘。預設值設為 1 分鐘，但可以透過 CLI 進行變更，方法是使用 `--timeout` 旗標來指定新值（毫秒）。您也可以在動作詳細資料區段中透過 GUI 來變更值。

## 函數可以使用的上限記憶體是多少？
{: #max-memory}

對於每個函數，最多可以使用 2048 MB 的記憶體。預設設定為 256 MB，但可以使用 `--memory` 旗標或透過 GUI 中的動作詳細資料區段來變更此預設。

## 動作與 Web 動作有何區別？
{: #difference}

動作與 Web 動作之間的主要差異是回應輸出物件。對於 [Web 動作](/docs/openwhisk?topic=cloud-functions-actions_web)，結果代表 HTTP 回應，其中 JSON 輸出至少包含 `body` 欄位。（選用）還可以包含 `statusCode` 和 `headers`。

## 如何查看我的動作日誌？
{: #logs_faq}

收集度量值之後，您可以使用 [{{site.data.keyword.loganalysislong_notm}} 服務](/docs/openwhisk?topic=cloud-functions-logs)來查看日誌。

## 監視的運作方式為何？
{: #monitor_faq}

您可以使用 {{site.data.keyword.monitoringlong}}，以瞭解使用 {{site.data.keyword.openwhisk_short}} 部署之動作的效能。您也可以使用儀表板來查看活動的圖形摘要，以監視動作的性能及效能。











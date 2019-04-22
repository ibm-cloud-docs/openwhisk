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


# 常見問題
{: #faq}

此常見問題提供 {{site.data.keyword.openwhisk_short}} 服務常見問題的答案。
{: shortdesc}


## 支援哪些語言運行環境？
{: #runtimes}
{: faq}

下列是支援的語言：

<table>
  <tr>
    <th id="language-col">語言</th>
    <th id="kind-identifier-col">類型 ID</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col"> Node.js </td>
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
    <td headers="language-col" colspan="2">其他語言是使用 Docker 動作支援的。</td>
  </tr>
</table>
{: caption="表 1. 受支援的運行環境" caption-side="top"}


## 我的函數可以執行的時間上限為何？
{: #max-runtime}
{: faq}

逾時上限為 10 分鐘。預設值設為 1 分鐘，但可以透過 CLI 進行變更，方法是使用 `--timeout` 旗標來指定新值（毫秒）。您也可以在動作詳細資料區段中透過 GUI 來變更值。


## 我的函數可以使用的記憶體上限為何？
{: #max-memory}
{: faq}

每個函數最多可以使用 2048MB 的記憶體。預設值設為 256MB，但您可以在動作詳細資料區段中使用 `--memory` 旗標或透過 GUI 來進行變更。


## 動作與 Web 動作之間的差異為何？
{: #difference}
{: faq}

動作與 Web 動作之間的主要差異是回應輸出物件。對於 [Web 動作](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions)，結果代表 HTTP 回應（其中是最小值），而 JSON 輸出應該具有 `body` 欄位。它可能也會選擇性地包含 statusCode 及 headers。

## 如何查看我的動作日誌？
{: #logs}
{: faq}

收集度量值之後，您可以使用 [{{site.data.keyword.loganalysislong_notm}} 服務](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs)來查看日誌。


## 監視的運作方式為何？
{: #monitor}
{: faq}

您可以使用 {{site.data.keyword.monitoringlong}}，以瞭解使用 {{site.data.keyword.openwhisk_short}} 部署之動作的效能。您也可以使用儀表板來查看活動的圖形摘要，以監視動作的性能及效能。



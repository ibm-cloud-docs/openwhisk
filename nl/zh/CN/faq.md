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


# 常见问题
{: #faq}

此常见问题提供了对 {{site.data.keyword.openwhisk_short}} 服务相关常见问题的解答。
{: shortdesc}


## 支持哪些语言运行时？
{: #runtimes}
{: faq}

支持以下语言：

<table>
  <tr>
    <th id="language-col">语言</th>
    <th id="kind-identifier-col">Kind 标识</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col">Node.js</td>
    <td headers="kind-identifier-col language-col-nodejs"><code>nodejs:6</code> 和 <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td id="language-col-python" headers="language-col">Python</td>
    <td headers="kind-identifier-col language-col-python"><code>python:3.7</code> 和 <code>python:3.6</code></td>
  </tr>
  <tr>
    <td id="language-col-swift" headers="language-col">Swift</td>
    <td headers="kind-identifier-col language-col-swift"><code>swift:4.1</code> 和 <code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td id="language-col-php" headers="language-col">PHP</td>
    <td headers="kind-identifier-col language-col-php"><code>php:7.2</code> 和 <code>php:7.1</code></td>
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
    <td headers="language-col" colspan="2">其他语言使用 Docker 操作进行支持。</td>
  </tr>
</table>
{: caption="表 1. 支持的运行时" caption-side="top"}


## 函数最多可以运行多长时间？
{: #max-runtime}
{: faq}

最大超时为 10 分钟。缺省值设置为 1 分钟，但可以通过 CLI 使用 `--timeout` 标志来指定新值（以毫秒为单位）以更改缺省值。此外，还可以通过 GUI 在操作详细信息部分中更改该值。


## 函数可以使用的最大内存是多少？
{: #max-memory}
{: faq}

对于每个函数，最多可以使用 2048 MB 内存。缺省值设置为 256 MB，但可以使用 `--memory` 标志或通过 GUI 中的操作详细信息部分来更改此缺省值。


## 操作与 Web 操作有何区别？
{: #difference}
{: faq}

操作和 Web 操作之间的主要区别在于响应输出对象。对于 [Web 操作](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions)，结果表示 HTTP 响应，其中 JSON 输出应该至少具有 `body` 字段。（可选）还可以包含 statusCode 和 headers。

## 如何查看操作日志？
{: #logs}
{: faq}

收集度量值后，可以使用 [{{site.data.keyword.loganalysislong_notm}} 服务](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs)来查看日志。


## 监视是如何运作的？
{: #monitor}
{: faq}

您可以通过 {{site.data.keyword.monitoringlong}} 来了解使用 {{site.data.keyword.openwhisk_short}} 部署的操作的性能。此外，还可以使用仪表板来查看活动的图形摘要，从而监视操作的运行状况和性能。



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


# 常见问题
{: #faq}

此常见问题提供了对 {{site.data.keyword.openwhisk_short}} 服务相关常见问题的解答。
{: shortdesc}


## 支持哪些语言运行时？
{: #supported-runtimes}

支持以下语言：

<table>
  <tr>
    <th>语言</th>
    <th>Kind 标识</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code>（缺省值）和 <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>、<code>python:3.6</code> 和 <code>python:2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>（缺省值）</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>（缺省值）</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>（缺省值）</td>
  </tr>
  <tr>
    <td>其他语言使用 Docker 操作进行支持。</td>
  </tr>
</table>
{: caption="表 1. 支持的运行时" caption-side="top"}


## 函数最多可以运行多长时间？
{: #max-runtime}

最大超时为 10 分钟。缺省值设置为 1 分钟，但可以通过 CLI 使用 `--timeout` 标志来指定新值（以毫秒为单位）以更改缺省值。此外，还可以通过 GUI 在操作详细信息部分中更改该值。


## 函数可以使用的最大内存是多少？
{: #max-memory}

对于每个函数，最多可以使用 2048 MB 内存。缺省值设置为 256 MB，但可以使用 `--memory` 标志或通过 GUI 中的操作详细信息部分来更改此缺省值。



## 操作与 Web 操作有何区别？
{: #difference}

操作和 Web 操作之间的主要区别在于响应输出对象。对于 [Web 操作](/docs/openwhisk?topic=cloud-functions-actions_web)，结果表示 HTTP 响应，其中 JSON 输出应该至少具有 `body` 字段。（可选）还可以包含 statusCode 和 headers。

## 如何查看操作日志？
{: #logs_faq}

收集度量值后，可以使用 [{{site.data.keyword.loganalysislong_notm}} 服务](/docs/openwhisk?topic=cloud-functions-logs)来查看日志。


## 监视是如何运作的？
{: #monitor_faq}

您可以通过 {{site.data.keyword.monitoringlong}} 来了解使用 {{site.data.keyword.openwhisk_short}} 部署的操作的性能。此外，还可以使用仪表板来查看活动的图形摘要，从而监视操作的运行状况和性能。





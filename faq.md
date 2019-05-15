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

This FAQ provides answers to common questions about the {{site.data.keyword.openwhisk_short}} service.
{: shortdesc}


## What language runtimes are supported?
{: #supported-runtimes}

The following languages are supported:

<table>
  <tr>
    <th id="language-col">Language</th>
    <th id="kind-identifier-col">Kind identifier</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col">Node.js</td>
    <td headers="kind-identifier-col language-col-nodejs"><code>nodejs:6</code>, <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td id="language-col-python" headers="language-col">Python</td>
    <td headers="kind-identifier-col language-col-python"><code>python:3.7</code>, <code>python:3.6</code></td>
  </tr>
  <tr>
    <td id="language-col-swift" headers="language-col">Swift</td>
    <td headers="kind-identifier-col language-col-swift"><code>swift:4.1</code>, <code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td id="language-col-php" headers="language-col">PHP</td>
    <td headers="kind-identifier-col language-col-php"><code>php:7.2</code>, <code>php:7.1</code></td>
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
    <td headers="language-col" colspan="2">Other languages are supported by using Docker actions.</td>
  </tr>
</table>
{: caption="Table 1. Supported runtimes" caption-side="top"}


## What's the maximum time my function can run?
{: #max-runtime}
{: faq}

The maximum timeout is 10 minutes. The default value is set to 1 minute, but can be changed through the CLI by specifying a new value in milliseconds by using the `--timeout` flag. You can also change the value through the GUI in the action details section.


## What's the maximum memory my function can use?
{: #max-memory}
{: faq}

You can use up to 2048MB of memory for each function. The default is set to 256MB, but you can change it by using the `--memory` flag or through the GUI in the action details section.


## What's the difference between an action and and a web action?
{: #difference}
{: faq}

The main difference between an action and a web action is the response output object. For [web actions](/docs/openwhisk?topic=cloud-functions-actions_web), the result represents an HTTP response, where are a minimum, the JSON output should have a `body` field. Optionally, it might also contain a statusCode and headers.

## How can I see my action logs?
{: #logs_faq}
{: faq}

After the metrics are collected, you can see your logs by using the [{{site.data.keyword.loganalysislong_notm}} service](/docs/openwhisk?topic=cloud-functions-logs).


## How does monitoring work?
{: #monitor_faq}
{: faq}

You can get insight into the performance of your actions that are deployed with {{site.data.keyword.openwhisk_short}} by using {{site.data.keyword.monitoringlong}}. You can also monitor the health and performance of your actions by using the dashboard to see a graphical summary of your activity.





---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:faq: data-hd-content-type='faq'}


# FAQ
{: #faq}

This FAQ provides answers to common questions about the {{site.data.keyword.openwhisk_short}} service.
{: shortdesc}


## What language runtime are supported?
{: #runtimes}
{: faq}

The following languages are supported:

<table>
  <tr>
    <th>Language</th>
    <th>Kind identifier</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td><code>nodejs:6</code>, <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code></td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.1</code>, <code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.2</code>, <code>php:7.1</code></td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code></td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code></td>
  </tr>
  <tr>
    <td colspan="2">Any other language by using Docker actions.</td>
  </tr>
</table>


## What's the maximum time my function can run?
{: #max-runtime}
{: faq}

The maximum timeout is 10 minutes. The default value is set to 1 minute, but can be changed through the CLI by specifying a new value in milliseconds by using the `--timeout` flag. You can also change the value through the GUI in the action details section.


## What's the maximum memory my function can use?
{: #max-memory}
{: faq}

You can use up to 512MB of memory for each function. The default is set to 256MB, but you can change it by using the `--memory` flag or through the GUI in the action details section.

## What's the difference between an action and and a web action?
{: #difference}
{: faq}

The main difference between an action and a web action is the response output object. For [web actions](openwhisk_webactions.html), the result represents an HTTP response, where are a minimum, the JSON output should have a `body` field. Optionally, it might also contain a statusCode and headers.

## How can I see my action logs?
{: #logs}
{: faq}

After the metrics are collected, you can see your logs by using the [{{site.data.keyword.loganalysislong_notm}} service](openwhisk_logs.html#view-logs).


## How does monitoring work?
{: #monitor}
{: faq}

You can get insight into the performance of your actions that are deployed with {{site.data.keyword.openwhisk_short}} by using {{site.data.keyword.monitoringlong}}. You can also monitor the health and performance of your actions by using the dashboard to see a graphical summary of your activity.




---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-22"

keywords: faq, runtimes, actions, memory, monitoring, functions

subcollection: openwhisk

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

# FAQ
{: #faq}

This FAQ provides answers to common questions about the {{site.data.keyword.openwhisk_short}} service.
{: shortdesc}

## What language runtimes are supported in {{site.data.keyword.openwhisk_short}}?
{: #supported-runtimes}
{: faq}
{: support}

The following languages are supported:

<table>
  <tr>
    <th>Language</th>
    <th>Kind identifier</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (default)</td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (default)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (default)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (default)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (default)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (default)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (default)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (default)</td>
  </tr>
  <tr>
    <td>Other languages are supported by using Docker actions.</td>
  </tr>
</table>
{: caption="Table 1. Supported runtimes" caption-side="top"}


## What's the maximum time that my function can run?
{: #max-runtime}
{: faq}

The maximum timeout is 10 minutes. The default value is set to 1 minute, but can be changed through the CLI by specifying a new value in milliseconds by using the `--timeout` flag. You can also change the value through the GUI in the action details section.

## What's the maximum memory that my function can use?
{: #max-memory}
{: faq}

You can use up to 2048 MB of memory for each function. The default is set to 256 MB, but you can change it by using the `--memory` flag or through the GUI in the action details section.

## What's the difference between an action and a web action?
{: #difference}
{: faq}

The main difference between an action and a web action is the response output object. For [web actions](/docs/openwhisk?topic=openwhisk-actions_web), the result represents an HTTP response, where at a minimum, the JSON output contains a `body` field. Optionally, it might also contain a `statusCode` and `headers`.

## How can I see my action logs?
{: #logs_faq}
{: faq}

After the metrics are collected, you can see your logs by using the [{{site.data.keyword.loganalysislong_notm}} service](/docs/openwhisk?topic=openwhisk-logs).


<!-- Added comment -->

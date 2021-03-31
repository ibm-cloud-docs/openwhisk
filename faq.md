---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-31"

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

{{site.data.keyword.openwhisk_short}} supports many different language runtimes, including Node.js, Python, and PHP. Languages without specific support in {{site.data.keyword.openwhisk_short}} are supported by using Docker actions. For a complete list of supported language runtimes, see [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).


## What's the maximum time or maximum memory that my function can run?
{: #max-runtime}
{: faq}

The maximum timeout is 10 minutes and you can use up to 2048 MB of memory for each function. Find more details about these limits and others in [System details and limits](/docs/openwhisk?topic=openwhisk-limits).

## What's the difference between an action and a web action?
{: #difference}
{: faq}

The main difference between an [action](/docs/openwhisk?topic=openwhisk-actions) and a [web action](/docs/openwhisk?topic=openwhisk-actions_web) is the response output object. For web actions, the result represents an HTTP response, where at a minimum, the JSON output contains a `body` field. Optionally, it might also contain a `statusCode` and `headers`.

## How can I see my action logs?
{: #logs_faq}
{: faq}

You can see your logs by using the instructions in [Viewing logs](/docs/openwhisk?topic=openwhisk-logs).

## How does monitoring work?
{: #monitor_faq}
{: faq}

You can get insight into the performance of your actions that are deployed with {{site.data.keyword.openwhisk_short}} by using [{{site.data.keyword.monitoringlong}}](/docs/openwhisk?topic=openwhisk-monitor-functions). You can also monitor the health and performance of your actions by using the dashboard to see a graphical summary of your activity.

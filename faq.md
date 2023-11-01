---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: faq, runtimes, actions, memory, monitoring, functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# FAQ
{: #faq}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

This FAQ provides answers to common questions about the {{site.data.keyword.openwhisk}} service.
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



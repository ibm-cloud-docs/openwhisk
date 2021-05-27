---

copyright:
  years: 2021
lastupdated: "2021-05-26"

keywords: error, error handling, functions, serverless, monitoring, composer

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

# Error handing for {{site.data.keyword.openwhisk_short}}
{: #error-handing-functions}

{{site.data.keyword.openwhisk}} provides error handling through the execution status of each function invocation. Error handling is provided by {{site.data.keyword.openwhisk_short}} for invocations that are exposed through the {{site.data.keyword.openwhisk_short}} API, SDK, CLI, or the console. For more information about the various error types, see  [Action executions](/docs/openwhisk?topic=openwhisk-limits#limits_exec).
{: shortdesc}

In addition, the {{site.data.keyword.openwhisk_short}} service automatically reports errors to the Monitoring service. You can set up the built-in integration for the Monitoring service with the {{site.data.keyword.openwhisk}} service to automatically react to error messages. For more information about setting up an alert through a subsequent action invocation, see [{{site.data.keyword.mon_full_notm}}](/docs/openwhisk?topic=openwhisk-alerts-notify).

In order to handle errors, choose one of the following options.

1. Handle application errors programmatically within the action implementation itself and handle service errors through the {{site.data.keyword.openwhisk_short}} service API, CLI, SDK or console by leveraging the automatically set {{site.data.keyword.openwhisk_short}} execution status.

2. Use the built-in integration with the {{site.data.keyword.mon_full_notm}} service to define a subsequent action to be invoked in response to the error. All configuration and execution occurs service-side. For more information, see [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-notifications) and [Monitoring {{site.data.keyword.openwhisk_short}} entities with {{site.data.keyword.mon_full_notm}}](/docs/openwhisk?topic=openwhisk-monitor-functions).

3. Handle errors programmatically by including the Composer library within the action implementation. By using the library, you can chain together a sequence of actions or define other types of control-flow logic. When you use this library, all execution of the [error-handling code occurs service-side](/docs/openwhisk?topic=openwhisk-pkg_composer#error-handling) and there are no additional components that you must manage. To articulate the error-handling logic programmatically, import the Composer library, a common practice in the programming language communities. For more information about installing and using the Composer library with {{site.data.keyword.openwhisk_short}}, see [Composer](/docs/openwhisk?topic=openwhisk-pkg_composer).

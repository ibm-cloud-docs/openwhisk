---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: feeds, serverless, functions

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



# Creating custom event provider feeds
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} supports an open API, where any user can expose an event producer service as a feed in a package.
{: shortdesc}


## Feed architecture
{: #feeds_arch}

You can create a feed by using one of the three architectural patterns: **Hooks**, **Polling**, and **Connections**.

### Hooks

With the hooks pattern, a feed is set up by using a [webhook](https://en.wikipedia.org/wiki/Webhook){: external} that is exposed by another service. In this strategy, a webhook is configured on an external service to POST directly to a URL to fire a trigger. This method is by far the easiest and most attractive option for implementing low-frequency feeds.

For example, the [GitHub package](/docs/openwhisk?topic=cloud-functions-pkg_github)  and the [Push Notification package](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications) use a webhook.


### Polling

With the polling pattern, a {{site.data.keyword.openwhisk_short}} action is arranged to poll an endpoint periodically to fetch new data. This pattern is relatively easy to build, but the frequencies of events are limited by the polling interval.

### Connections

With the connections pattern, a separate service maintains a persistent connection to a feed source. The connection-based implementation might interact with a service endpoint by using long polling intervals, or to set up a push notification.

For example, the [{{site.data.keyword.cloudant}} package](/docs/openwhisk?topic=cloud-functions-pkg_cloudant) uses the connections pattern.



##  Implementing feed actions
{: #feeds_actions}

The feed action is an action, and accepts the following parameters.

| Parameter | Description |
| --- | --- |
| `lifecycleEvent` | `CREATE`, `READ`, `UPDATE`, `DELETE`, `PAUSE`, or `UNPAUSE`. |
| `triggerName` | The fully qualified name of the trigger, which contains events that are produced from this feed. |
| `authKey` | The basic authentication credentials of the {{site.data.keyword.openwhisk_short}} user who owns the trigger. |

The feed action can also accept any other parameters that it needs to manage the feed. For example, the {{site.data.keyword.cloudant}} changes feed action expects to receive parameters that include `dbname` and `username`.

When the user creates a trigger from the CLI with the `--feed` parameter, the feed action is automatically invoked with the appropriate parameters.

For example, a user creates a **mycloudant** binding for the `cloudant` package with a username and password as bound parameters. When the user issues the following command from the CLI:
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

Then, something equivalent to the following command runs:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

The feed action that is named *changes* takes these parameters, and is expected to perform whatever action is necessary to set up a stream of events from {{site.data.keyword.cloudant_short_notm}}. The feed action occurs by using the appropriate configuration, which is directed to the trigger.

For the {{site.data.keyword.cloudant_short_notm}} *changes* feed, the action talks directly to a *{{site.data.keyword.cloudant_short_notm}} trigger* service that is implemented with a connection-based architecture.

A similar feed action protocol occurs for `ibmcloud fn trigger delete`, `ibmcloud fn trigger update`, and `ibmcloud fn trigger get`.

## Implementing feeds with hooks
{: #feeds_hooks}

Set up a feed by using a hook when an event producer supports a webhook-callback facility.

With this method, you are not required to stand up any persistent service outside of {{site.data.keyword.openwhisk_short}}. All feed management happens naturally though stateless {{site.data.keyword.openwhisk_short}} **feed actions**, which negotiate directly with a third-party webhook API.

When invoked with `CREATE`, the feed action simply installs a webhook for some other service, asking the remote service to POST notifications to the appropriate `fireTrigger` URL in {{site.data.keyword.openwhisk_short}}.

The webhook is directed to send notifications to a URL such as:

`POST /namespaces/{namespace}/triggers/{triggerName}`

The form with the POST request is interpreted as a JSON document that defines parameters on the trigger event. {{site.data.keyword.openwhisk_short}} rules pass these trigger parameters to any actions to fire as a result of the event.

## Implementing feeds with polling
{: #feeds_polling}

You can set up an action to poll a feed source entirely within {{site.data.keyword.openwhisk_short}}, without the need to stand up any persistent connections or external service.

For feeds where a webhook is not available, but do not need high-volume or low latency response times, you can use polling.

To set up a polling-based feed, the feed action takes the following steps when called for `CREATE`:

1. The feed action sets up a periodic trigger with a specific frequency, by using the `whisk.system/alarms` feed.
2. The feed developer creates a `pollMyService` action that polls the remote service and returns any new events.
3. The feed action sets up a *rule* *T -> pollMyService*.

This procedure implements a polling-based trigger entirely by using {{site.data.keyword.openwhisk_short}} actions, without any need for a separate service.

## Implementing feeds by using connections
{: #feeds_connections}

The previous two architectural choices are simple and easy to implement. However, if you want a high-performance feed, you can use persistent connections and long-polling or similar techniques.

Since {{site.data.keyword.openwhisk_short}} actions must be short-running, an action cannot maintain a persistent connection to a third party. Instead, you can stand up a separate service, called **provider services**, outside of {{site.data.keyword.openwhisk_short}} that run all the time. A provider service can maintain connections to third-party event sources that support long polling or other connection-based notifications.

The provider service has a REST API that allows the {{site.data.keyword.openwhisk_short}} **feed action** to control the feed. The provider service acts as a proxy between the event provider and {{site.data.keyword.openwhisk_short}}. When it receives events from the third party, it sends them on to {{site.data.keyword.openwhisk_short}} by firing a trigger.

The {{site.data.keyword.cloudant_short_notm}} **changes** feed is the canonical example as it stands up a `cloudanttrigger` service, which mediates between {{site.data.keyword.cloudant_short_notm}} notifications over a persistent connection, and {{site.data.keyword.openwhisk_short}} triggers.


The **alarm** feed is implemented with a similar pattern.

The connection-based architecture is the highest performance option, but operations are more labor-intensive than polling and hook architectures.






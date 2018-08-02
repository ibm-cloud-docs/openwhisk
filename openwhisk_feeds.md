---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Creating custom event provider feeds
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} supports an open API, where any user can expose an event producer service as a feed in a package. The following section describes architectural, and implementation options for providing your own custom feed.
{: shortdesc}

This material is intended for advanced {{site.data.keyword.openwhisk_short}} users who intend to publish their own feeds. Most {{site.data.keyword.openwhisk_short}} users can safely skip the following architecture section.

## Feed architecture

There are at least three architectural patterns for creating a feed: **Hooks**, **Polling**, and **Connections**.

### Hooks
In the *Hooks* pattern, a feed is set up by using a [webhook](https://en.wikipedia.org/wiki/Webhook) facility that is exposed by another service.   In this strategy, a webhook is configured on an external service to POST directly to a URL to fire a trigger. This method is by far the easiest and most attractive option for implementing low-frequency feeds.

<!-- The github feed is implemented using webhooks.  Put a link here when we have the open repo ready -->

### Polling
In the "Polling" pattern, a {{site.data.keyword.openwhisk_short}} action is arranged to poll an endpoint periodically to fetch new data. This pattern is relatively easy to build, but the frequencies of events are limited by the polling interval.

### Connections
In the "Connections"  pattern, a separate service maintains a persistent connection to a feed source. The connection-based implementation might interact with a service endpoint to by using long polling intervals, or to set up a push notification.

<!-- Our cloudant changes feed is connection based.  Put a link here to
an open repo -->

<!-- What is the foundation for the Message Hub feed? If it is "connections" then lets put a link here as well -->

## Difference between feed and trigger

Feeds and triggers are closely related, but technically distinct concepts.   

- {{site.data.keyword.openwhisk_short}} processes **events** that flow into the system.

- A **trigger** is technically a name for a class of events. Each event belongs to exactly one trigger; by analogy, a trigger resembles a *topic* in topic-based pub-sub systems. A **rule** *T -> A* means "whenever an event from trigger *T* arrives, invoke action *A* with the trigger payload.

- A **feed** is a stream of events that all belong to some trigger *T*. A feed is controlled by a **feed action**, which handles creating, deleting, pausing, and resuming the stream of events that comprise a feed. The feed action typically interacts with external services that produce the events, by using a REST API that manages notifications.

##  Implementing feed actions

The *feed action* is a normal {{site.data.keyword.openwhisk_short}} *action*, and accepts the following parameters:
* **lifecycleEvent**: One of 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE', or 'UNPAUSE'.
* **triggerName**: The fully qualified name of the trigger, which contains events that are produced from this feed.
* **authKey**: The Basic auth credentials of the {{site.data.keyword.openwhisk_short}} user who owns the trigger.

The feed action can also accept any other parameters that it needs to manage the feed. For example, the {{site.data.keyword.cloudant}} changes feed action expects to receive parameters that include *'dbname'*, *'username'*, and so on.

When the user creates a trigger from the CLI with the **--feed** parameter, the system automatically invokes the feed action with the appropriate parameters.

For example, assume that the user creates a **mycloudant** binding for the `cloudant` package with a username and password as bound parameters. When the user issues the following command from the CLI:
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

Then, under the covers the system does something equivalent to the following command:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

The feed action that is named *changes* takes these parameters, and is expected to perform whatever action is necessary to set up a stream of events from {{site.data.keyword.cloudant_short_notm}}. The feed action occurs by using the appropriate configuration, which is directed to the trigger *T*.

For the {{site.data.keyword.cloudant_short_notm}} *changes* feed, the action happens to talk directly to a *{{site.data.keyword.cloudant_short_notm}} trigger* service that is implemented with a connection-based architecture.

A similar feed action protocol occurs for `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` and `ibmcloud fn trigger get`.

## Implementing feeds with Hooks

It is easy to set up a feed by using a hook if the event producer supports a webhook/callback facility.

With this method, there is _no need_ to stand up any persistent service outside of {{site.data.keyword.openwhisk_short}}. All feed management happens naturally though stateless {{site.data.keyword.openwhisk_short}} *feed actions*, which negotiate directly with a third part webhook API.

When invoked with `CREATE`, the feed action simply installs a webhook for some other service, asking the remote service to POST notifications to the appropriate `fireTrigger` URL in {{site.data.keyword.openwhisk_short}}.

The webhook is directed to send notifications to a URL such as:

`POST /namespaces/{namespace}/triggers/{triggerName}`

The form with the POST request is interpreted as a JSON document that defines parameters on the trigger event. {{site.data.keyword.openwhisk_short}} rules pass these trigger parameters to any actions to fire as a result of the event.

## Implementing feeds with Polling

It is possible to set up an {{site.data.keyword.openwhisk_short}} *action* to poll a feed source entirely within {{site.data.keyword.openwhisk_short}}, without the need to stand up any persistent connections or external service.

For feeds where a webhook is not available, but do not need high-volume or low latency response times, polling is an attractive option.

To set up a polling-based feed, the feed action takes the following steps when called for `CREATE`:

1. The feed action sets up a periodic trigger (*T*) with the desired frequency, by using the `whisk.system/alarms` feed.
2. The feed developer creates a `pollMyService` action that polls the remote service and returns any new events.
3. The feed action sets up a *rule* *T -> pollMyService*.

This procedure implements a polling-based trigger entirely by using {{site.data.keyword.openwhisk_short}} actions, without any need for a separate service.

## Implementing feeds by using Connections

The previous two architectural choices are simple and easy to implement. However, if you want a high-performance feed, there is no substitute for persistent connections and long-polling or similar techniques.

Since {{site.data.keyword.openwhisk_short}} actions must be short-running, an action cannot maintain a persistent connection to a third party. Instead, you can stand up a separate service, called *provider services*, outside of {{site.data.keyword.openwhisk_short}} that run all the time. A provider service can maintain connections to third-party event sources that support long polling or other connection-based notifications.

The provider service has a REST API that allows the {{site.data.keyword.openwhisk_short}} *feed action* to control the feed. The provider service acts as a proxy between the event provider and {{site.data.keyword.openwhisk_short}}. When it receives events from the third party, it sends them on to {{site.data.keyword.openwhisk_short}} by firing a trigger.

The {{site.data.keyword.cloudant_short_notm}} *changes* feed is the canonical example as it stands up a `cloudanttrigger` service, which mediates between {{site.data.keyword.cloudant_short_notm}} notifications over a persistent connection, and {{site.data.keyword.openwhisk_short}} triggers.
<!-- TODO: add a reference to the open source implementation -->

The *alarm* feed is implemented with a similar pattern.

The connection-based architecture is the highest performance option, but imposes more overhead on operations that are compared to the polling and hook architectures.

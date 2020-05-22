---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-22"

keywords: event streams, package, messages, events, functions

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

# {{site.data.keyword.messagehub}}
{: #pkg_event_streams}

{{site.data.keyword.openwhisk}} provides a pre-installed packagefor publishing and consuming messages with {{site.data.keyword.messagehub}}.
{: shortdesc}

## Package options
{: #pkg_event_streams_options}

| Package | Availability | Description |
| --- | --- | --- |
| `/whisk.system/messaging` | Pre-installed | Publishing and consume messages with the native Kafka API. |
|  |  |  |

## {{site.data.keyword.messagehub}}
{: #eventstreams}

A pre-installed package that enables communication with [{{site.data.keyword.messagehub_full}}](/docs/EventStreams?topic=eventstreams-getting_started) instances for publishing and consuming messages by using the native high-performance Kafka API.
{: shortdesc}

### Setting up a {{site.data.keyword.messagehub}} package
{: #eventstreams_setup}

1. Create an instance of {{site.data.keyword.messagehub}} service under your current organization and space that you are using for {{site.data.keyword.openwhisk}}.

2. Verify that the topic you want to listen to is available in {{site.data.keyword.messagehub}} or create a new topic, for example, titled `mytopic`.

3. Refresh the packages in your namespace. The refresh automatically creates a package binding for the {{site.data.keyword.messagehub}} service instance that you created.

   ```
   ibmcloud fn package refresh
   ```
   {: pre}

   **Example output**
   ```
   created bindings:
   Message_Hub_Credentials-1
   ```
   {: screen}

4. List the packages in your namespace to show that your package binding is now available.
  
   ```
   ibmcloud fn package list
   ```
   {: pre}

   **Example output**
   ```
   packages
   /myOrg_mySpace/Message_Hub_Credentials-1 private
   ```
   {: screen}

Your package binding now contains the credentials that are associated with your {{site.data.keyword.messagehub}} instance.

### Setting up an {{site.data.keyword.messagehub}} package outside {{site.data.keyword.cloud_notm}}
{: #eventstreams_outside}

If you want to set up your {{site.data.keyword.messagehub}} outside of {{site.data.keyword.cloud_notm}}, you must manually create a package binding for your {{site.data.keyword.messagehub}} service. You need the {{site.data.keyword.messagehub}} service credentials and connection information.

Create a package binding that is configured for your {{site.data.keyword.messagehub}} service.

```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

### Producing messages to {{site.data.keyword.messagehub}}
{: #eventstreams_messages}

The `/messaging/messageHubProduce` action is deprecated and will be removed at a future date. It is already removed in the Tokyo region. To maintain optimal performance, migrate your usage of the `/messaging/messageHubProduce` action to use a persistent connection when data is produced to {{site.data.keyword.messagehub}}/Kafka.
{: tip}

To learn more about producing messages, check out the [Event Streams documentation](/docs/EventStreams?topic=eventstreams-producing_messages#producing_messages).

References
- [{{site.data.keyword.messagehub_full}}](https://www.ibm.com/cloud/event-streams){: external}
- [Apache Kafka](https://kafka.apache.org){: external}

## Event Streams events source
{: #eventstreams_events}

You can create a trigger that reacts when messages are posted to an {{site.data.keyword.messagehub_full}} instance by using feeds. Learn how to create {{site.data.keyword.messagehub}} triggers with or without {{site.data.keyword.cloud}}, listen for messages, and handle batched messages.
{: shortdesc}

## {{site.data.keyword.messagehub}} package
{: #eventstreams_pkg}

The `/messaging/messageHubProduce` action is deprecated and will be removed at a future date. It is already removed in the Tokyo region. To maintain optimal performance, migrate your usage of the `/messaging/messageHubProduce` action to use a persistent connection when data is produced to {{site.data.keyword.messagehub}}/Kafka.
{: deprecated}

This package enables communication with [{{site.data.keyword.messagehub}}](https://www.ibm.com/cloud/event-streams-for-cloud){: external} instances for publishing and consuming messages by using the native high-performance Kafka API.

### Creating a trigger that listens to an {{site.data.keyword.messagehub}} instance
{: #eventstreams_trigger}

In order to create a trigger that reacts when messages are posted to an {{site.data.keyword.messagehub}} instance, you need to use the feed that is named `/messaging/messageHubFeed`. The feed action supports the following parameters:

| Name | Type | Description |
| --- | --- | --- |
| `kafka_brokers_sasl` | JSON array of strings | This parameter is an array of `<host>:<port>` strings that comprise the brokers in your {{site.data.keyword.messagehub}} instance. |
| `user` | String | Your {{site.data.keyword.messagehub}} username. |
| `password` | String | Your {{site.data.keyword.messagehub}} password. |
| `topic` | String | The topic that you would like the trigger to listen to. |
| `kafka_admin_url` | URL string | The URL of the {{site.data.keyword.messagehub}} admin REST interface. |
| `isJSONData` | Boolean (Optional - default=false) | When set to `true`, the provider attempts to parse the message value as JSON before passing it along as the trigger payload. |
| `isBinaryKey` | Boolean (Optional - default=false) | When set to `true`, the provider encodes the key value as Base64 before passing it along as the trigger payload. |
| `isBinaryValue` | Boolean (Optional - default=false) | When set to `true`, the provider encodes the message value as Base64 before passing it along as the trigger payload. |

While this list of parameters can seem daunting, they can be automatically set for you by using the `ibmcloud fn package refresh` CLI plug-in command.

1. Create an instance of {{site.data.keyword.messagehub}} service under your current organization and space that you are using for {{site.data.keyword.openwhisk}}.

2. Verify that the topic you want to listen to is available in {{site.data.keyword.messagehub}} or create a new topic, for example, `mytopic`.

3. Refresh the packages in your Namespace. The refresh automatically creates a package binding for the {{site.data.keyword.messagehub}} service instance that you created.

   ```
   ibmcloud fn package refresh
   ```
   {: pre}

   **Example output**

   ```
   created bindings:
   Message_Hub_Credentials-1
   ```
   {: screen}

4. List the packages in your Namespace to show that your package binding is now available.
  
   ```
   ibmcloud fn package list
   ```
   {: pre}

   **Example output**

   ```
   packages
   /myOrg_mySpace/Message_Hub_Credentials-1 private
   ```
   {: screen}

   Your package binding now contains the credentials that are associated with your {{site.data.keyword.messagehub}} instance.

5. Now all you need to do is create a trigger that is fired when new messages are posted to your {{site.data.keyword.messagehub}} topic.

   ```
   ibmcloud fn trigger create MyMessageHubTrigger -f /myOrg_mySpace/Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
   ```
   {: pre}

### Creating a trigger for an {{site.data.keyword.messagehub}} package outside {{site.data.keyword.cloud_notm}}
{: #eventstreams_trigger_outside}

If you want to set up your {{site.data.keyword.messagehub}} outside of {{site.data.keyword.cloud_notm}}, you must manually create a package binding for your {{site.data.keyword.messagehub}} service. You need the {{site.data.keyword.messagehub}} service credentials and connection information.

1. Create a package binding that is configured for your {{site.data.keyword.messagehub}} service.

   ```
   ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "
   [\"broker-1-9eyy8dkv3rrj0wdn.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093\", \"broker-1-9eyy8dkv3rrj0wdn.kafka.svc02.us-south.eventstreams.cloud.ibm.com:9093\", \"broker-1-9eyy8dkv3rrj0wdn.kafka.svc03.us-south.eventstreams.cloud.ibm.com:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://9eyy8dkv3rrj0wdn.svc01.us-south.eventstreams.cloud.ibm.com
   ```
   {: pre}

2. Now you can create a trigger by using your new package that is fired when new messages are posted to your {{site.data.keyword.messagehub}} topic.

   ```
   ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
   ```
   {: pre}

### Listening for messages
{: #eventstreams_listen_messages}

Once a trigger is created, the system monitors the specified topic in your messaging service. When new messages are posted, the trigger is fired.

The payload of that trigger contains a `messages` field, which is an array of messages that are posted from the last time the trigger is fired. Each message object in the array contains the following fields:
- `topic`
- `partition`
- `offset`
- `key`
- `value`

In Kafka terms, the fields are self-evident. However, `key` has a feature that is called `isBinaryKey` that allows the `key` to transmit binary data. Additionally, the `value` requires special consideration. Fields `isJSONData` and `isBinaryValue` are available to handle JSON and binary messages. These fields, `isJSONData`, and `isBinaryValue`, cannot be used together.

As an example, if `isBinaryKey` was set to `true` when the trigger was created, the `key` is encoded as a Base64 string when returned from they payload of a fired trigger.

If a `key` of `Some key` is posted with `isBinaryKey` set to `true`, the trigger payload resembles the following example:
```json
{
    "messages": [
        {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```
{: codeblock}

If the `isJSONData` parameter was set to `false` (or not set at all) when the trigger was created, the `value` field is the raw value of the posted message. However, if `isJSONData` was set to `true` when the trigger was created, the system attempts to parse this value as a JSON object, on a best-effort basis. If parsing is successful, then the `value` in the trigger payload is the resulting JSON object.

If a message of `{"title": "Some string", "amount": 5, "isAwesome": true}` is posted with `isJSONData` set to `true`, the trigger payload might look something like the following example:
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": {
          "amount": 5,
          "isAwesome": true,
          "title": "Some string"
      }
    }
  ]
}
```
{: codeblock}

However, if the same message content is posted with `isJSONData` set to `false`, the trigger payload would look like the following example:
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```
{: codeblock}

Similar to `isJSONData`, if `isBinaryValue` was set to `true` during trigger creation, the resultant `value` in the trigger payload is encoded as a Base64 string.

If a `value` of `Some data` is posted with `isBinaryValue` set to `true`, the trigger payload might look something like the following example:
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```
{: codeblock}

If the same message is posted without `isBinaryData` set to `true`, the trigger payload would resemble the following example:
```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```
{: codeblock}

### Messages are batched
{: #eventstreams_batched}

Notice that the trigger payload contains an array of messages. If these messages are produced to your messaging system quickly, the feed attempts to batch up the posted messages into a single firing of your trigger. Batch processing allows the messages to be posted to your trigger more rapidly and efficiently.

Keep in mind when you are coding actions that are fired by your trigger, that the number of messages in the payload is technically unbounded, but is always greater than 0. See the following example of a batched message (note the change in the *offset* value):
```json
{
  "messages": [
      {
        "partition": 0,
        "key": null,
        "offset": 100,
        "topic": "mytopic",
        "value": {
            "amount": 5
        }
      },
      {
        "partition": 0,
        "key": null,
        "offset": 101,
        "topic": "mytopic",
        "value": {
            "amount": 1
        }
      },
      {
        "partition": 0,
        "key": null,
        "offset": 102,
        "topic": "mytopic",
        "value": {
            "amount": 999
        }
      }
  ]
}
```

## References
{: #message_references}
- [{{site.data.keyword.messagehub}}](https://www.ibm.com/cloud/event-streams){: external}
- [Apache Kafka](https://kafka.apache.org){: external}

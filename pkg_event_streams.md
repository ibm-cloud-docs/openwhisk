---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-10"

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

{{site.data.keyword.openwhisk}} provides a pre-installed package for publishing and consuming messages with {{site.data.keyword.messagehub}}.
{: shortdesc}

## Package entities
{: #pkg_event_streams_options}

| Package |Type | Description |
| --- | --- | --- |
| `/whisk.system/messaging` | Pre-installed package | Publishing and consume messages with the native Kafka API. |
| `/messaging/messageHubProduce` | Action | Deprecated |
| `/messaging/messageHubFeed` | Feed | Reacts when messages are posted to an {{site.data.keyword.messagehub}} instance |

The `/messaging/messageHubProduce` action is deprecated. To maintain optimal performance, migrate your usage of the `/messaging/messageHubProduce` action to use a persistent connection when data is produced to {{site.data.keyword.messagehub}}/Kafka. The `/messaging/messageHubProduce` action is not available in Tokyo or Sydney regions.
{: tip}

To learn more about producing messages, check out the [Event Streams documentation](/docs/EventStreams?topic=EventStreams-producing_messages#producing_messages).

## Binding the `/whisk.system/messaging` package to your {{site.data.keyword.messagehub}} instance
{: #event_streams_binding}

If you're using {{site.data.keyword.openwhisk}} from the {{site.data.keyword.cloud_notm}}, you can use the {{site.data.keyword.openwhisk}} CLI plug-in to bind a service to an action or package. If you do not bind your service, you must pass your credentials each time you use the action or package.
{: #shortdesc}

**Before you begin**

You must have an instance of {{site.data.keyword.messagehub}}. To create an instance, see [Event Streams documentation](/docs/EventStreams?topic=EventStreams-getting_started).

1. Create a `/whisk.system/messaging` package binding that is configured for your {{site.data.keyword.messagehub}} account. In this example, the package name is `MyEventStreamBind`.

  ```
  ibmcloud fn package bind /whisk.system/messaging MyEventStreamBind
  ```
  {: pre}

2. Verify that the package binding exists.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Example output**

  ```
  packages
  /<namespace_ID>/MyEventStreamBind
  ```
  {: screen}

3. Get the name of the service instance that you want to bind to an action or package.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Example output**
    
    ```
    Name               Location   State    Type
    EventStreams-0s    us-south   active   service_instance
    ```
    {: screen}

4. Get the name of the credentials that are defined for the service instance you got in the previous step.

    ```
    ibmcloud resource service-keys --instance-name EventStreams-0s
    ```
    {: pre}

    **Example output**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}
    
    You can create service keys by using the [`ibmcloud resource service-keys`](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_keys).
    {: tip}

5. Bind the service to the package that you created in the first step. In the example, this package is called `MyEventStreamBind`.

    ```
    ibmcloud fn service bind messagehub MyEventStreamBind --instance EventStreams-0s --keyname 'Service credentials-1'
    ```
    {: pre}
    
    **Example output**
    
    ```
    Credentials 'Service credentials-1' from 'messagehub' service instance 'EventStreams-0s' bound to 'MyEventStreamBind'.
    ```
    {: screen}

6. Verify that the credentials are successfully bound.

    ```
    ibmcloud fn package get MyEventStreamBind parameters
    ```
    {: pre}

    **Example output**

    ```
    ok: got package MyEventStreamBind
    ...
      "parameters": [
        {
            "key": "bluemixServiceName",
            "value": "messagehub"
        },
        {
            "key": "endpoint",
            "value": "openwhisk.ng.bluemix.net"
        },
        {
            "key": "__bx_creds",
            "value": {
                "messagehub": {
                    "api_key": "2RxXWBVUdR5-8GDXrYhpm7zMCB5dNtJ1vB3YfaI3o7",
                    "apikey": "2RxXWBVUdR5-8GDXrYhpm7zMCB5dNtJ1vB3YfaI3o7",
                    "credentials": "Service credentials-1",
                    "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:messagehub:us-south:a/6ef045fb2b43266cfe8e6745dd2ec098:1244a768-a7e5-3c48-a6d2-1ab7c8b63d57::",
                    "iam_apikey_name": "auto-generated-apikey-dc334c91-3503-480f-a812-49bff8bedc60",
                    "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
                    "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/6ef045fd2b43266cfe8e6388dd2ec098::serviceid:ServiceId-e2e58c4f-9bc2-4bbe-cbc9-0d679e7a0d85",
                    "instance": "EventStreams-0s",
                    "instance_id": "1244a768-a7e5-3c48-a6d2-1ab7c8b63d57",
                    "kafka_admin_url": "https://5syh8qrs9rpj0zds.svc01.us-south.eventstreams.cloud.ibm.com",
                    "kafka_brokers_sasl": [
                        "broker-1-5syh8qrs9rpj0zds.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093",
                        "broker-4-5syh8qrs9rpj0zds.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093",
                        "broker-3-5syh8qrs9rpj0zds.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093",
                        "broker-2-5syh8qrs9rpj0zds.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093",
                        "broker-5-5syh8qrs9rpj0zds.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093",
                        "broker-0-5syh8qrs9rpj0zds.kafka.svc01.us-south.eventstreams.cloud.ibm.com:9093"
                    ],
                    "kafka_http_url": "https://5syh8qrs9rpj0zds.svc01.us-south.eventstreams.cloud.ibm.com",
                    "password": "2RxXBVUdR5-9GDXrYhpmzMCB5dNtJ1vB3YfaI3o7",
                    "user": "token"
                }
            }
        }
    ]
    
    ```
    {: screen}

In this example, the credentials for the {{site.data.keyword.messagehub}} service belong to a parameter named `__bx_creds`.

### Setting up an {{site.data.keyword.messagehub}} package outside {{site.data.keyword.cloud_notm}}
{: #eventstreams_outside}

If you want to set up your {{site.data.keyword.messagehub}} outside of {{site.data.keyword.cloud_notm}}, you must manually create a package binding for your {{site.data.keyword.messagehub}} service. You need the {{site.data.keyword.messagehub}} service credentials and connection information.

Create a package binding that is configured for your {{site.data.keyword.messagehub}} service.

```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Creating a trigger that listens to an {{site.data.keyword.messagehub}} instance
{: #eventstreams_trigger}

To create a trigger that reacts when messages are posted to an {{site.data.keyword.messagehub}} instance, use the feed that is named `/messaging/messageHubFeed`. The feed action supports the following parameters:

| Name | Type | Description |
| --- | --- | --- |
| `kafka_brokers_sasl` | JSON array of strings | This parameter is an array of `<host>:<port>` strings that comprise the brokers in your {{site.data.keyword.messagehub}} instance. |
| `user` | String | Your {{site.data.keyword.messagehub}} username. |
| `password` | String | Your {{site.data.keyword.messagehub}} password. |
| `topic` | String | The topic that you want the trigger to listen to. |
| `kafka_admin_url` | URL string | The URL of the {{site.data.keyword.messagehub}} admin REST interface. |
| `isJSONData` | Boolean (Optional - default=false) | When set to `true`, the provider attempts to parse the message value as JSON before passing it along as the trigger payload. |
| `isBinaryKey` | Boolean (Optional - default=false) | When set to `true`, the provider encodes the key value as Base64 before passing it along as the trigger payload. |
| `isBinaryValue` | Boolean (Optional - default=false) | When set to `true`, the provider encodes the message value as Base64 before passing it along as the trigger payload. |

To create the trigger:

1. [Bind the `/whisk.system/messaging` package](#event_streams_binding) to your {{site.data.keyword.messagehub}} instance. 

2. Create a trigger that is fired when new messages are posted to your {{site.data.keyword.messagehub}} topic. Replace `<namespace_ID>` with your namespace ID, `<binding>` with your package binding name, and `<mytopic>` with the topic in your {{site.data.keyword.messagehub}} instance that you want the trigger to listen to.

   ```
   ibmcloud fn trigger create MyMessageHubTrigger -f /<namespace_ID>/<binding>/messageHubFeed -p topic <mytopic>
   ```
   {: pre}
   
   To find the path to the `messageHubFeed` trigger, run `ibmcloud fn package list | grep <binding>`, where `<binding>` is the name of your package binding.
   {: tip}

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

After a trigger is created, the system monitors the specified topic in your messaging service. When new messages are posted, the trigger is fired.

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

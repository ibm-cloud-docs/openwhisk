---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub events source
{: #openwhisk_catalog_message_hub}

You can create a Trigger that reacts when messages are posted to a Message Hub instance by using Feeds. Learn how to create Message Hub Triggers with or without {{site.data.keyword.Bluemix}}, listen for messages, and handle batched messages.
{: shortdesc}

## Message Hub package 

The `/messaging/messageHubProduce` Action is deprecated and will be removed at a future date. To maintain optimal performance, migrate your usage of the `/messaging/messageHubProduce` Action to use a persistent connection when data is produced to Message Hub/Kafka.
{: tip}

This package enables communication with [Message Hub](https://developer.ibm.com/messaging/message-hub) instances for publishing and consuming messages by using the native high-performance Kafka API. For more information about the Message Hub package, how to set it up, and how to produce messages see the [Message Hub package](./messagehub_actions.html) topic.

## Creating a Trigger that listens to an IBM Message Hub instance
{: #create_message_hub_trigger}

In order to create a Trigger that reacts when messages are posted to a Message Hub instance, you need to use the Feed that is named `/messaging/messageHubFeed`. The Feed Action supports the following parameters:

|Name|Type|Description|
|---|---|---|
|kafka_brokers_sasl|JSON Array of Strings|This parameter is an array of `<host>:<port>` strings that comprise the brokers in your Message Hub instance|
|user|String|Your Message Hub username.|
|password|String|Your Message Hub password.|
|topic|String|The topic that you would like the Trigger to listen to.|
|kafka_admin_url|URL String|The URL of the Message Hub admin REST interface.|
|isJSONData|Boolean (Optional - default=false)|When set to `true`, the provider attempts to parse the message value as JSON before passing it along as the Trigger payload.|
|isBinaryKey|Boolean (Optional - default=false)|When set to `true`, the provider encodes the key value as Base64 before passing it along as the Trigger payload.|
|isBinaryValue|Boolean (Optional - default=false)|When set to `true`, the provider encodes the message value as Base64 before passing it along as the Trigger payload.|

While this list of parameters can seem daunting, they can be automatically set for you by using the `ic wsk package refresh` CLI plug-in command.

1. Create an instance of Message Hub service under your current organization and space that you are using for {{site.data.keyword.openwhisk}}.

2. Verify that the topic you want to listen to is available in Message Hub or create a new topic, for example, **mytopic**.

3. Refresh the packages in your Namespace. The refresh automatically creates a package binding for the Message Hub service instance that you created.
  ```
  ic wsk package refresh
  ```
  {: pre}

  **Output:**
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. List the packages in your Namespace to show that your package binding is now available.
  ```
  ic wsk package list
  ```
  {: pre}

  **Output:**
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Your package binding now contains the credentials that are associated with your Message Hub instance.

5. Now all you need to do is create a Trigger that is fired when new messages are posted to your Message Hub topic.
  ```
  ic wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Creating a Trigger for a Message Hub package outside {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_trigger_outside}

If you want to set up your Message Hub outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Message Hub service. You need the Message Hub service credentials and connection information.

1. Create a package binding that is configured for your Message Hub service.
  ```
  ic wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Now you can create a Trigger by using your new package that is fired when new messages are posted to your Message Hub topic.
  ```
  ic wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## Listening for messages
{: #message_hub_listen}

Once a Trigger is created, the system monitors the specified topic in your messaging service. When new messages are posted, the Trigger is fired.

The payload of that Trigger contains a `messages` field, which is an array of messages that are posted from the last time the Trigger is fired. Each message object in the array contains the following fields:
- topic
- partition
- offset
- key
- value

In Kafka terms, the fields are self-evident. However, `key` has a feature that is called `isBinaryKey` that allows the `key` to transmit binary data. Additionally, the `value` requires special consideration. Fields `isJSONData` and `isBinaryValue` are available to handle JSON and binary messages. These fields, `isJSONData`, and `isBinaryValue`, cannot be used together.

As an example, if `isBinaryKey` was set to `true` when the Trigger was created, the `key` is encoded as a Base64 string when returned from they payload of a fired Trigger.

If a `key` of `Some key` is posted with `isBinaryKey` set to `true`, the Trigger payload resembles the following example:
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

If the `isJSONData` parameter was set to `false` (or not set at all) when the Trigger was created, the `value` field is the raw value of the posted message. However, if `isJSONData` was set to `true` when the Trigger was created, the system attempts to parse this value as a JSON object, on a best-effort basis. If parsing is successful, then the `value` in the Trigger payload is the resulting JSON object.

If a message of `{"title": "Some string", "amount": 5, "isAwesome": true}` is posted with `isJSONData` set to `true`, the Trigger payload might look something like the following example:
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

However, if the same message content is posted with `isJSONData` set to `false`, the Trigger payload would look like the following example:
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

Similar to `isJSONData`, if `isBinaryValue` was set to `true` during Trigger creation, the resultant `value` in the Trigger payload is encoded as a Base64 string.

If a `value` of `Some data` is posted with `isBinaryValue` set to `true`, the Trigger payload might look something like the following example:
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

If the same message is posted without `isBinaryData` set to `true`, the Trigger payload would resemble the following example:
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
Notice that the Trigger payload contains an array of messages. If these messages are produced to your messaging system quickly, the Feed attempts to batch up the posted messages into a single firing of your Trigger. Batch processing allows the messages to be posted to your Trigger more rapidly and efficiently.

Keep in mind when coding Actions that are fired by your Trigger, that the number of messages in the payload is technically unbounded, but is always greater than 0. See the following example of a batched message (note the change in the *offset* value):
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

## Examples
{: #examples}

### Integrating OpenWhisk with IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage, and IBM Data Science Experience
Example that integrates OpenWhisk with IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage, IBM Data Science Experience (Spark) service can be [found here](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## References
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)

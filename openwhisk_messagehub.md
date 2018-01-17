---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Using the Message Hub package
{: #openwhisk_catalog_message_hub}

A package that enables communication with [Message Hub](https://developer.ibm.com/messaging/message-hub) instances for publishing and consuming messages by using the native high-performance Kafka API.
{: shortdesc}

## Creating a Trigger that listens to an IBM MessageHub instance
{: #openwhisk_catalog_message_hub_trigger}

In order to create a Trigger that reacts when messages are posted to a Message Hub instance, you need to use the Feed that is named `/messaging/messageHubFeed`. This Feed Action supports the following parameters:

|Name|Type|Description|
|---|---|---|
|kafka_brokers_sasl|JSON Array of Strings|This parameter is an array of `<host>:<port>` strings that comprise the brokers in your Message Hub instance|
|user|String|Your Message Hub user name|
|password|String|Your Message Hub password|
|topic|String|The topic that you would like the Trigger to listen to|
|kafka_admin_url|URL String|The URL of the Message Hub admin REST interface|
|isJSONData|Boolean (Optional - default=false)|When set to `true`, the provider attempts to parse the message value as JSON before passing it along as the Trigger payload.|
|isBinaryKey|Boolean (Optional - default=false)|When set to `true`, the provider encodes the key value as Base64 before passing it along as the Trigger payload.|
|isBinaryValue|Boolean (Optional - default=false)|When set to `true`, the provider encodes the message value as Base64 before passing it along as the Trigger payload.|

While this list of parameters can seem daunting, they can be automatically set for you by using the package refresh CLI command:

1. Create an instance of Message Hub service under your current organization and space that you are using for OpenWhisk.

2. Verify that the topic you want to listen to is available in Message Hub or create a new topic, for example, `mytopic`.

3. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Message Hub service instance that you created.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  Your package binding now contains the credentials that are associated with your Message Hub instance.

4. Now all you need to do is create a Trigger that is fired when new messages are posted to your Message Hub topic.

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Setting up a Message Hub package outside {{site.data.keyword.Bluemix_notm}}

If you want to set up your Message Hub outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Message Hub service. You need the Message Hub service credentials and connection information.

1. Create a package binding that is configured for your Message Hub service.

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Now you can create a Trigger by using your new package that is fired when new messages are posted to your Message Hub topic.

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## Listening for messages
{: #openwhisk_catalog_message_hub_listen}

Once a Trigger is created, the system monitors the specified topic in your messaging service. When new messages are posted, the Trigger is fired.

The payload of that Trigger contains a `messages` field, which is an array of messages that are posted from the last time the Trigger was fired. Each message object in the array contains the following fields:
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

## Producing messages to Message Hub
If you would like to use an OpenWhisk Action to conveniently produce a message to Message Hub, you can use the `/messaging/messageHubProduce` Action. This Action takes the following parameters:

|Name|Type|Description|
|---|---|---|
|kafka_brokers_sasl|JSON Array of Strings|This parameter is an array of `<host>:<port>` strings that comprise the brokers in your Message Hub instance|
|user|String|Your Message Hub user name|
|password|String|Your Message Hub password|
|topic|String|The topic that you would like the Trigger to listen to|
|value|String|The value for the message you would like to produce|
|key|String (Optional)|The key for the message you would like to produce|

While the first three parameters can be automatically bound by using `wsk package refresh`, see the following example that invokes the Action with all necessary parameters:

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## Examples

### Integrating OpenWhisk with IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage, and IBM Data Science Experience
Example that integrates OpenWhisk with IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage, IBM Data Science Experience (Spark) service can be [found here](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## References
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)

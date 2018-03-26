---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Message Hub
{: #openwhisk_catalog_message_hub}

A package that enables communication with [Message Hub](https://developer.ibm.com/messaging/message-hub) instances for publishing and consuming messages by using the native high-performance Kafka API.
{: shortdesc}

## Setting up a Message Hub package using {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Create an instance of Message Hub service under your current organization and space that you are using for {{site.data.keyword.openwhisk}}.

2. Verify that the topic you want to listen to is available in Message Hub or create a new topic, for example, titled **mytopic**.

3. Refresh the packages in your Namespace. The refresh automatically creates a package binding for the Message Hub service instance that you created.
  ```
  bx wsk package refresh
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
  bx wsk package list
  ```
  {: pre}

  **Output:**
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Your package binding now contains the credentials that are associated with your Message Hub instance.

## Setting up a Message Hub package outside {{site.data.keyword.Bluemix_notm}}

If you want to set up your Message Hub outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Message Hub service. You need the Message Hub service credentials and connection information.

Create a package binding that is configured for your Message Hub service.
```
bx wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Listening for messages using events

For detailed information about how to use Triggers in Message Hub to listen for messages, see the following
[Message hub events source](./openwhisk_messagehub.html) topic where the following tasks are covered:
* [Creating a Trigger that listens to an IBM Message Hub instance](./openwhisk_messagehub.html#create_message_hub_trigger)
* [Creating a Trigger for a Message Hub package outside {{site.data.keyword.Bluemix_notm}}](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [Listening for messages](./openwhisk_messagehub.html#message_hub_listen)
* [Examples](./openwhisk_messagehub.html#)

## Producing messages to Message Hub
{: #producing_messages}

If you would like to use a {{site.data.keyword.openwhisk_short}} Action to conveniently produce a message to Message Hub, you can use the `/messaging/messageHubProduce` Action. This Action takes the following parameters:

|Name|Type|Description|
|---|---|---|
|kafka_brokers_sasl|JSON Array of Strings|This parameter is an array of `<host>:<port>` strings that comprise the brokers in your Message Hub instance.|
|user|String|Your Message Hub username.|
|password|String|Your Message Hub password.|
|topic|String|The topic that you would like the Trigger to listen to.|
|value|String|The value for the message you would like to produce.|
|key|String (Optional)|The key for the message you would like to produce.|

While the first three parameters can be automatically bound by using `bx wsk package refresh`, see the following example that invokes the Action with all necessary parameters:
```
bx wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## References
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)

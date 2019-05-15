---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: triggers, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Creating triggers for events
{: #triggers}

A trigger is a declaration that you want to react to a certain type of event, whether from a user or by an event source.
{: shortdesc}

The following are examples of triggers.
- Location update events
- Document uploads to a website
- Incoming emails



## Creating triggers from the CLI
{: #triggers_create}


1. Create the trigger. Triggers must be created directly within a namespace and can't be created inside packages.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    Example output:
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Verify that the trigger is created.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Example output:
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



Next, you can [test the trigger](/docs/openwhisk?topic=cloud-functions-test#test_triggers) or [create a rule](/docs/openwhisk?topic=cloud-functions-rules) to associate the trigger with an action.



## Difference between feed and trigger
{: #triggers_difference}

Feeds and triggers are closely related, but technically distinct concepts.

- {{site.data.keyword.openwhisk_short}} processes **events** that flow into the system.

- A **trigger** is a name for a class of events. Each event belongs to exactly one trigger; by analogy, a trigger resembles a topic in topic-based pub-sub systems. A **rule** means that whenever an event from trigger arrives, invoke action with the trigger payload.

- A **feed** is a convenient way to configure an external event source to fire trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. A feed is a stream of events that all belong to some trigger. Pre-installed packages, installable packages, and you own custom packages might contain feeds.  A feed is controlled by a **feed action**, which handles creating, deleting, pausing, and resuming the stream of events that comprise a feed. The feed action typically interacts with external services that produce the events, by using a REST API that manages notifications.

Examples of feeds:
- An {{site.data.keyword.cloudant}} data change feed that fires a trigger event each time a document in a database is added or modified
- A Git feed that fires a trigger event for every commit to a Git repository



## Creating a trigger for a feed
{: #triggers_feeds}

This example shows how to use a feed in the Alarms package to fire a trigger once a minute, and how to use a rule to invoke an action once a minute.

1. Get a description list of the entities in the `/whisk.system/alarms` package.

    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    Example output:
    ```
    package /whisk.system/alarms
     feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. Get a description of the feed in the `/whisk.system/alarms` package to see the parameters that you can use.

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Example output:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  The `/whisk.system/alarms/alarm` feed takes two parameters:
  - `cron`: A crontab specification of when to fire the trigger.
  - `trigger_payload`: The payload parameter value to set in each trigger event.

2. Create a trigger that fires every one minute.
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Example output:
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. Create an app. Example `hello.js`:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Create an action.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. Create a rule that invokes the `hello` action every time the `everyOneMinute` trigger fires.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  Example output:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Check that the action is being invoked by polling for activation logs.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  You can see that the activations occur every minute for the trigger, the rule, and the action. The action receives the parameters `{"name":"Mork", "place":"Ork"}` on every invocation.



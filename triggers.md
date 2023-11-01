---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: triggers, serverless, functions, events, actions, feed

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Creating triggers for events
{: #triggers}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

In {{site.data.keyword.openwhisk}}, a trigger is a declaration that you want to react to a certain type of event, whether from a user or by an event source.
{: shortdesc}

The following are examples of triggers.

- Location update events
- Document uploads to a website.
- Incoming email

## Creating triggers from the CLI
{: #triggers_create}

1. Create the trigger. Triggers must be created directly within a namespace and can't be created inside packages.

    ```sh
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    The following example shows possible output from the previous command.

    ```sh
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Verify that the trigger is created.

    ```sh
    ibmcloud fn trigger list
    ```
    {: pre}

    The following example shows possible output from the previous command.

    ```sh
    triggers
    /<namespace_ID>/<TRIGGER_NAME>                            private
    ```
    {: screen}

Next, you can [test the trigger](/docs/openwhisk?topic=openwhisk-test#test_triggers) or [create a rule](/docs/openwhisk?topic=openwhisk-rules) to associate the trigger with an action.

## Difference between feed and trigger
{: #triggers_difference}

Feeds and triggers are closely related, but technically distinct concepts.

- {{site.data.keyword.openwhisk_short}} processes **events** that flow into the system.

- A **trigger** is a name for a class of events. Each event belongs to exactly one trigger; by analogy, a trigger resembles a topic in topic-based pub-sub systems. A **rule** is used to indicate that whenever an event from trigger arrives, invoke action with the trigger payload.

- A **feed** is a convenient way to configure an external event source to fire trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. A feed is a stream of events that all belong to some trigger. Pre-installed packages, installable packages, and your own custom packages might contain feeds.  A feed is controlled by a **feed action**, which handles creating, deleting, pausing, and resuming the stream of events that comprise a feed. The feed action typically interacts with external services that produce the events, by using a REST API that manages notifications.

Examples of feeds:

- An {{site.data.keyword.cloudant}} data change feed that fires a trigger event each time a document in a database is added or modified.
- A Git feed that fires a trigger event for every commit to a Git repository.

## Creating a trigger for a feed
{: #triggers_feeds}

This example shows how to use a feed in the Alarms package to fire a trigger once a minute, and how to use a rule to invoke an action once a minute.

1. Get a description list of the entities in the `/whisk.system/alarms` package.

    ```sh
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    The following example shows possible output from the previous command.

    ```sh
    package /whisk.system/alarms
        feed   /whisk.system/alarms/alarm
    ```
    {: screen}

2. Get a description of the feed in the `/whisk.system/alarms` package to see the parameters that you can use.

    ```sh
    ibmcloud fn action get --summary /whisk.system/alarms/alarm
    ```
    {: pre}

    The following example shows possible output from the previous command.

    ```sh
    action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
        (params: cron trigger_payload)
    ```
    {: screen}

    The `/whisk.system/alarms/alarm` feed takes two parameters:
    - `cron`: A crontab specification of when to fire the trigger.
    - `trigger_payload`: The payload parameter value to set in each trigger event.

3. Create a trigger that fires every minute.

    ```sh
    ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
    ```
    {: pre}

    The following example shows possible output from the previous command.

    ```sh
    ok: created trigger feed everyOneMinute
    ```
    {: screen}

4. Create an app. Example `hello.js`:

    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

5. Create an action.

    ```sh
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

6. Create a rule that invokes the `hello` action every time the `everyOneMinute` trigger fires.

    ```sh
    ibmcloud fn rule create myRule everyOneMinute hello
    ```
    {: pre}

    The following example shows possible output from the previous command.

    ```sh
    ok: created rule myRule
    ```
    {: screen}

7. Check that the action is being invoked by polling for activation logs.

    ```sh
    ibmcloud fn activation poll
    ```
    {: pre}

    You can see that the activations occur every minute for the trigger, the rule, and the action. The action receives the parameters `{"name":"Mork", "place":"Ork"}` on every invocation.



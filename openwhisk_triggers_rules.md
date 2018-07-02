---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Responding to events with triggers and rules
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk}} triggers and rules bring event-driven capabilities to the platform. Events from external and internal event sources are channeled through a trigger, and rules allow your actions to react to these events.
{: shortdesc}

## General concepts
{: #definitions}

### Triggers
{: #openwhisk_triggers_create}

Triggers are a named channel for a class of events.
{: shortdesc}

A trigger is a declaration that you want to react to a certain type of event, whether from a user or by an event source. The following are examples of triggers.
- A trigger of location update events
- A trigger of document uploads to a website
- A trigger of incoming emails

Triggers can be fired, or activated, by using a dictionary of key-value pairs. Sometimes this dictionary is referred to as the event. Triggers can be explicitly fired by a user or fired on behalf of a user by an external event source. As with actions, each firing of a trigger that is associated with a rule results in an activation ID. A trigger that isn't associated with a rule has no visible effect when it is fired.

A feed is a convenient way to configure an external event source to fire trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. The following are examples of feeds.
- An {{site.data.keyword.cloudant}} data change feed that fires a trigger event each time a document in a database is added or modified
- A Git feed that fires a trigger event for every commit to a Git repository

### Rules
{: #openwhisk_rules_use}

A rule associates a trigger with an action.
{: shortdesc}

Every time the trigger fires, the rule uses the trigger event as input and invokes the associated action. With the appropriate set of rules, it's possible for a single trigger event to invoke multiple actions, or for an action to be invoked as a response to events from multiple triggers.

For example, consider a system with the following actions.
- `classifyImage` - An action that detects the objects in an image and classifies them.
- `thumbnailImage` - An action that creates a thumbnail version of an image.

Also suppose that two event sources are firing the following triggers.
- `newTweet` - A trigger that is fired when a new tweet is posted.
- `imageUpload` - A trigger that is fired when an image is uploaded to a website.

You can set up rules so that a single trigger event invokes multiple actions, and have multiple triggers invoke the same action.
- `newTweet -> classifyImage` rule
- `imageUpload -> classifyImage` rule
- `imageUpload -> thumbnailImage` rule

The three rules establish the following behavior.
- Images in both tweets are classified.
- Uploaded images are classified
- A thumbnail version is generated.

## Creating triggers to channel events
{: #openwhisk_triggers_fire}

The following steps show you how to create an example trigger to send user location updates, and how to manually fire the trigger.

1. Create the trigger. Triggers must be created directly within a namespace and can't be created inside packages.
    ```
    ibmcloud wsk trigger create locationUpdate
    ```
    {: pre}

    Example output:
    ```
    ok: created trigger locationUpdate
    ```
    {: screen}

2. Verify that the trigger is created.
    ```
    ibmcloud wsk trigger list
    ```
    {: pre}

    Example output:
    ```
    triggers
    /someNamespace/locationUpdate                            private
    ```
    {: screen}
    The trigger serves as a named channel in which events can be fired.

3. Fire a trigger event.
    ```
    ibmcloud wsk trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Because there is no rule associated with this trigger, the passed parameters are not used as input by any action. Example output:
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

In the next section, you can associate the trigger with an action by creating a rule.

## Using rules to associate triggers with actions
{: #openwhisk_rules_assoc}

Rules are used to associate a trigger with an action. Each time a trigger event is fired, the action is invoked with the parameters from the trigger event.

After you create the [`locationUpdate` trigger](#openwhisk_triggers_fire), the following steps show you how to create an example rule that calls the `hello` action whenever a location update is posted.

1. Create a file named 'hello.js' with the following action code:
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: pre}

2. Create the `hello` action.
    ```
    ibmcloud wsk action create hello hello.js
    ```
    {: pre}

3. Create the `myRule` rule to associate the `locationUpdate` trigger with the `hello` action. Rules must be created directly within a namespace and can't be created inside packages.
    ```
    ibmcloud wsk rule create myRule locationUpdate hello
    ```
    {: pre}

4. Fire the `locationUpdate` trigger. Each a trigger event occurs, the `hello` action is called with the event parameters.
    ```
    ibmcloud wsk trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Example output:
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Verify that the `hello` action was invoked by checking the most recent activation record.
    ```
    ibmcloud wsk activation list --limit 1 hello
    ```
    {: pre}

    Example output:
    ```
    activations
    9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. Get more information on the activation ID from the previous command output.
    ```
    ibmcloud wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    Example output:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    You see that the `hello` action received the event payload and returned the expected string.

7. To disable the rule, you can run the following command.
    ```
    ibmcloud wsk rule disable myRule
    ```
    {: pre}

You can also use rules to associate triggers with sequences. For example, you can create an action sequence called `recordLocationAndHello` that is activated by the rule `anotherRule`:
```
ibmcloud wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud wsk rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}

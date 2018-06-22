---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# General concepts
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} triggers and rules bring event-driven capabilities to the platform. Events from external and internal event sources are channeled through a trigger, and rules allow your actions to react to these events.
{: shortdesc}

## What is a trigger?
{: #openwhisk_triggers_create}

Triggers are a named channel for a class of events. The following are examples of triggers:
- A trigger of location update events.
- A trigger of document uploads to a website.
- A trigger of incoming emails.

Triggers can be *fired* (activated) by using a dictionary of key-value pairs. Sometimes this dictionary is referred to as the *event*. As with actions, each firing of a trigger results in an **activation ID**.

Triggers can be explicitly fired by a user or fired on behalf of a user by an external event source.
A *Feed* is a convenient way to configure an external event source to fire trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. Refer to the following example Feeds:
- {{site.data.keyword.cloudant}} data change feed that fires a trigger event each time a document in a database is added or modified.
- A Git feed that fires a trigger event for every commit to a Git repository.

## How do rules affect triggers?
{: #openwhisk_rules_use}

A rule associates one trigger with one action, for every firing of the trigger that causes the corresponding action to be invoked with the trigger event as input.

With the appropriate set of rules, it's possible for a single trigger event to invoke multiple actions, or for an action to be invoked as a response to events from multiple triggers.

For example, consider a system with the following actions:
- `classifyImage` - An action that detects the objects in an image and classifies them.
- `thumbnailImage` - An action that creates a thumbnail version of an image.

Also, suppose that two event sources are firing the following triggers:
- `newTweet` - A trigger that is fired when a new tweet is posted.
- `imageUpload` - A trigger that is fired when an image is uploaded to a website.

You can set up rules so that a single trigger event invokes multiple actions, and have multiple triggers invoke the same action:
- `newTweet -> classifyImage` rule.
- `imageUpload -> classifyImage` rule.
- `imageUpload -> thumbnailImage` rule.

The three rules establish the following behavior:
- Images in both tweets are classified.
- Uploaded images are classified
- A thumbnail version is generated.

## Create and fire triggers
{: #openwhisk_triggers_fire}

Triggers can be fired when certain events occur, or can be fired manually.

As an example, create a trigger to send user location updates, and manually fire the trigger.
1. Enter the following command to create the trigger:
  ```
  ibmcloud wsk trigger create locationUpdate
  ```
  {: pre}

  Example output:
  ```
  ok: created trigger locationUpdate
  ```
  {: screen}

2. Check that you created the trigger by listing the set of triggers.
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

  Now a named "channel" is created to which events can be fired.

3. Next, fire a trigger event by specifying the trigger name and parameters:
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Example output:
  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

A trigger that is fired without an accompanying rule to match against has no visible effect.
triggers cannot be created inside a package; they must be created directly under a **Namespace**.

## Using rules to associate triggers with actions
{: #openwhisk_rules_assoc}

Rules are used to associate a trigger with an action. Each time a trigger event is fired, the action is invoked with the event parameters.

As an example, create a rule that calls the `hello` action whenever a location update is posted.
1. Create a file named 'hello.js' with the following action code:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Make sure that the trigger and action exist:
  ```
  ibmcloud wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. The next step is to create the rule. The rule is enabled upon creation, meaning that it is immediately available to respond to Activations of your trigger. The three parameters are: _rule name_, _trigger name_, and the _action name_.
  ```
  ibmcloud wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  At any time, you can choose to disable a rule:
  ```
  ibmcloud wsk rule disable myRule
  ```
  {: pre}

4. Fire the **locationUpdate** trigger. Each time that you fire an event, the **hello** action is called with the event parameters.
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Example output:
  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. Verify that the **hello** action was invoked by checking the most recent Activation.
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

  Now query the activation ID listed in the previous command output:
  ```
  ibmcloud wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  Example output:
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  You see that the **hello** action received the event payload and returned the expected string.

You can create multiple rules that associate the same trigger with different actions.
Triggers and rules cannot belong to a package. The rule can be associated with an action
that belongs to a package however, for example:
  ```
  ibmcloud wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

You can also use rules with sequences. For example, one can create an action
sequence `recordLocationAndHello` that is activated by the rule `anotherRule`.
  ```
  ibmcloud wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}

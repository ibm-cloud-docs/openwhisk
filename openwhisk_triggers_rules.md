---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# General concepts
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} Triggers and Rules bring event-driven capabilities to the platform. Events from external and internal event sources are channeled through a Trigger, and Rules allow your Actions to react to these events.
{: shortdesc}

## What is a Trigger?
{: #openwhisk_triggers_create}

Triggers are a named channel for a class of events. The following are examples of Triggers:
- A Trigger of location update events.
- A Trigger of document uploads to a website.
- A Trigger of incoming emails.

Triggers can be *fired* (activated) by using a dictionary of key-value pairs. Sometimes this dictionary is referred to as the *event*. As with Actions, each firing of a Trigger results in an **activation ID**.

Triggers can be explicitly fired by a user or fired on behalf of a user by an external event source.
A *Feed* is a convenient way to configure an external event source to fire Trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. Refer to the following example Feeds:
- Cloudant data change feed that fires a Trigger event each time a document in a database is added or modified.
- A Git feed that fires a Trigger event for every commit to a Git repository.

## How do Rules affect Triggers?
{: #openwhisk_rules_use}

A Rule associates one Trigger with one Action, for every firing of the Trigger that causes the corresponding Action to be invoked with the Trigger event as input.

With the appropriate set of Rules, it's possible for a single Trigger event to invoke multiple Actions, or for an Action to be invoked as a response to events from multiple Triggers.

For example, consider a system with the following Actions:
- `classifyImage` - An Action that detects the objects in an image and classifies them.
- `thumbnailImage` - An Action that creates a thumbnail version of an image.

Also, suppose that two event sources are firing the following Triggers:
- `newTweet` - A Trigger that is fired when a new tweet is posted.
- `imageUpload` - A Trigger that is fired when an image is uploaded to a website.

You can set up rules so that a single Trigger event invokes multiple Actions, and have multiple Triggers invoke the same Action:
- `newTweet -> classifyImage` Rule.
- `imageUpload -> classifyImage` Rule.
- `imageUpload -> thumbnailImage` Rule.

The three Rules establish the following behavior: 
- Images in both tweets are classified.
- Uploaded images are classified
- A thumbnail version is generated.

## Create and fire Triggers
{: #openwhisk_triggers_fire}

Triggers can be fired when certain events occur, or can be fired manually.

As an example, create a Trigger to send user location updates, and manually fire the Trigger.
1. Enter the following command to create the Trigger:
  ```
  ic wsk trigger create locationUpdate
  ```
  {: pre}

  **Output:**
  ```
  ok: created trigger locationUpdate
  ```
  {: screen}

2. Check that you created the Trigger by listing the set of Triggers.
  ```
  ic wsk trigger list
  ```
  {: pre}

  **Output:**
  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```
  {: screen}

  Now a named "channel" is created to which events can be fired.

3. Next, fire a Trigger event by specifying the Trigger name and parameters:
  ```
  ic wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  **Output:**
  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

A Trigger that is fired without an accompanying Rule to match against has no visible effect.
Triggers cannot be created inside a Package; they must be created directly under a **Namespace**.

## Using Rules to associate Triggers with Actions
{: #openwhisk_rules_assoc}

Rules are used to associate a Trigger with an Action. Each time a Trigger event is fired, the Action is invoked with the event parameters.

As an example, create a rule that calls the `hello` Action whenever a location update is posted.
1. Create a file named 'hello.js' with the following Action code:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Make sure that the Trigger and Action exist:
  ```
  ic wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  ic wsk action update hello hello.js
  ```
  {: pre}

3. The next step is to create the Rule. The rule is enabled upon creation, meaning that it is immediately available to respond to Activations of your Trigger. The three parameters are: _Rule name_, _Trigger name_, and the _Action name_.
  ```
  ic wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  At any time, you can choose to disable a Rule:
  ```
  ic wsk rule disable myRule
  ```
  {: pre}

4. Fire the **locationUpdate** Trigger. Each time that you fire an event, the **hello** Action is called with the event parameters.
  ```
  ic wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  **Output:**
  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. Verify that the **hello** Action was invoked by checking the most recent Activation.
  ```
  ic wsk activation list --limit 1 hello
  ```
  {: pre}

  **Output:**
  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  Now query the activation ID listed in the previous command output:
  ```
  ic wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  **Output:**
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  You see that the **hello** Action received the event payload and returned the expected string.

You can create multiple Rules that associate the same Trigger with different Actions.
Triggers and Rules cannot belong to a Package. The Rule can be associated with an Action
that belongs to a Package however, for example:
  ```
  ic wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

You can also use Rules with sequences. For example, one can create an Action
sequence `recordLocationAndHello` that is activated by the rule `anotherRule`.
  ```
  ic wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ic wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}

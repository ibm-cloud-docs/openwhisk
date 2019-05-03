<staging>---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-03"

keywords: actions, serverless, javascript, node, node.js

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


# Associating triggers with actions
{: #rules}

Every time the trigger fires, the rule uses the trigger event as input and invokes the associated action. With the appropriate set of rules, it's possible for a single trigger event to invoke multiple actions, or for an action to be invoked as a response to events from multiple triggers.
{: shortdesc}

# Associating triggers with actions
{: #rules}

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


## Creating rules from the CLI
{: #rules_create}

Rules are used to associate a trigger with an action. Each time a trigger event is fired, the action is invoked with the parameters from the trigger event.

1. Create a file named 'hello.js' with the following action code:
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: pre}

2. Create the `hello` action.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Create the `myRule` rule to associate the `locationUpdate` trigger with the `hello` action. Rules must be created directly within a namespace and can't be created inside packages.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. Fire the `locationUpdate` trigger. Each time a trigger event occurs, the `hello` action is called with the event parameters.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Example output:
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Verify that the `hello` action was invoked by checking the most recent activation record.
    ```
    ibmcloud fn activation list --limit 1 hello
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
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
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
    ibmcloud fn rule disable myRule
    ```
    {: pre}


## Creating rules for action sequences
{: #rules_seq}

You can also use rules to associate triggers with sequences. For example, you can create an action sequence called `recordLocationAndHello` that is activated by the rule `anotherRule`.

```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}

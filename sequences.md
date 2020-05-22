---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-22"

keywords: actions, functions, serverless, javascript, node, node.js, sequence

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

# Creating sequences
{: #sequences}

You can create an action, called a sequence, that chains together several actions. The result of one action is passed as an argument to the next action. Sequences can use standard actions or web actions. 
{: shortdesc}

Parameters that are passed between actions in the sequence are explicit, except for default parameters. Therefore, parameters that are passed to the action sequence are only available to the first action in the sequence. The result of the first action in the sequence becomes the input JSON object to the second action in the sequence, and so on. This object does not include any of the parameters that are originally passed to the sequence unless the first action includes them in its result. Input parameters to an action are merged with the action's default parameters, with the former taking precedence and overriding any matching default parameters.

A sequence does not have limits that are separate from those limits of each action that is contained within the sequence. While you can set limits for `timeout`, `memory`, and `logsize` when you create a sequence, those limits are ignored. Note that the limits configured for each action in the sequence are individually enforced. Because a sequence is a pipeline of actions, a failure in one action breaks the pipeline. For example, if one action times out, the entire sequence is exited with that failure.

After you create a sequence, you can use the name of the sequence when you create a rule or invoke the actions. 

## Creating a sequence from the console
{: #actions_seq_console}

Create a sequence from the console using actions that are available in your namespace or public actions.
{: shortdesc}

**Before you begin**

Select a namespace to contain your {{site.data.keyword.openwhisk_short}} entities from the [console](https://cloud.ibm.com/functions){: external}. For more information about namespaces, see [Managing namespaces](/docs/openwhisk?topic=openwhisk-namespaces).


1. Go to the [Create page ](https://cloud.ibm.com/functions/create){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Click **Create Sequence**.

3. Specify a name, package, and inital action for your sequence. You can choose existing actions, available in your namespace or else select a public action. Click **Create**.

4. Add one or more actions to your sequence and **Save**. 

5. You can test your code by clicking **Invoke**.

## Creating a sequence from the CLI
{: #actions_seq_cli}

Create a sequence from the CLI with the [`ibmcloud fn action create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create) command.
{: shortdesc}

```
ibmcloud fn action create <sequence_name> --sequence <action_1>,<action_2>
```
{: pre}

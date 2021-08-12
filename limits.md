---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-06"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions, functions, statelessness, activation, action

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


# System details and limits
{: #limits}

The following sections provide technical details about the {{site.data.keyword.openwhisk}} system and limit settings.
{: shortdesc}

## Action limits
{: #limits_actions}

{{site.data.keyword.openwhisk_short}} has a few system limits, including how much memory an action can use and how many action invocations are allowed per minute.
{: shortdesc}

The following table lists the default limits for actions.

<table>
<caption>Default limits for actions</caption>
<colgroup>
<col width="20%">
<col width="50%">
<col width="10%">
<col width="10%">
<col width="10%">
</colgroup>
    <thead>
    <th>Limit</th>
    <th>Description</th>
    <th>Default</th>
    <th>Min</th>
    <th>Max</th>
    </thead>
    <tbody>
    <tr>
    <td><code>codeSize</code></td>
    <td>The maximum code size for an action is 48 MB. You can use [custom Docker images](/docs/openwhisk?topic=openwhisk-prep#prep_docker) as a workaround for large dependencies; however, the maximum code size for your action is still 48 MB. For JavaScript actions, use a tool to concatenate all source code, which includes dependencies, into a single bundled file. This limit is fixed and cannot be changed. Note that binary code is base64 encoded, which increases the size by approximately 33%. In this case, the actual limit is reduced to 36 MB.</td>
    <td>48</td>
    <td>1</td>
    <td>48</td>
    </tr>
    <tr>
    <td><code>concurrent</code></td>
    <td>The number of activations that are either executing or queued for execution for a namespace cannot exceed 1000. This limit value is fixed, but can be increased if a business case can justify higher safety limit values. See [Increasing fixed limits](#limits_fixed) for instructions on how to increase this limit.</td>
    <td>1000</td>
    <td>1</td>
    <td>1000*</td>
    </tr>
    <tr>
    <td><code>logs</code></td>
    <td>The log limit N is in the range [0 MB..10 MB] and is set per action. A user can change the action log limit when an action is created or updated. Logs that exceed the set limit are truncated, so any new log entries are ignored, and a warning is added as the last output of the activation to indicate that the activation exceeded the set log limit.</td>
    <td>10</td>
    <td>0</td>
    <td>10</td>
    </tr>
    <tr>
    <td><code>memory</code></td>
    <td>The memory limit M is in the range from [128 MB..2048 MB] and is set per action in MB. A user can change the memory limit when an action is created. A container cannot use more memory than is allocated by the limit.</td>
    <td>256</td>
    <td>128</td>
    <td>2048</td>
    </tr>
    <tr>
    <td><code>minuteRate</code></td>
    <td>No more than N activations can be submitted per namespace per minute. The rate limit N is set to 5000 and limits the number of action invocations in 1-minute windows. A CLI or API call that exceeds this limit receives an error code corresponding to HTTP status code <code>429: TOO MANY REQUESTS</code>. This limit value is fixed, but can be increased if a business case can justify higher safety limit values. See [Increasing fixed limits](#limits_fixed) for instructions on how to increase this limit.</td>
    <td>5000</td>
    <td>1</td>
    <td>5000*</td>
    </tr>
    <tr>
    <td><code>openulimit</code></td>
    <td>The maximum number of open files for an action is 1024 (for both hard and soft limits). This limit is fixed and cannot be changed. When an action is invoked, the docker run command uses the argument `--ulimit nofile=1024:1024` to set the <code>openulimit</code> value. For more information, see the [docker run](https://docs.docker.com/engine/reference/commandline/run/){: external} command line reference documentation.</td>
    <td>1024</td>
    <td>0</td>
    <td>1024</td>
    </tr>
    <tr>
    <td><code>parameters</code></td>
    <td>The maximum size of the parameters that can be attached in MB. The size limit for the total parameters on creating or updating of an Action/Package/Trigger is 5 MB. An entity with too large parameters is rejected on trying to create or update it. This limit is fixed and cannot be changed.</td>
    <td>5</td>
    <td>0</td>
    <td>5</td>
    </tr>
    <tr>
    <td><code>proculimit</code></td>
    <td>The maximum number of processes available to the action container is 1024. This limit is fixed and cannot be changed. When an action is invoked, the docker run command uses the argument <code>--pids-limit 1024</code> to set the <code>proculimit</code> value. For more information, see the [docker run](https://docs.docker.com/engine/reference/commandline/run/){: external} command-line reference documentation.</td>
    <td>1024</td>
    <td>0</td>
    <td>1024</td>
    </tr>
    <tr>
    <td><code>result</code></td>
    <td>The maximum output size of an action invocation result in MB. This limit is fixed and cannot be changed.</td>
    <td>5</td>
    <td>0</td>
    <td>5</td>
    </tr>
    <tr>
    <td><code>sequenceMaxActions</code></td>
    <td>The maximum number of actions that comprise a sequence. This limit is fixed and cannot be changed.</td>
    <td>50</td>
    <td>0</td>
    <td>50*</td>
    </tr>
    <tr>
    <td><code>timeout</code></td>
    <td>The timeout limit N is in the range [100 ms..600000 ms], and is set per action in milliseconds. A user can change the timeout limit when an action is created. A container that runs longer than N milliseconds is terminated.</td>
    <td>60000</td>
    <td>100</td>
    <td>600000</td>
    </tr>
    </tbody>
</table>

## Web action limits
{: #web_action_limits}

| Parameter | Limit |
| --- | --- |
| Request headers | 8 KB cumulative |
| Response headers | 4 KB cumulative |



## Sequence limits
{: #limits_sequence}

A sequence does not have limits that are separate from those limits of each action that is contained within the sequence. Because a sequence is a pipeline of actions, a failure in one action breaks the pipeline. For example, if one action times out, the entire sequence is exited with that failure.
{: shortdesc}

## Trigger rate limits
{: #limits_triggers}

Triggers are subject to a firing rate per minute as documented in the following table.

<table>
<caption>Default limits for triggers</caption>
<col width="15%">
<col width="40%">
<col width="15%">
<col width="15%">
<col width="15%">
    <thead>
    <th>Limit</th>
    <th>Description</th>
    <th>Default</th>
    <th>Min</th>
    <th>Max</th>
    </thead>
    <tbody>
    <tr>
    <td><code>minuteRate</code></td>
    <td>The rate limit N is set to 5000 and limits the number of triggers that a user can fire in 1-minute windows. A user cannot change the trigger limit when a trigger is created. A CLI or API call that exceeds this limit receives an error code corresponding to HTTP status code <code>429: TOO MANY REQUESTS</code>. This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](#limits_fixed) for detailed instructions on how to increase this limit.</td>
    <td>5000*</td>
    <td>5000*</td>
    <td>5000*</td>
    </tr>
    </tbody>
</table>

## Increasing fixed limits
{: #limits_fixed}

Limit values that end with a (*) are fixed, but can be increased if a business case can justify higher safety limit values. If you would like to increase the limit value, contact IBM support by opening a ticket directly from the IBM [{{site.data.keyword.openwhisk_short}} web console](https://cloud.ibm.com/functions){: external}.

1. Select **Support**.
2. Select **Add Ticket** from the drop-down menu.
3. Select **Technical** for the ticket type.
4. Select **Functions** for Technical area of support.


## {{site.data.keyword.openwhisk_short}} entities
{: #limits_entities_ov}

The following sections describe system details about {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

### Namespaces and packages
{: #limits_namespaces}

{{site.data.keyword.openwhisk_short}} actions, triggers, and rules belong in a namespace, and sometimes a package.
{: shortdesc}

Packages can contain actions and feeds. A package cannot contain another package, so package nesting is not allowed. Also, entities do not have to be contained in a package.

You can create new IAM-based namespaces by running `ibmcloud fn namespace create`.  Cloud Foundry-based namespaces are made from a combination of an org and space name. For example, if you are targeted to the `user@email.com` org and the `dev` space, then you are targeting the {{site.data.keyword.openwhisk_short}} Cloud Foundry-based namespace called `user@email.com_dev`. 

The `/whisk.system` namespace is reserved for entities that are distributed with the {{site.data.keyword.openwhisk_short}} system.

The [Serverless Framework](https://www.serverless.com/) is not supported for IAM-based namespaces.
{: note}

### Fully qualified names in {{site.data.keyword.openwhisk_short}}
{: #limits_fullnames}

The fully qualified name of an entity is
`/<namespace_id>/<package_name>/<entity_name>`. Notice that `/` is used to delimit Cloud Foundry-based namespaces, packages, and entities. Also, when you work in the CLI, some commands with namespaces must be prefixed with a `/`.

For convenience, the namespace can be omitted if it is the user's default namespace. For example, consider a user whose default namespace is `/myOrg`. Following are examples of the fully qualified names of a number of entities and their aliases.

| Fully qualified name | Alias | Namespace | Package | Name |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

You can use this naming scheme when you use the {{site.data.keyword.openwhisk_short}} CLI, among other places.

More precisely, a name must match the following regular expression (expressed with Java metacharacter syntax): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Action semantics
{: #limits_semantics}

The following sections describe details about {{site.data.keyword.openwhisk_short}} actions.
{: shortdesc}

### Statelessness
{: #limits_stateless}

Action implementations are stateless, or *idempotent*. While the system does not enforce this property, it is not guaranteed that any state maintained by an action is available across invocations.

Moreover, multiple instantiations of an action might exist, with each instantiation with its own state. An action invocation might be dispatched to any of these instantiations.

### Invocation input and output
{: #limits_invocationio}

The input to and output from an action is a dictionary of key-value pairs. The key is a string, and the value a valid JSON value.

### Invocation ordering of actions
{: #limits_ordering}

Invocations of an action are not ordered. If the user invokes an action twice from the command-line or the REST API, the second invocation might run before the first. If the actions have side effects, they might be observed in any order.

Additionally, it is not guaranteed that actions execute automatically. Two actions can run concurrently and their side effects can be interleaved. OpenWhisk does not ensure any particular concurrent consistency model for side effects. 

### Action executions
{: #limits_exec}

When an invocation request is received, the system records the request and dispatches an activation.

The system returns an activation ID (with a nonblocking invocation) that confirms that it is received.
If a network failure or other failure that intervenes before you receive an HTTP response, it is possible that {{site.data.keyword.openwhisk_short}} received and processed the request.

The system attempts to invoke the action once, resulting in one of the following four outcomes:

| Result | Description |
| --- | --- |
| `success` | The action invocation completed successfully. |
| `application error` | The action invocation was successful, but the action returned an error value on purpose, for instance because a precondition on the arguments was not met. |
| `action developer error` | The action was invoked, but it completed abnormally, for instance the action did not detect an exception, or a syntax error existed. |
| `whisk internal error` | The system was unable to invoke the action.
The outcome is recorded in the `status` field of the activation record, as document in a following section. |

For every invocation that is successfully received, and that the user might be billed for, has an activation record.

When the outcome is *action developer error*, the action might partially run, and generate external visible side effects. It is the user's responsibility to check whether such side effects happened, and issue retry logic. Certain *whisk internal errors* indicate that an action starts to run, but fails before the action registers completion.

## Activation record
{: #limits_activation}

Each action invocation and trigger firing results in an activation record.

An activation record contains the following fields:

| Field | Description
| --- | --- |
| `activationId` | The activation ID. |
| `start` and `end` | Timestamps recording the start and end of the activation. The values are in [UNIX time format](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15){: external}. |
| `namespace` and `name` | The namespace and name of the entity. |
| <code>logs</code> | An array of strings with the logs that are produced by the action during its activation. Each array element corresponds to a line output to `stdout` or `stderr` by the action, and includes the time and stream of the log output. The structure is as follows: `TIMESTAMP STREAM: LOG_OUTPUT`. |
| `response` | A dictionary that defines the keys `success`, `status`, and <code>result</code>. <ul><li><code>status</code>: The activation result, which might be one of the following values: <code>success</code>, <code>application error</code>, <code>action developer error</code>, or <code>whisk internal error</code>.</li><li> <code>success</code>: <code>true</code> if and only if the status is <code>success</code>.</li></ul> |
| <code>result</code> | A dictionary that contains the activation result. If the activation was successful, the result contains the value that is returned by the action. If the activation was unsuccessful, <code>result</code> contains the `error` key, generally with an explanation of the failure. |



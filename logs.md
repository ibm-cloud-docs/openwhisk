---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-31"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health, functions

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

# Viewing logs
{: #logs}

Logging is automatically enabled in {{site.data.keyword.openwhisk}} to help you troubleshoot issues. You can also use the {{site.data.keyword.cloudaccesstraillong}} service to track how users and applications interact with the {{site.data.keyword.openwhisk_short}} service. Finally, you can [view logs in {{site.data.keyword.la_full_notm}}](#logs_logdna).
{: shortdesc}

## Viewing action logs as they occur
{: #logs_poll}

{{site.data.keyword.openwhisk_short}} actions can be invoked by other users, in response to various events, or as part of an action sequence. To get information about when actions were invoked and what the output was, it can be useful to monitor the action logs.
{: shortdesc}

Activation data is only accessible through the {{site.data.keyword.openwhisk_short}} API (CLI or REST) for 24 hours after the action finishes processing. In addition, the activation data is also sent to your logging instance and can be accessed there for longer period of time, depending on the service plan that has been chosen for the logging instance. For more information, see [Logging details](#logs_logdna).

You can use the {{site.data.keyword.openwhisk_short}} CLI to watch the output of actions as they are invoked.

1. Start a polling loop that continuously checks for logs from activations.

    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Switch to another window and invoke an action.

    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    **Example output**
    
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. In the polling window, you can see the activation log.

    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
      2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    
    You might also see the logs for any actions that are run on your behalf in {{site.data.keyword.openwhisk_short}} in real time.

## Viewing activation details
{: #activation_details}

{{site.data.keyword.openwhisk_short}} actions can be invoked by other users, in response to various events, or as part of an action sequence. When an action is invoked, an activation record is created for that invocation. To get information about the result of the action invocation, you can get details about activations.

Activation data is only accessible through the {{site.data.keyword.openwhisk_short}} API (CLI or REST) for 24 hours after the action finishes processing. In addition, the activation data is also sent to logging and can be accessed there for a longer period of time, depending on the service plan that you chose for the logging instance. For more information, see [Viewing logs in {{site.data.keyword.la_full_notm}}](#logs_logdna).
{: note}

You can get all activation record IDs in a namespace by running the following command.

```
ibmcloud fn activation list
```
{: pre}

You can get details about a specific activation record that resulted from an action invocation by running the following command. Replace `<activation_ID>` with the ID of the activation. 

```
ibmcloud fn activation get <activation_ID>
```
{: pre}

**Example output**

```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "<namespace_ID>",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
        }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "<namespace_ID>/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
            "value": "nodejs:6"
        },
        {
            "key": "limits",
            "value": {
                "logs": 10,
                "memory": 256,
                "timeout": 60000
            }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

</br>

<table>
<caption>Understanding the <code>activation get</code> command output</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>activation get</code> command output</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>The namespace ID that this activation is in. This namespace might be different than the namespace that the action is in.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>The name of the action.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>The semantic version of the action.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>The user account that activated the item.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID of this activation record.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>Time when the activation began.</td>
</tr>
<tr>
<td><code>end</code></td>
<td>Time when the activation completed.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Time, in milliseconds, that it took for the activation to complete.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: The exit status of the activation.</li>
<li><code>statusCode</code>: The status code. If the action resulted in an error, this value is the HTTP error code.</li>
<li><code>success</code>: The result of whether the action completed successfully.</li>
<li><code>result</code>: The return value from the activation.</li>
</ul>For more possible outcomes, see [Action executions](/docs/openwhisk?topic=openwhisk-limits#limits_exec).</td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Logs for this activation.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotations on this action. For a list of possible activation annotations, see the [annotations reference topic](/docs/openwhisk?topic=openwhisk-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>The result of whether the action is published.</td>
</tr>
</tbody></table>

## Viewing logs in {{site.data.keyword.la_full_notm}}
{: #logs_logdna}

Action logs are forwarded to an {{site.data.keyword.la_full_notm}} service where they are indexed, enabling full-text search through all generated messages and convenient querying based on specific fields.
{: shortdesc}

To get started, complete the following steps.

1. Navigate to the {{site.data.keyword.la_full_notm}} service and create an instance in the same region as your {{site.data.keyword.openwhisk_short}} namespace.

2. Configure the {{site.data.keyword.la_short}} instance to receive platform service logs.

You can also launch logging from your {{site.data.keyword.openwhisk_short}} dashboard by selecting **Launch Logging**.
{: tip}

### Configure {{site.data.keyword.la_full_notm}}
{: #logs_configure_logdna}

In order to use the [{{site.data.keyword.la_full_notm}} service](https://cloud.ibm.com/observe/logging){: external} to view the logs of your {{site.data.keyword.openwhisk_short}} actions, you need to provision an instance first. For more information about additional options, see the [Getting started tutorial](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-getting-started){: external}.

To enable an instance that is receiving {{site.data.keyword.openwhisk_short}} action logs, you need to configure the [Platform Service Logs](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-config_svc_logs){: external} in the logging service.

{{site.data.keyword.openwhisk_short}} sends the action logs to the {{site.data.keyword.la_short}} service in the same region as the {{site.data.keyword.openwhisk_short}} namespace. Actions logs of a {{site.data.keyword.openwhisk_short}} namespace in `us-south` are sent to a logging instance in `us-south`.



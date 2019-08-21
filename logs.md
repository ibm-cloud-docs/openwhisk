---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-21"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health, functions

subcollection: cloud-functions

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

Logging is automatically enabled in {{site.data.keyword.openwhisk}} to help you troubleshoot issues. You can also use the {{site.data.keyword.cloudaccesstraillong}} service to track how users and applications interact with the {{site.data.keyword.openwhisk_short}} service.


## Viewing action logs as they occur
{: #logs_poll}

{{site.data.keyword.openwhisk_short}} actions can be invoked by other users, in response to various events, or as part of an action sequence. To get information about when actions were invoked and what the output was, it can be useful to monitor the action logs.

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
    "namespace": "myNamespace",
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
            "value": "myNamespace/hello"
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

<table>
<caption>Understanding the <code>activation get</code> command output</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>activation get</code> command output</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>The namespace that this activation is in. This might be different than the namespace that the action is in.</td>
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
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Logs for this activation.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotations on this action. For a list of possible activation annotations, see the [annotations reference topic](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>The result of whether the action is published.</td>
</tr>
</tbody></table>



## Viewing logs in IBM Log Analysis with LogDNA
{: #logs_logdna}

You can view activation records and action logs for individual action invocations directly from the {{site.data.keyword.openwhisk_short}} Monitor dashboard by clicking on an action's activation id.

In addition, action logs are also forwarded to IBM Log Analysis service where they are indexed, enabling full-text search through all generated messages and convenient querying based on specific fields.

### Configure IBM Log Analysis with LogDNA
{: #logs_configure_logdna}

In order to use the {{site.data.keyword.loganalysislong_notm}} with LogDNA service to view the logs of your {{site.data.keyword.openwhisk_short}} actions, you need to provision an instance first. See the Getting started tutorial for details on the various options.

{{site.data.keyword.openwhisk_short}} logs work well with all available deployment plans, so chose the option which fits best for your needs.
To enable an instance receiving {{site.data.keyword.openwhisk_short}} action logs, you need to configure the Platform Service Logs in LogDNA service.

{{site.data.keyword.openwhisk_short}} sends the action logs to the IBM Log Analysis with LogDNA service of the same region as the {{site.data.keyword.openwhisk_short}} namespace. This means that actions logs of a `us-south` namespace will be forwarded to a LogDNA instance in `us-south`.

Currently there is one exception, that actions of namespaces in `us-east` sends logs to a LogDNA instance in `us-south`.
{: important}

### Querying logs
{: #logs_query}

To view and query your action logs go to the [{{site.data.keyword.openwhisk_short}} dashboard](link) and select a namespace. 

1. In the left-hand navigation, click **Logs** to launch the {{site.data.keyword.loganalysislong_notm}} with LogDNA page.
2. Click **View LogDNA** of the appropriate instance, see [Configure LogDNA]().
3. Use the LogDNA search capabilities to filter for certain namespaces and/or actions. For details on how to search and filter logs see the [LogDNA Search Guide](link).

Logs produced by the action code will have a field type:user_log.
{: note}

### Querying activation metadata

In addition to log messages, {{site.data.keyword.openwhisk_short}} also forwards activation records to LogDNA for indexing and searching. The activation records contain metadata such as the activation duration or the activation result code. Querying result fields can help you understand how your Cloud Functions' actions are behaving. Activation records are marked in the logs with field type:activation_record.

You can find specific activation records by using LogDNA query syntax. The following example query can help you to find all failed activations and debug errors.
Enter type:activation_record response.success:false into LogDNA search field.

For more details on how to search and filter logs see the LogDNA Search Guide
Action results in the logs.

The legacy {{site.data.keyword.loganalysislong_notm}} service with Kibana search has also indexed the action return results. This service is being deprecated. The support for it will be removed soon, and by that we will no longer provide the results in the logs for security reasons.
If you still need the action results, you have to add an appropriate log statement explicitly.
{: deprecated}
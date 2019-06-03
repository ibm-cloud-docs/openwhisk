---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-03"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

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

    Example output:
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

{{site.data.keyword.openwhisk_short}} actions can be invoked by other users, in response to various events, or as part of an action sequence. Whenever an action is invoked, an activation record is created for that invocation. To get information about the result of the action invocation, you can get details about activations.

To get all activation record IDs in a namespace:
```
ibmcloud fn activation list
```
{: pre}

To get details about a specific activation record that resulted from an action invocation:
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Example output:
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
<li><code>statusCode</code>: The status code. If the action errored, the HTTP error code.</li>
<li><code>success</code>: Whether the action completed successfully.</li>
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
<td>Whether the action is publicly published.</td>
</tr>
</tbody></table>



## Viewing logs in {{site.data.keyword.loganalysisfull_notm}}
{: #logs_view}

{{site.data.keyword.loganalysislong_notm}} logs are not available for IAM-based namespaces.
{: note}

You can view activation logs directly from the {{site.data.keyword.openwhisk_short}} Monitoring dashboard. The logs are also forwarded to [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) where they are indexed, enabling full-text search through all generated messages and convenient querying based on specific fields.
{:shortdesc}

**Note**: Logging is not available for the US East region.

1. Open the [{{site.data.keyword.openwhisk_short}} Monitoring page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/dashboard).

2. Optional: To view logs only for a specific action, limit the monitoring summary to that action. In the Filtering Options section, select the action name from the **Limit to** drop-down list.

3. In the left-hand navigation, click **Logs**. The {{site.data.keyword.loganalysisshort_notm}} Kibana page opens.

4. Optional: To see older logs, change the default timeframe value of 15 minutes by clicking **Last 15 minutes** in the upper right-hand corner and selecting a different timeframe.

### Querying logs
{: #logs_query}

You can find specific activation logs in [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) by using Kibana's query syntax.

The following example queries can help you debug errors:
  * Find all error logs:
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Find all error logs that are generated by "myAction":
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Querying results
{: #logs_query_results}

In addition to log lines, [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) also indexes the results, or the activation records, that are generated by {{site.data.keyword.openwhisk_short}}. The results contain activation metadata such as the activation duration or the activation result code. Querying result fields can help you understand how your {{site.data.keyword.openwhisk_short}} actions are behaving.

You can find specific activation logs by using Kibana's query syntax. The following example queries can help you debug errors:

* Find all failed activations:
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    In the results, a `0` indicates a successfully exited action and all other values indicate an error.

* Find all activations that failed with a specific error:
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}


---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health, functions, activations, configuring

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Viewing logs
{: #logs}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Logging is automatically enabled in {{site.data.keyword.openwhisk}} to help you troubleshoot issues. You can also use the {{site.data.keyword.cloudaccesstraillong}} service to track how users and applications interact with the {{site.data.keyword.openwhisk_short}} service. Finally, you can [view logs in {{site.data.keyword.la_full_notm}}](#logs_console).
{: shortdesc}

## Viewing action logs as they occur
{: #logs_poll}

{{site.data.keyword.openwhisk_short}} actions can be invoked by other users, in response to various events, or as part of an action sequence. To get information about when actions were invoked and what the output was, it can be useful to monitor the action logs.
{: shortdesc}

Activation data is only accessible through the {{site.data.keyword.openwhisk_short}} API (CLI or REST) for 24 hours after the action finishes processing. In addition, the activation data is also sent to your logging instance and can be accessed there for longer period of time, depending on the service plan that has been chosen for the logging instance.

You can use the {{site.data.keyword.openwhisk_short}} CLI to watch the output of actions as they are invoked.

1. Start a polling loop that continuously checks for logs from activations.

    ```bash
    ibmcloud fn activation poll
    ```
    {: pre}

2. Switch to another window and invoke an action.

    ```bash
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    **Example output**

    ```sh
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. In the polling window, you can see the activation log.

    ```sh
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
        2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}

    You might also see the logs for any actions that are run on your behalf in {{site.data.keyword.openwhisk_short}} in real time.

## Viewing activation details
{: #activation_details}

{{site.data.keyword.openwhisk_short}} actions can be invoked by other users, in response to various events, or as part of an action sequence. When an action is invoked, an activation record is created for that invocation. To get information about the result of the action invocation, you can get details about activations.

Activation data is only accessible through the {{site.data.keyword.openwhisk_short}} API (CLI or REST) for 24 hours after the action finishes processing. In addition, the activation data is also sent to logging and can be accessed there for a longer period of time, depending on the service plan that you chose for the logging instance. For more information, see [Viewing logs in {{site.data.keyword.la_full_notm}}](#logs_console).
{: note}

You can get all activation record IDs in a namespace by running the following command.

```bash
ibmcloud fn activation list
```
{: pre}

You can get details about a specific activation record that resulted from an action invocation by running the following command. Replace `<activation_ID>` with the ID of the activation. 

```bash
ibmcloud fn activation get <activation_ID>
```
{: pre}

See the following example for the **`ibmcloud fn activation get <activation_ID>`** command.

```sh
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

| `namespace` | The namespace ID that this activation is in. This namespace might be different than the namespace that the action is in. | 
| `name` | The name of the action. | 
| `version` | The semantic version of the action. | 
| `subject` | The user account that activated the item. | 
| `activationId` | ID of this activation record. | 
| `start` | Time when the activation began. | 
| `end` | Time when the activation completed. | 
| `duration` | Time, in milliseconds, that it took for the activation to complete. | 
| `response` |  - `status`: The exit status of the activation. \n - `statusCode`: The status code. If the action resulted in an error, this value is the HTTP error code. \n - `success`: The result of whether the action completed successfully. \n - `result`: The return value from the activation. \n For more possible outcomes, see [Action executions](/docs/openwhisk?topic=openwhisk-limits#limits_exec). | 
| `logs` | Logs for this activation. | 
| `annotations` | Annotations on this action. For a list of possible activation annotations, see the [annotations reference topic](/docs/openwhisk?topic=openwhisk-annotations#annotations_activation). | 
| `publish` | The result of whether the action is published. | 
{: caption="Table 1. Understanding the command output" caption-side="bottom"}

## Viewing logs in {{site.data.keyword.la_full_notm}}
{: #logs_console}

Action logs are forwarded to an {{site.data.keyword.la_full_notm}} service where they are indexed, enabling full-text search through all generated messages and convenient querying based on specific fields.
{: shortdesc}

To get started, complete the following steps.

1. Navigate to the {{site.data.keyword.la_full_notm}} service and create an instance in the same region as your {{site.data.keyword.openwhisk_short}} namespace.

2. Configure the {{site.data.keyword.la_short}} instance to receive platform service logs.

You can also launch logging from your {{site.data.keyword.openwhisk_short}} dashboard by selecting **Launch Logging**.
{: tip}

### Configure {{site.data.keyword.la_full_notm}}
{: #logs_configure_log}

To use the [{{site.data.keyword.la_full_notm}} service](https://cloud.ibm.com/observe/logging){: external} to view the logs of your {{site.data.keyword.openwhisk_short}} actions, you need to provision an instance first. For more information about additional options, see the [Getting started tutorial](/docs/log-analysis?topic=log-analysis-getting-started){: external}.
The method for accessing action logs can vary depending on the context in which you are referring to "action logs." Here are some common scenarios and the corresponding ways to see action logs:

1. **Computer/System Action Logs:** On a computer or server, action logs often refer to records of various system activities, errors, and events. To access these logs, you'll typically need administrative privileges. The steps to view logs can vary based on the operating system you're using (e.g., Windows, macOS, or Linux). Generally, you can find system logs in the "Event Viewer" on Windows, the "Console" app on macOS, and the "/var/log" directory on Linux.

2. **Website/Application Action Logs:** If you're referring to action logs within a website or application you're using, you might need to check with the developers or administrators of that specific service. Many web applications and services have their own logs for tracking user actions and system events. Depending on the platform, there may be a dedicated section or log files available for this purpose.

3. **Cloud Service Action Logs:** For cloud services, like Amazon Web Services (AWS), Google Cloud Platform (GCP), or Microsoft Azure, you can usually access action logs from the respective service's management console or dashboard. These logs provide details about actions taken within the cloud environment, such as resource creation, access events, and security-related activities.

4. **Mobile Device Action Logs:** For mobile devices, accessing action logs typically requires specific developer tools and software. On Android, you can use Android Studio or third-party tools to view logs. On iOS, Xcode or third-party tools are used for log viewing.

In general, accessing and interpreting action logs can be a technical task, and it's essential to have the necessary permissions and knowledge of the tools used for log analysis. If you're looking for specific action logs, it's best to consult the documentation or support resources related to the system, application, or service you are interested in. Additionally, seeking assistance from IT administrators, developers, or support teams can help you access and interpret the logs effectively.
To enable an instance that is receiving {{site.data.keyword.openwhisk_short}} action logs, you need to configure the [Platform Service Logs](/docs/log-analysis?topic=log-analysis-config_svc_logs){: external} in the logging service.

{{site.data.keyword.openwhisk_short}} sends the action logs to the {{site.data.keyword.la_short}} service in the same region as the {{site.data.keyword.openwhisk_short}} namespace. Actions logs of a {{site.data.keyword.openwhisk_short}} namespace in `us-south` are sent to a logging instance in `us-south`. More detail here [currentpackages](
https://currentpackages.com/)





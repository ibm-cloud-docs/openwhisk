---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-07"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions

---






{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Managing actions
{: #openwhisk_managing}
{: #managing_actions}


Manage actions by monitoring action output, getting specific information on an action, or deleting actions.
{: shortdec}

## Getting actions
{: #getting-actions}

After you have created an action, you can get more information about the action details and list the actions in your namespace.
{: shortdesc}

To list all of the actions you have created:
```
ibmcloud fn action list
```
{: pre}

As you create more actions, it can be helpful to group related actions into [packages](/docs/openwhisk?topic=cloud-functions-openwhisk_packages). To filter your list of actions to only actions within a specific package:
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

To get metadata that describes specific actions:

```
ibmcloud fn action get hello
```
{: pre}

Example output:
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption>Understanding the <code>action get</code> command output</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>action get</code> command output</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>The namespace that this action is in.</td>
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
<td><code>exec</code></td>
<td><ul><li><code>kind</code>: The type of action. Possible values are nodejs:6, nodejs:8, php:7.1, python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox, and sequence.</li>
<li><code>code</code>: Javascript or Swift code to execute when kind is nodejs or swift.</li>
<li><code>components</code>: The actions in the sequence when kind is sequence. The actions are listed in order.</li>
<li><code>image</code>: Container image name when kind is blackbox.</li>
<li><code>init</code>: Optional zipfile reference when kind is nodejs.</li>
<li><code>binary</code>: Whether the action is compiled into a binary executable.</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotations on this action. For a list of possible annotations, see the [action annotations](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#action) and [web action annotations](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#annotations-specific-to-web-actions) reference topics.</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>: The timeout, in milliseconds, set for the action after which the action is terminated. Default: 6000</li>
<li><code>memory</code>: The maximum memory limit, in MB, set for the action. Default: 256</li>
<li><code>logs</code>: The maximum log size limit, in MB, set for the action. Default: 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Whether the action is publicly published.</td>
</tr>
</tbody></table>

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
<td>Annotations on this action. For a list of possible activation annotations, see the [annotations reference topic](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Whether the action is publicly published.</td>
</tr>
</tbody></table>

## Accessing action metadata within the action body
{: #accessing-action-metadata-within-the-action-body}

The action environment contains several properties that are specific to the running action. These properties allow the action to programmatically work with OpenWhisk assets through the REST API or set an internal alarm when the action is about to use up its allotted time budget. The properties are accessible in the system environment for all supported runtimes: Node.js, Python, Swift, Java, and Docker when using the OpenWhisk Docker skeleton.

| Property | Description |
| -------- | ----------- |
| `__OW_API_HOST` | The API host for the OpenWhisk deployment running this action. |
| `__OW_API_KEY` | The API key for the subject invoking the action. This key may be a restricted API key and is absent unless explicitly requested, see [Annotations](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#openwhisk_annotations). |
| `__OW_NAMESPACE` | The namespace for the activation. This namespace might not be the same as the namespace for the action. |
| `__OW_ACTION_NAME` | The fully qualified name of the running action. |
| `__OW_ACTIVATION_ID` | The activation ID for this running action instance. |
| `__OW_DEADLINE` | The approximate time, in epoch milliseconds, when this action will consume its entire duration quota. |

## Getting an action URL
{: #get-action-url}

An action can be invoked by using the REST interface through an HTTPS request.
{: shortdesc}

To get an action URL:
```
ibmcloud fn action get actionName --url
```
{: pre}

Example output for standard actions:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Example output for [web actions](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions):
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Note:** For standard actions, authentication must be provided when invoked through an HTTPS request. For more information regarding action invocations using the REST interface, see the [REST API reference](https://cloud.ibm.com/apidocs/functions).

## Saving action code
{: #save-action}

You can get and locally save code associated with an existing action. You can save code for all actions except for sequences and Docker actions.
{: shortdesc}

Save action code to a filename that corresponds with an existing action name in the current working directory.
```
ibmcloud fn action get actionName --save
```
{: pre}

A file extension that corresponds to the action kind is used. For action code that is a zip file, an extension of .zip is used. Example output:
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

You can instead provide a custom file path, filename, and extension by using the `--save-as` flag.
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

Example output:
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## Monitoring action logs
{: #monitor-action-output}

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

## Changing action runtime
{: #changing-action-runtime}

You can change the runtime `kind` to migrate to a newer version of the action runtime. For example, since Node.js version 8 is in maintenance mode, you may want to switch the runtime to Node.js 10. You can use the following steps to change an action runtime. **Note:** You may need to change the code in `actionName.js` to comply with the new runtime version. This depends on the whether such changes are needed by the runtime switch. In most cases, the runtime versions are compatible.

1. Save the action code in a file.

  ```
  ibmcloud fn action get actionName --save
  ```
  {: pre}

2. Update the action by specifying the new runtime.

  ```
  ibmcloud fn action update actionName actionName.js --kind nodejs:10
  ```
  {: pre}

For a list of available runtimes, see [Runtimes](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)

## Managing large actions
{: #large-app-support}

The maximum code size for an action is 48 MB. Applications that contain many third-party modules, native libraries, or external tools might hit this limit. If you create a .zip or .jar package action that is larger than 48 MB, you must extend the runtime image with dependencies, and then use a single source file or smaller archive than 48 MB.

For example, by building a custom Docker runtime that includes necessary shared libraries, dependencies are not required to be present in the archive file. Private source files can still be bundled in the archive and injected at runtime.

## Deleting actions
{: #deleting-actions}

You can clean up by deleting actions that you do not want to use.

1. Delete an action.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    Example output:
    ```
    ok: deleted hello
    ```
    {: screen}

2. Verify that the action no longer appears in the list of actions.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    Example output:
    ```
    actions
    ```
    {: screen}


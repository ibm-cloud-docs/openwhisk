---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Managing actions
{: #openwhisk_managing}

Manage actions by monitoring action output, getting specific information on an action, or deleting actions.
{: shortdec}

## Monitoring action output
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}} actions might be invoked by other users, in response to various events, or as part of an action sequence. In such cases, it can be useful to monitor the invocations.

You can use the {{site.data.keyword.openwhisk_short}} CLI to watch the output of actions as they are invoked.

1. Issue the following command from a shell:
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

  This command starts a polling loop that continuously checks for logs from activations.

2. Switch to another window and invoke an action:
  ```
  ibmcloud wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  Example output:
  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```
  {: screen}

3. Observe the activation log in the polling window:
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
  {: screen}

  Similarly, whenever you run the poll utility, you see in real time the logs for any actions that are run on your behalf in OpenWhisk.

## Getting actions
{: #getting-actions}

Metadata that describes existing actions can be retrieved by using the `ibmcloud wsk action` get command.

**Command:**
```
ibmcloud wsk action get hello
```

***Result:**
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

You can list all the actions created using the following command:
```
ibmcloud wsk action list
```
{: pre}

As you write more actions, this list gets longer and it can be helpful to group related actions into [packages](./openwhisk_packages.html). To filter your list of actions to just those within a specific package, you can use the following command syntax:
```
ibmcloud wsk action list [PACKAGE NAME]
```
{: pre}

## Accessing action metadata within the action body
{: #accessing-action-metadata-within-the-action-body}

The action environment contains several properties that are specific to the running action. These properties allow the action to programmatically work with OpenWhisk assets through the REST API, or set an internal alarm when the action is about to use up its allotted time budget. The properties are accessible in the system environment for all supported runtimes: Node.js, Python, Swift, Java and Docker actions when using the OpenWhisk Docker skeleton.

* `__OW_API_HOST` the API host for the OpenWhisk deployment running this action
* `__OW_API_KEY` the API key for the subject invoking the action, this key may be a restricted API key
* `__OW_NAMESPACE` the namespace for the _activation_ (this may not be the same as the namespace for the action)
* `__OW_ACTION_NAME` the fully qualified name of the running action
* `__OW_ACTIVATION_ID` the activation id for this running action instance
* `__OW_DEADLINE` the approximate time when this action will have consumed its entire duration quota (measured in epoch milliseconds)

## Getting an action URL
{: #get-action-url}

An action can be invoked by using the REST interface through an HTTPS request. To get an action URL, execute the following command:
```
ibmcloud wsk action get actionName --url
```
{: pre}

A URL with the following format is returned for standard actions:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

For [web actions](./openwhisk_webactions.html), a URL is returned in the the following format:
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Note:** For standard actions, authentication must be provided when invoked through an HTTPS request. For more information regarding action invocations using the REST interface, see the [REST API reference](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

## Saving action code
{: #save-action}

Code associated with an existing action may be retrieved and saved locally. Saving can be performed on all actions except sequences and Docker actions.

1. Save action code to a filename that corresponds with an existing action name in the current working directory. A file extension that corresponds to the action kind is used, or an extension of .zip will be used for action code that is a zip file.
  ```
  ibmcloud wsk action get actionName --save
  ```
  {: pre}

  Example output:
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. Instead of allowing the CLI to determine the destination of the code to be saved, a custom file path, filename and extension can be provided by using the `--save-as` flag.
  ```
  ibmcloud wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  Example output:
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## Deleting actions
{: #deleting-actions}

You can clean up by deleting actions that you do not want to use.

1. Run the following command to delete an action:
  ```
  ibmcloud wsk action delete hello
  ```
  {: pre}

  Example output:
  ```
  ok: deleted hello
  ```
  {: screen}

2. Verify that the action no longer appears in the list of actions.
  ```
  ibmcloud wsk action list
  ```
  {: pre}

  Example output:
  ```
  actions
  ```
  {: screen}

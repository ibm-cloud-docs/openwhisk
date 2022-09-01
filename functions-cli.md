---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-01"

keywords: managing actions, manage, activation, action logs, changing runtime, delete, namespace, cli, rule, trigger, deployment, list, package, property, sdk, service

subcollection: cloud-functions-cli-plugin

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.openwhisk_short}} CLI
{: #functions-cli}

Run these commands to manage the entities that make up your functions.
{: shortdec}

## Action commands
{: #cli_action}

Create, invoke, list, and delete actions.
{: shortdec}

To see CLI help for the **`action`** command, run `ibmcloud fn action`.
{: tip}

### `ibmcloud fn action create`
{: #cli_action_create}

Create an action.
{: shortdec}

```sh
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--memory MEMORY_LIMIT] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

#### Command options
{: #cli_action_create-options}

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`ACTION_NAME`
:    The name of the action. To include the action in a package, enter the name in the format `PACKAGE_NAME`/`ACTION_NAME`. This value is required. 

`APP_FILE`
:    The path to the app file or package to run as an action. This option is required.

`--copy`
:    Treat the action as the name of an existing action.

`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`
:    The Docker Hub user name and the name of the Docker image in Docker Hub to run the action. This flag is required for creating actions from Docker images.

`--kind` `LANGUAGE`
:    The runtime for your app. This flag is optional. If no `VALUE` is specified, the default version for the detected runtime is used.
    Possible `VALUES` for the `--kind` option.
        | Language | Kind identifier |
| ---- | -------------- |
| Node.js | `nodejs:16` (default) |
| Python | `python:3.9` (default) |
| Swift | `swift:4.2` (default) |
| PHP | `php:7.4` (default) |
| Go | `go:1.17` (default) |
| Ruby | `ruby:2.6` (default) |
| Java | `java (JDK 8)` (default) |
| .NET Core | `dotnet:2.2` (default) |
{: caption="Table 1. Supported runtimes" caption-side="bottom"}

Other languages are supported by using Docker actions.
        

`--logsize` `LIMIT`, `-l` `LIMIT`
:    The maximum log size in MB for the action. The default value is 10 MB.

`--main` `ENTRY_METHOD_NAME`
:    If the action's entry method is not `main`, specify the custom name. This flag is required when the entry method is not `main`. For some runtimes, such as Java, the name must be the fully qualified method.

`--memory` `MEMORY_LIMIT`
:    The maximum memory limit in MB for your action. The default is 256 MB.

`--native`
:    You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. By using this argument, you can create and deploy an executable that runs inside the standard Docker action SDK. \n - When you create a Docker image, an executable is created inside the container at `/action/exec`. Copy the `/action/exec` file to your local file system and compress it into `exec.zip`. \n - Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--sequence` `ACTION_NAME`, `ACTION_NAME`
:    Create an action sequence and include the names of the related actions. Separate the `ACTION_NAMEs` by commas.

`--timeout` `LIMIT`, `-t` `LIMIT`
:    The timeout `LIMIT` in milliseconds. The default value is 60000 milliseconds. When the timeout is reached, the action is terminated.

`--web yes|true|raw|no|false`
:    Treat the action as a web action, a raw HTTP web action, or as a standard action. Specify `yes` or `true` for a web action, `raw` for a raw HTTP web action, or `no` or `false` for a standard action. To secure your web action, also include the `--web-secure` option.

`--web-secure` `SECRET`
:    Secure the web action. The `VALUE` for `SECRET` can be `true`, `false`, or any string. This option can be used only with the `--web` option.


#### Example
{: #cli_action_create_example}

```sh
ibmcloud fn action create hello folder/hello_world.js
```
{: pre}

Output

```sh
ok: created hello
```
{: screen}

### `ibmcloud fn action delete`
{: #cli_action_delete}

You can clean up your namespace by deleting actions that you do not want to use any longer.
{: shortdec}

```sh
ibmcloud fn action delete ACTION_NAME
```
{: pre}

#### Example
{: #cli_action_delete_example}

```sh
ibmcloud fn action delete helloworld
```
{: pre}

Output

```sh
ok: deleted hello
```
{: screen}

### `ibmcloud fn action get`
{: #cli_action_get}

Get metadata that describes a specific action.
{: shortdec}

```sh
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

#### Command options
{: #cli_action_get_command}

`ACTION_NAME`
:    The name of an action. This value is required.

`--save`
:    You can get and locally save the code that is associated with an existing action, except for sequences and Docker actions. The `FILENAME` corresponds with an existing action name in the current working directory and the file extension  corresponds to the action kind. For example, for action code that is an archive file, an extension of .zip is used. This flag is optional.

`--save-as` `FILENAME`
:    Save the code for actions in a custom-named file by providing a file path, `FILENAME`, and extension. This flag is optional.

`--summary`
:    Get a summary of the action details. Parameters with the prefix "*" are bound; parameters with the prefix "**" are bound and finalized. This flag is optional.

`--url`
:    Get the URL only for the action. This flag is optional.


#### Example
{: #cli_action_get_example}

```sh
ibmcloud fn action get hello
```
{: pre}

Output

```sh
ok: got action hello
{
    "namespace": "<namespace_ID>",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:10",
        "binary": false
    },
    "annotations": [
        {
            "KEY": "exec",
            "VALUE": "nodejs:10"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

Run an action to test it.
{: shortdec}

```sh
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

#### Command options
{: #cli_action_invoke_command}

`ACTION_NAME`
:    The name of the action. This value is required. 

`--blocking, -b`
:    Blocking invocations use a request and response style to wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time `LIMIT` `VALUE`](/docs/openwhisk?topic=openwhisk-limits). This flag is optional.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--result, -r`
:    The result of the app code is displayed as the output of the command. If this option is not specified, the activation ID is displayed. The invocation is blocking when this option is specified. This flag is optional.



#### Example
{: #cli_action_invoke_example}

```sh
ibmcloud fn action invoke hello --blocking
```
{: pre}

### `ibmcloud fn action list`
{: #cli_action_list}

List all the actions that you created or a specific number of actions.
{: shortdec}

```sh
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

#### Command options
{: #cli_action_list_command}

`ACTION_NAME`
:    The name of a package of actions. This value is optional. If not specified, all actions are listed.

`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`
:    List a specified number of actions. The default is 30 actions.

`--name-sort, -n`
:    Sort the list of returned actions by name, otherwise the list is sorted by creation date.

`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`
:    Exclude a specified number of the most recently created actions from the result.



#### Example
{: #cli_action_list_example}

```sh
ibmcloud fn action list
```
{: pre}

### `ibmcloud fn action update`
{: #cli_action_update}

Update an action or the app within an action.
{: shortdec}

When you update parameters for a package or action, you must specify all previously created parameters. Otherwise, the previously created parameters are removed. For packages, any services that were bound to the package are also removed, so after you update other parameters you must [bind services](/docs/openwhisk?topic=openwhisk-services) to your package again.
{: important}

```sh
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--memory MEMORY_LIMIT] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

#### Command options
{: #cli_action_update_command}

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`ACTION_NAME`
:    The name of the action. To include the action in a package, enter the name in the format `PACKAGE_NAME`/`ACTION_NAME`. This value is required. 

`APP_FILE`
:    The path to the app file or package to run as an action. This option is required when you want to update your app within the action.

`--copy`
:    Treat the action as the name of an existing action.

`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`
:    The Docker Hub user name and the name of the Docker image in Docker Hub to run the action. This flag is required for creating actions from Docker images.

`--kind LANGUAGE`
:    The runtime for your app. This flag is optional. If no VALUE is specified, the default version for the detected runtime is used.
    Possible values for the `--kind` option.
    | Language | Kind identifier |
| ---- | -------------- |
| Node.js | `nodejs:16` (default) |
| Python | `python:3.9` (default) |
| Swift | `swift:4.2` (default) |
| PHP | `php:7.4` (default) |
| Go | `go:1.17` (default) |
| Ruby | `ruby:2.6` (default) |
| Java | `java (JDK 8)` (default) |
| .NET Core | `dotnet:2.2` (default) |
{: caption="Table 1. Supported runtimes" caption-side="bottom"}

Other languages are supported by using Docker actions.
        

`--logsize` `LIMIT`, `-l` `LIMIT`
:    The maximum log size in MB for the action. The default value is 10 MB.

`--main ENTRY_METHOD_NAME`
:    If the action's entry method is not `main`, specify the custom name. This flag is required when the entry method is not `main`. For some runtimes, such as Java, the name must be the fully qualified method.

`--memory MEMORY_LIMIT`
:    The maximum memory limit in MB for your action. The default is 256 MB.

`--native`
:    You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. By using this argument, you can create and deploy an executable that runs inside the standard Docker action SDK. \n - When you create a Docker image, an executable is created inside the container at `/action/exec`. Copy the `/action/exec` file to your local file system and compress it into `exec.zip`. \n - Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameters `KEYS` and `VALUES`. This flag is optional.

`--sequence` `ACTION_NAME`, `ACTION_NAME`
:    Create an action sequence by specifying the name of related actions.

`--timeout` `LIMIT`, `-t` `LIMIT`
:    The timeout limit in milliseconds. The default value is 60000 milliseconds. When the timeout is reached, the action is terminated.

`--web yes|true|raw|no|false`
:    Treat the action as a web action, a raw HTTP web action, or as a standard action. Specify `yes` or `true` for a web action, `raw` for a raw HTTP web action, or `no` or `false` for a standard action. To secure your web action, also include the `--web-secure` option.

`--web-secure` `SECRET`
:    Secure the web action. The `VALUE` for `SECRET` can be `true`, `false`, or any string. This option can be used only with the `--web` option.


#### Example
{: #cli_action_update_example}

```sh
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}

## Activation commands
{: #cli_activation}

Get metadata, list IDs, find logs, and list results for specific activations.
{: shortdec} 

To see CLI help for the **`activation`** command, run `ibmcloud fn activation`.
{: tip}

### `ibmcloud fn activation get`
{: #cli_activation_get}

Get metadata that describes a specific activation.
{: shortdec}

```sh
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}

#### Command options
{: #cli_activation_get_command}

`ACTIVATION_ID`
:    The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified..

`FIELD_FILTER`
:    A field in the metadata to display information from. For example, to display the logs field, run `ibmcloud fn activation get ACTIVATION_ID logs`. This value is optional.

`--last, -l`
:    Display the metadata for the most recent activation. This flag is optional.

`--summary, -s`
:    Display the result response only from the activation details. This flag is optional.


#### Example
{: #cli_activation_get_example}

```sh
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}

### `ibmcloud fn activation list`
{: #cli_activation_list}

List all the activation IDs for all the actions in a package.
{: shortdec}

```sh
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}

#### Command options
{: #cli_activation_list_command}

`--full, -f`
:    Display the full activation description.

`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`
:    List a specified number of activations. The default is 30 activations and the maximum is 200 activations.

`--since` `UNIX_EPOCH_TIME`
:    List activations that were created since the date specified. Duration is measured in milliseconds since 01 January 1970. Example: `1560371263` is 12 June 2019 08:27:43 UTC.

`--skip` `NUMBER_OF_ACTIVATIONS`, `-s NUMBER_OF_ACTIVATIONS`
:    Exclude a specified number of the most recent activations from the result.

`--upto` `UNIX_EPOCH_TIME`
:    List activations that were created before the date specified. Duration is measured in milliseconds since 01 January 1970. Example: `1560371263` is 12 June 2019 08:27:43 UTC.


#### Example
{: #cli_activation_list_example}

```sh
ibmcloud fn activation list
```
{: pre}

Output

```sh
activations
44794bd6aab74415b4e42a308d880e5b         hello
6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

Get logs for a specific activation.
{: shortdec}

```sh
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

#### Command options
{: #cli_activation_logs_command}

`ACTIVATION_ID`
:    The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified.

`--last, -l`
:    Display the logs for the most recent activation. This flag is optional.

`--strip, -r`
:    Display the log message only; exclude the timestamp and stream information. This flag is optional.


#### Example
{: #cli_activation_logs_example}

```sh
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

View a streaming, live list of activations for an action or a namespace. You can press `CTRL+C` to exit the polling.
{: shortdec}

```sh
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

#### Command options
{: #cli_activation_poll_command}

/`NAMESPACE`
:    A namespace name or ID. Poll activations for a namespace, an action, or a space. This value is optional. If a namespace or action is not specified, the space is polled.

`ACTION_NAME`
:    An action name. Poll activations for a namespace, an action, or a space. This value is optional. If a namespace or action is not specified, the space is polled.

`--exit` `SECONDS`, `-e` `SECONDS`
:    Poll activations for a specified number of seconds and then exit. This flag is optional.

`--since-days` `DAYS`
:    Start polling for activations a specified number of days ago. This flag is optional.

`--since-hours` `HOURS`
:    Start polling for activations a specified number of hours ago. This flag is optional.

`--since-minutes` `MINUTES`
:    Start polling for activations a specified number of minutes ago. This flag is optional.

`--since-seconds` `SECONDS`
:    Start polling for activations a specified number of seconds ago. This flag is optional.


#### Example
{: #cli_activation_poll_example}

```sh
ibmcloud fn activation poll
```
{: pre}

### `ibmcloud fn activation result`
{: #cli_activation_result}

Get the result from a specific activation.
{: shortdec}

```sh
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

#### Command options
{: #cli_activation_result_command}

`ACTIVATION_ID`
:    The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified.

`--last, -l`
:    Display the result for the most recent activation. This flag is optional.


#### Example
{: #cli_activation_result_example}

```sh
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



## Deployment commands
{: #cli_deploy_cmds}

Use deployment commands to deploy and undeploy packages, actions, triggers, and rules.
{: shortdec}

### `ibmcloud fn deploy`
{: #cli_deploy}

Use a manifest file to deploy a collection of packages, actions, triggers, and rules.
{: shortdec}

To see CLI help for the **`deploy`** command, run `ibmcloud fn deploy`.
{: tip}

```sh
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE][--deployment FILE] [--manifest FILE] [--namespace NAMESPACE][--param KEY VALUE] [--param-file FILE] [--preview][--project PATH] [--strict] [--verbose]
```
{: pre}

#### Command options
{: #cli_deploy_command}

`--apihost HOST`
:    The `wsk` API host. This flag is optional.

`--auth` `KEY`, `-u` `KEY`
:    The `wsk` authorization `KEY`. This flag is optional.

`--config` `FILE`
:    The configuration file. The default is `$HOME/.wskprops`.

`--deployment` `FILE`
:    The path to the deployment file.

`--manifest` `FILE`, `-m` `FILE`
:    The path to the manifest file. This flag is required if the manifest.yaml is not in the current directory.

`--namespace` `NAMESPACE`, `-n` `NAMESPACE`
:    The name or ID for a namespace.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--preview` 
:    Display the deployment plan before you deploy.

`--project PATH`
:    The path to the serverless project. The default is `.` (current directory).

`--strict`
:    Allow a user-defined runtime version.

`--verbose, -v`
:    View verbose output.


#### Example
{: #cli_deploy_example}

```sh
ibmcloud fn deploy --manifest folder/manifest.yaml
```
{: pre}

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Use a manifest file to undeploy a collection of packages, actions, triggers, and rules.
{: shortdec}

To see CLI help for the `undeploy` command, run `ibmcloud fn undeploy`.
{: tip}

```sh
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

#### Command options
{: #cli_undeploy_command}

`--apihost HOST`
:    The `wsk` API host. This flag is optional.

`--auth` `KEY`, `-u` `KEY`
:    The `wsk` authorization `KEY`. This flag is optional.

`--config` `FILE`
:    The configuration file. The default is `$HOME/.wskprops`.

`--deployment` `FILE`
:    The path to the deployment file.

`--manifest` `FILE`, -m `FILE`
:    The path to the manifest file. This flag is required if the manifest.yaml is not in the current directory.

`--namespace` `NAMESPACE`, `-n` `NAMESPACE`
:    The name or ID for a namespace.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--preview` 
:    Display the result of the command without running the command.

`--project PATH`
:    The path to the serverless project. The default is `.` (current directory).

`--strict`
:    Allow a user-defined runtime version.

`--verbose, -v`
:    View verbose output.



#### Example
{: #cli_undeploy_example}

```sh
ibmcloud fn undeploy --manifest folder/manifest.yaml
```
{: pre}

## List command
{: #cli_list_cmd}

Use the **`list`** command to view packages, actions, triggers, and rules in the namespace.
{: shortdec}

### `ibmcloud fn list`
{: #cli_list}

View a grouped list of the packages, actions, triggers, and rules in the namespace.
{: shortdec}

```sh
ibmcloud fn list [--name-sort]
```
{: pre}

#### Command options
{: #cli_list_command}

`--name-sort, -n`
:    Sort each group of returned entities by name, otherwise each group is sorted by creation date.


#### Example
{: #cli_list_example}

```sh
ibmcloud fn list
```
{: pre}

## Namespace commands
{: #cli_namespace}

Create, update, delete, and find information about your namespace.
{: shortdec}

To learn how to target your {{site.data.keyword.openwhisk_short}} namespace, see [Targeting namespaces](/docs/openwhisk?topic=openwhisk-namespaces#targeting-namespaces).
{: note}

To see CLI help for the **`namespace`** command, run `ibmcloud fn namespace`.
{: tip}

### `ibmcloud fn namespace create`
{: #cli_namespace_create}

Create an IAM namespace.
{: shortdec}

```sh
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

#### Command options
{: #cli_namespace_create_command}


`NAMESPACE`
:    The name for a namespace. Do not include hyphens (-) in the name. This value is required.

`--description DESCRIPTION`, `-n DESCRIPTION`
:    Write your own unique description to help you identify the namespace. If your description is more than one word, include quotation marks (") around your description. This flag is optional.



#### Example
{: #cli_namespace_create_example}

```sh
ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
```
{: pre}

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

Delete an IAM namespace.
{: shortdec}

```sh
ibmcloud fn namespace delete NAMESPACE
```
{: pre}

#### Example
{: #cli_namespace_delete_example}

```sh
ibmcloud fn namespace delete mynamespace
```
{: pre}

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Get the entities for or the metadata information from a Cloud Foundry or IAM namespace.
{: shortdec}

```sh
ibmcloud fn namespace get NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

#### Command options
{: #cli_namespace_get_command}

`NAMESPACE`
:    The name or ID for a namespace. This value is required.

`--auth` `KEY`, `-u` `KEY`
:    The `wsk` authorization `KEY`. This flag is optional.

`--name-sort, -n`
:    Sort the list of returned namespaces by name, otherwise the list is sorted by creation date. This flag is optional. 

`--properties`
:    Display the namespace properties instead of the entities contained within it. This flag is optional. 



#### Example
{: #cli_namespace_get_example}

```sh
ibmcloud fn namespace get user@domain.com_dev --properties
```
{: pre}

Output

```sh
Name: user@domain.com_dev
Description: This is a description of my namespace.
Resource Plan Id: functions-base-plan
Location: us-south
ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
```
{: screen}

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

List the available Cloud Foundry and IAM namespaces.
{: shortdec}

```sh
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

#### Command options
{: #cli_namespace_list_command}

`--auth` `KEY`, `-u` `KEY`
:    The `wsk` authorization `KEY`. This flag is optional.

`--cf`
:    Display the Cloud Foundry namespaces only. IAM namespaces are not displayed. This flag is optional.

`--iam`
:    Display the IAM namespaces only. Cloud Foundry namespaces are not displayed. This flag is optional.

`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`
:    List a specified number of namespaces. The default is 30 namespaces. This flag is optional. 

`--name-sort, -n`
:    Sort the list of returned namespaces by name, otherwise the list is sorted by creation date. This flag is optional. 

`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`
:    Exclude a specified number of the most recently created namespaces from the result. This flag is optional. 


#### Example
{: #cli_namespace_list_example}

```sh
ibmcloud fn namespace list
```
{: pre}

### `ibmcloud fn namespace target`
{: #cli_namespace_target}

Target an available IAM or Cloud Foundry namespace.
{: shortdec}

```sh
ibmcloud fn namespace target NAMESPACE
```
{: pre}

#### Command options
{: #cli_namespace_target_command}

`NAMESPACE`
:    The name or ID for a namespace. This value is required.


#### Example
{: #cli_namespace_target_example}

```sh
ibmcloud fn namespace target HBCTeamProd
```
{: pre}


### `ibmcloud fn namespace update`
{: #cli_namespace_update}

Change the name or description of an IAM namespace.
{: shortdec}

```sh
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION]
```
{: pre}

#### Command options
{: #cli_namespace_update_command}

`NAMESPACE`
:    The name for a namespace. Do not include hyphens (-) in the name. This value is required.

`NEW_``NAMESPACE``_NAME`
:    The new name for a namespace. Do not include hyphens (-) in the name. This value is optional.

`--description DESCRIPTION`, `-n DESCRIPTION`
:    Write your own unique description to help you identify the namespace. If your description is more than one word, include quotation marks (") around your description. This flag is optional.


#### Example
{: #cli_namespace_update_example}

```sh
ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
```
{: pre}

## Package commands
{: #cli_pkg}

Create, update, delete, bind, and find information about packages.
{: shortdec}

To see CLI help for the **`package`** command, run `ibmcloud fn package`.
{: tip}

### `ibmcloud fn package bind`
{: #cli_pkg_bind}

Bind parameters to a package. All the actions within the package inherit those parameters unless otherwise specified.
{: shortdec}

```sh
ibmcloud fn package bind PACKAGE_NAME BOUND_PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

#### Command options
{: #cli_pkg_bind_command}

`PACKAGE_NAME`
:    The name of the package. This value is required. 

`BOUND_PACKAGE_NAME`
:    The name of the package binding. This value is required. 

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.


#### Example
{: #cli_pkg_bind_example}

```sh
ibmcloud fn package bind hellopkg hellopkgbind --param name Bob
```
{: pre}

### `ibmcloud fn package create`
{: #cli_pkg_create}

Create a package designed to contain one or more actions. To add an action in the package, include the package name with the action name when you create or update the action.
{: shortdec}

```sh
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

#### Command options
{: #cli_pkg_create_command}

`PACKAGE_NAME`
:    The name of the package. This value is required. 

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` `VALUE` format. This flag is optional.

`--shared yes|no`
:    When specified without a value or with a value of yes, the package is shared with other users.


#### Example
{: #cli_pkg_create_example}

```sh
ibmcloud fn package create hellopkg
```
{: pre}

Output

```sh
ok: created hellopkg
```
{: screen}

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

You can clean up your namespace by deleting packages that you do not want to use any longer.
{: shortdec}

```sh
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

#### Example
{: #cli_pkg_delete_example}

```sh
ibmcloud fn package delete hello
```
{: pre}

Output

```sh
ok: deleted hello
```
{: screen}

### `ibmcloud fn package get`
{: #cli_pkg_get}

Get metadata that describes a specific package.
{: shortdec}

```sh
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

#### Command options
{: #cli_pkg_get_command}

`PACKAGE_NAME`
:    The name of a package. This value is required.

`--summary`
:    Get a summary of the package details. Parameters with the prefix "*" are bound. This flag is optional.


#### Example
{: #cli_pkg_get_example}

```sh
ibmcloud fn package get hello
```
{: pre}

### `ibmcloud fn package list`
{: #cli_pkg_list}

List all the packages that you created or a specific number of packages.
{: shortdec}

```sh
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

#### Command options
{: #cli_pkg_list_command}

`/NAMESPACE`
:    List the packages in a specific namespace ID. This value must begin with a `/`. This value is optional. If not specified, all packages are listed.

`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`
:    List a specified number of packages. The default is 30 packages.

`--name-sort, -n`
:    Sort the list of returned packages by name, otherwise the list is sorted by creation date.

`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`
:    Exclude a specified number of the most recently created packages from the result.


#### Example
{: #cli_pkg_list_example}

```sh
ibmcloud fn package list
```
{: pre}

Run `ibmcloud fn package list /whisk.system` to view a list of preinstalled packages.
{: tip}

### `ibmcloud fn package refresh`
{: #cli_pkg_refresh}

Refresh the package bindings for all the packages within a specific namespace.
{: shortdec}

```sh
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

#### Command options
{: #cli_pkg_refresh_command}


`/NAMESPACE`
:    A namespace ID, beginning with /. This flag is required. Run `ibmcloud fn namespace list` to get a list of namespaces to choose from.


#### Example
{: #cli_pkg_refresh_example}

```sh
ibmcloud fn package refresh /user@domain.com_dev
```
{: pre}

### `ibmcloud fn package update`
{: #cli_pkg_update}

Update a package designed to contain one or more actions. To add an action in the package, include the package name with the action name when you create or update the action.
{: shortdec}

When you update parameters for a package or action, you must specify all previously created parameters. Otherwise, the previously created parameters are removed. For packages, any services that were bound to the package are also removed, so after you update other parameters you must [bind services](/docs/openwhisk?topic=openwhisk-services) to your package again.
{: important}

```sh
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

#### Command options
{: #cli_pkg_update_command}

`PACKAGE_NAME`
:    The name of the package. This value is required. 

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--shared yes|no`
:    When specified without a value or with a value of `yes`, the package is shared with other users.


#### Example
{: #cli_pkg_update_example}

```sh
ibmcloud fn package create hellopkg
```
{: pre}

Output

```sh
ok: created hellopkg
```
{: screen}

## Property commands
{: #cli_prop}

Set global properties for your CLI environment or view properties about the `wsk` CLI, which runs as part of the `ibmcloud fn` CLI.
{: shortdec}

To see CLI help for the **`property`** command, run `ibmcloud fn property`.
{: tip}

### `ibmcloud fn property get`
{: #cli_prop_get}

View the metadata details for a property from the `wsk` CLI.
{: shortdec}

```sh
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

#### Command options
{: #cli_prop_get_command}

`--all`
:    View all properties for the `wsk` CLI. This flag is optional.

`---apibuild`
:    The `wsk` API build information. This flag is optional.

`--apibuildno`
:    The `wsk` API build number. This flag is optional.

`--apihost` `HOST`
:    The `wsk` API host. This flag is optional.

`--apiversion` `VERSION`
:    The `wsk` API version. This flag is optional.

`--auth` `KEY`, `-u` `KEY`
:    The `wsk` authorization `KEY`. This flag is optional.

`--cert` `STRING`
:    The `wsk` client certificate. This flag is optional.

`--cliversion`
:    The `wsk` CLI version. This flag is optional.

`--key` `STRING`
:    The `wsk` client `KEY`. This flag is optional.

`--namespace` `NAMESPACE`
:    An IAM namespace name or ID. This flag cannot be set for Cloud Foundry namespaces. This flag is optional.


#### Example
{: #cli_prop_get_example}

```sh
ibmcloud fn property get --auth
```
{: pre}

### `ibmcloud fn property set`
{: #cli_prop_set}

Set a property. At least one flag is required. After a property is set, it is retained on your workstation at `<home_dir>/.bluemix/plugins/cloud-functions/config.json`. To remove a property, run [`ibmcloud fn property unset --<property>`](#cli_prop_set).
{: shortdec}

```sh
ibmcloud fn property set [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

#### Command options
{: #cli_prop_set_command}

`--auth` `KEY`, -u
:    The `wsk` authorization `KEY`. This flag is optional.

`--cert` `STRING`
:    The `wsk`client certificate. This flag is optional.

`--key` `STRING`
:    The `wsk` client `KEY`. This flag is optional.

`--namespace` `NAMESPACE`
:    An IAM namespace name or ID. This flag cannot be set for Cloud Foundry namespaces. This flag is optional.


#### Example
{: #cli_prop_set_example}

```sh
ibmcloud fn property set --namespace myNamespace
```
{: pre}

Output

```sh
ok: whisk namespace set to myNamespace
```
{: screen}

### `ibmcloud fn property unset`
{: #cli_prop_unset}

Unset a property for the `wsk` CLI. At least one flag is required.
{: shortdec}

If properties are retained after running the `property unset` command, you can delete the `config.json` file located at `<home_dir>/.bluemix/plugins/cloud-functions/config.json` to remove all properties.
{: note}

```sh
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

#### Command options
{: #cli_prop_unset_command}

`--apihost` `HOST`
:    The `wsk` API host. This flag is optional.

`--apiversion` `VERSION`
:    The `wsk` API version. This flag is optional.

`--auth` `KEY`, `-u`
:    The `wsk` authorization `KEY`. This flag is optional.

`--cert` `STRING`
:    The `wsk` client certificate. This flag is optional.

`--key` `STRING`
:    The `wsk` client `KEY`. This flag is optional.

`--namespace` `NAMESPACE`
:    An IAM namespace name or ID. This flag cannot be set for Cloud Foundry namespaces. This flag is optional.


#### Example
{: #cli_prop_unset_example}

```sh
ibmcloud fn property unset --namespace
```
{: pre}

## Rule commands
{: #cli_rule}

Create, delete, enable, disable, update, and find information about rules.
{: shortdec}

To see CLI help for the **`rule`** command, run `ibmcloud fn rule`.
{: tip}

### `ibmcloud fn rule create`
{: #cli_rule_create}

Create a rule to associate a trigger with an action. Before you can create a rule, create a trigger and an action first.
{: shortdec}

```sh
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

#### Example
{: #cli_rule_create_example}

```sh
ibmcloud fn rule create myrule mytrigger myaction
```
{: pre}

Output

```sh
ok: created myrule
```
{: screen}

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

To clean up your namespace, remove rules you no longer need.
{: shortdec}

```sh
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

#### Command options
{: #cli_rule_delete_command}

`RULE_NAME`
:    The name of a rule. This value is required.

`--disable`
:    Disable the rule before you delete it.


#### Example
{: #cli_rule_delete_example}

```sh
ibmcloud fn rule delete myrule
```
{: pre}

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

Change the status of a rule to inactive and stop it from running an action when a trigger is fired.
{: shortdec}

```sh
ibmcloud fn rule disable RULE_NAME
```
{: pre}

#### Example
{: #cli_rule_disable_example}

```sh
ibmcloud fn rule disable myrule
```
{: pre}

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

Change the status of a rule from inactive to active. When active, an action runs when a trigger is fired.
{: shortdec}

```sh
ibmcloud fn rule enable RULE_NAME
```
{: pre}

#### Example
{: #cli_rule_enable_example}

```sh
ibmcloud fn rule enable myrule
```
{: pre}

### `ibmcloud fn rule get`
{: #cli_rule_get}

Get metadata that describes a specific rule.
{: shortdec}

```sh
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

#### Command options
{: #cli_rule_get_command}

`RULE_NAME`
:    The name of a rule. This value is required.

`--summary`
:    Get a summary of the rule details.


#### Example
{: #cli_rule_get_example}

```sh
ibmcloud fn rule get myrule
```
{: pre}

### `ibmcloud fn rule list`
{: #cli_rule_list}

List all the rules that you created or a specific number of rules.
{: shortdec}

```sh
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

#### Command options
{: #cli_rule_list_command}

`RULE_NAME`
:    The name of a rule. This value is optional. If not specified, all rules are listed.

`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`
:    List a specified number of rules. The default is 30 rules.

`--name-sort`, `-n`
:    Sort the list of returned rules by name, otherwise the list is sorted by creation date.

`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`
:    Exclude a specified number of the most recently created rules from the result.


#### Example
{: #cli_rule_list_example}

```sh
ibmcloud fn rule list
```
{: pre}

### `ibmcloud fn rule status`
{: #cli_rule_status}

See whether a rule is active or inactive. Run the `ibmcloud fn rule disable` or `ibmcloud fn run enable` commands to change the status.
{: shortdec}

```sh
ibmcloud fn rule status RULE_NAME
```
{: pre}

#### Example
{: #cli_rule_status_example}

```sh
ibmcloud fn rule status myrule
```
{: pre}

### `ibmcloud fn rule update`
{: #cli_rule_update}

To change which triggers are associated with which rules, you can update a rule.
{: shortdec}

```sh
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

#### Example
{: #cli_rule_update_example}

```sh
ibmcloud fn rule update myrule mytrigger myaction
```
{: pre}

## SDK command
{: #cli_sdk}

Install SDK components, such as `docker`, `iOS`, and `bashauto`.
{: shortdec}

To see CLI help for the **`sdk`** command, run `ibmcloud fn sdk`.

### `ibmcloud fn sdk install`
{: #cli_sdk_install}

Install an SDK.
{: shortdec}

```sh
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

#### Command options
{: #cli_sdk_install_command}

`COMPONENT`
:    The SDK component, such as `docker`, `iOS`, and `bashauto`. This value is required.

`--stdout, --s`
:    Prints the bash command results to `STDOUT`. This flag is optional.


#### Example
{: #cli_sdk_install_example}

```sh
ibmcloud fn sdk install docker
```
{: pre}

## Service commands
{: #cli_service}

Service commands bind and unbind service credentials to an action or package.
{: shortdec}

To see CLI help for the **`service`** command, run `ibmcloud fn service`.
{: tip}

If you receive the error `Unable to refresh user access token: CloudFoundry API endpoint is not set` when running the `service` command, run `ibmcloud target --cf`.
{: note}

### `ibmcloud fn service bind`
{: #cli_service_bind}

Bind service credentials to an action or package.
{: shortdec}

```sh
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

#### Command options
{: #cli_service_bind_command}

`SERVICE`
:    The name of the service.

`PACKAGE_or_ACTION_NAME`
:    The name of the package or action to bind the credentials to.

`--instance SERVICE_INSTANCE`
:    The service instance name.

`--keyname SERVICE_KEY`
:    The name of the service `KEY` credentials to bind.


#### Example
{: #cli_service_bind_example}

```sh
ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
```
{: pre}

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

Unbind service credentials from an action or package.
{: shortdec}

```sh
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

#### Command options
{: #cli_service_unbind_command}

`SERVICE`
:    The name of the service.

`PACKAGE_or_ACTION_NAME`
:    The name of the package or action to unbind the credentials from.


#### Example
{: #cli_service_unbind_example}

```sh
ibmcloud fn service unbind cloudant hello
```
{: pre}

## Trigger commands
{: #cli_trigger}

Create, update, delete, fire, and find information about triggers.
{: shortdec}

To see CLI help for the **`trigger`** command, run `ibmcloud fn trigger`.

### `ibmcloud fn trigger create`
{: #cli_trigger_create}

Create a trigger.
{: shortdec}

```sh
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE] [--trigger-param KEY VALUE] [--feed-param KEY VALUE]
```
{: pre}

#### Command options
{: #cli_trigger_create_command}

`TRIGGER_NAME`
:    The name of the trigger. This value is required. 

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`--feed ACTION_NAME`, `-f ACTION_NAME`
:    Sets the type of trigger as a feed. This flag is optional.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--trigger-param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Trigger parameter values in the `KEY` `VALUE` format. This flag is optional.

`--feed-param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Feed parameter values in the `KEY` `VALUE` format. This flag is optional.


#### Examples
{: #cli_trigger_create_examples}

```sh
ibmcloud fn trigger create mytrigger --param name Bob 
```
{: pre}

```sh
ibmcloud fn trigger create mytrigger --trigger-param name Bob
```
{: pre}

Starting in functions plug-in CLI version 1.0.38, two new options are available for the trigger command: `--trigger-param` and `--feed-param`. These options are an extension to creating and updating a trigger, making the `trigger create` and `update` commands more flexible.

Previously, you created and updated a trigger with a parameter on it with the following command: 

```sh
ibmcloud fn trigger create triggerHelloWorld --param msg “Hello World!” 
```
{: pre}

This command creates a trigger called `triggerHelloWorld` with a parameter of KEY `msg` and VALUE of `Hello World!`. This is very simple and straightforward. However, it becomes a little complicated when you create a trigger that contains a feed, especially when you want to add parameters on both the trigger and the trigger feed. For example, if you want to create a trigger with alarm feed, then you must run a command similar to the following example:

```sh
ibmcloud fn trigger create triggerCron --feed /whisk.system/alarms/alarm --param cron “0,1,2,3,4,5”
```
{: pre}

In this case, the KEY and VALUE pair that follows `--param` are consumed by feed and are treated as feed parameters. By using the new options, you can differentiate between trigger parameters and feed parameters. The following command creates a trigger called `triggerCron` with cron feed parameters of `0,1,2,3,4,5` and a trigger parameter of KEY `msg` and VALUE of `Hello World!`.

```sh
ibmcloud fn  trigger create triggerCron --feed /whisk.system/alarms/alarm --feed-param cron “0,1,2,3,4,5” --trigger-param msg “Hello World!”
```
{: pre}

The original `--param` option is not deprecated so you can continue to use it as you have previously. You simply now have the ability to separate your trigger and feed parameters by using the new options. However, please do not mix `--param` or `--param-file` options with either `--trigger-param` or `--feed-param` options. These options are considered two different ways of setting trigger parameters and must not be used together. 
{: note}

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

Delete a trigger.
{: shortdec}

```sh
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

#### Example
{: #cli_trigger_delete_example}

```sh
ibmcloud fn trigger delete mytrigger
```
{: pre}

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

Test a trigger by firing it, rather than waiting for it to be triggered automatically.
{: shortdec}

```sh
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

#### Command options
{: #cli_trigger_fire_command}

`TRIGGER_NAME`
:    The name of the trigger. This value is required. 

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter `VALUES` in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.


#### Example
{: #cli_trigger_fire_example}

```sh
ibmcloud fn trigger fire --param name Bob
```
{: pre}

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

Get metadata that describes a specific trigger.
{: shortdec}

```sh
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

#### Command options
{: #cli_trigger_get_command}

`TRIGGER_NAME`
:    The name of a trigger. This value is required.

`--summary`
:    Get a summary of the trigger details.


#### Example
{: #cli_trigger_get_example}

```sh
ibmcloud fn trigger get mytrigger
```
{: pre}

### `ibmcloud fn trigger list`
{: #cli_trigger_list}

List all the triggers that you created or a specific number of triggers.
{: shortdec}

```sh
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

#### Command options
{: #cli_trigger_list_command}

`RULE_NAME`
:    The name of a trigger. This value is optional. If not specified, all triggers are listed.

`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`
:    List a specified number of triggers. The default is 30 triggers.

`--name-sort, -n`
:    Sort the list of returned triggers by name, otherwise the list is sorted by creation date.

`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`
:    Exclude a specified number of the most recently created triggers from the result.


#### Example
{: #cli_trigger_list_example}

```sh
ibmcloud fn trigger list
```
{: pre}

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

Update a trigger.
{: shortdec}

When you update trigger parameters (Note: this is different than trigger feed parameters) that use the `--trigger-parameter` or `--param-file` option, you must specify all previously created parameters. Otherwise, the previously created parameters are removed.  For more information, see [Create trigger](#cli_trigger_create).
{: important}

```sh
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE] [--trigger-param KEY VALUE] [--feed-param KEY VALUE]
```
{: pre}

#### Command options
{: #cli_trigger_update_command}

`TRIGGER_NAME`
:    The name of the trigger. This value is required. 

`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`
:    Annotations are specified in a `KEY` `VALUE` format. To include more than one annotation, specify this option for each annotation. This flag is optional.

`--annotation-file` `FILE`, `-A` `FILE`
:    A JSON file that contains annotation in a `KEY` `VALUE` format. This flag is optional.

`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Parameter values in the `KEY` `VALUE` format. This flag is optional.

`--param-file` `FILE`, `-P` `FILE`
:    A JSON file that contains parameter `KEYS` and `VALUES`. This flag is optional.

`--trigger-param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Trigger parameter values in the `KEY` `VALUE` format. This flag is optional.

`--feed-param` `KEY` `VALUE`, `-p` `KEY` `VALUE`
:    Feed parameter values in the `KEY` `VALUE` format. This flag is optional.


#### Examples
{: #cli_trigger_update_example}

```sh
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}

```sh
ibmcloud fn trigger update mytrigger --trigger-param name Jim
```
{: pre}

```sh
ibmcloud fn trigger update mytrigger --feed-param cron "0,1,2,3,4"
```
{: pre}




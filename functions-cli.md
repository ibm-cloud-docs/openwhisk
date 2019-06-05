---

copyright:
  years: 2017, [{CURRENT_<em>YEAR</em>}]
lastupdated: "2019-06-05"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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



# {{site.data.keyword.openwhisk_short}} CLI
{: #functions-cli}

Run these commands to manage the entities that make up your functions.
{: shortdec}



<br />

## Action commands
{: #cli_action}



### ibmcloud fn action create
{: #cli_action_create}

Create an action.

```
ibmcloud fn action create <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>A JSON file that contains annotation in a <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of the action. To include the action in a package, enter the name in the format <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em>. This value is required. </dd>

   <dt><em>APP_FILE</em></dt>
   <dd>The path to the app file or package to run as an action. This option is required.</dd>
   
   <dt>--copy</dt>
   <dd>Treat the action as the name of an existing action.</dd>

   <dt>--docker <em>DOCKER_HUB_USERNAME</em>/<em>IMAGE_NAME</em></dt>
   <dd>The Docker Hub user name and the name of the Docker image in Docker Hub to run the action. This flag is required for creating actions from Docker images.</dd>

   <dt>--kind <em>LANGUAGE</em></dt>
   <dd>The runtime for your app. This flag is optional. If no <em>VALUE</em> is specified, the default version for the detected runtime is used.
     Possible <em>VALUES</em> for the --kind option:
     <table>
  <tr>
    <th>Language</th>
    <th>Kind identifier</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (default), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (default)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (default)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (default)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (default)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (default)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (default)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (default)</td>
  </tr>
  <tr>
    <td>Other languages are supported by using Docker actions.</td>
  </tr>
</table>
{: caption="Table 1. Supported runtimes" caption-side="top"}
       </dd>

   <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
   <dd>The maximum log size in MB for the action. The default value is 10 MB.</dd>

   <dt>--main <em>ENTRY_METHOD_NAME</em></dt>
   <dd>If the action's entry method is not `main`, specify the custom name. This flag is required when the entry method is not `main`. For some runtimes, such as Java, the name must be the fully-qualified method.</dd>

   <dt>--native</dt>
   <dd>You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. This argument allows you to create and deploy an executable that runs inside the standard Docker action SDK.
       <ol><li>When you create a Docker image, an executable is created inside the container located at `/action/exec`. Copy the `/action/exec` file to your local file system and compress it into `exec.zip`.</li>
       <li>Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.</li></ol>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

   <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
   <dd>Create an action sequence and include the names of related actions separated by commas.</dd>

   <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
   <dd>The timeout <em>LIMIT</em> in milliseconds. The default value is 60000 milliseconds. When the timeout is reached, the action is terminated.</dd>

   <dt>--web yes|true|raw|no|false</dt>
   <dd>Treat the action as a web action, a raw HTTP web action, or as a standard action. Specify <code>yes</code> or <code>true</code> for a web action, <code>raw</code> for a raw HTTP web action, or <code>no</code> or <code>false</code> for a standard action. To secure your web action, also include the `--web-secure` option.</dd>

   <dt>--web-secure <em>SECRET</em></dt>
   <dd>Secure the web action. The <em>VALUE</em> for <em>SECRET</em> can be <em>true</em>, <em>false</em>, or any string. This option can be used only with the `--web` option.</dd>
   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  Output:
  ```
  ok: created hello
  ```
  {: screen}


<br />

### ibmcloud fn action delete
{: #cli_action_delete}

You can clean up your namespace by deleting actions that you do not want to use any longer.

```
ibmcloud fn action delete <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  Output:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn action get
{: #cli_action_get}

Get metadata that describes a specific action.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as <em>FILENAME</em>] [--summary] [--url]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of an action. This value is required.</dd>

   <dt>--save</dt>
   <dd>You can get and locally save code associated with an existing action, except for sequences and Docker actions. The <em>FILENAME</em> corresponds with an existing action name in the current working directory and the file extension  corresponds to the action kind. For example, for action code that is an archive file, an extension of .zip is used. This flag is optional.</dd>

  <dt>--save-as <em>FILENAME</em></dt>
  <dd>Save the code for actions in a custom-named file by providing a file path, <em>FILENAME</em>, and extension. This flag is optional.</dd>

  <dt>--summary</dt>
  <dd>Get a summary of the action details. Parameters with the prefix "*" are bound; parameters with the prefix "**" are bound and finalized. This flag is optional.</dd>

  <dt>--url</dt>
  <dd>Get the URL only for the action. This flag is optional.</dd>
   </dl>

<br /><strong>Example</strong>:

```
ibmcloud fn action get hello
```
{: pre}

Output:
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
            "<em>KEY</em>": "exec",
            "<em>VALUE</em>": "nodejs:6"
        }
    ],
    "<em>LIMIT</em>s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />
### ibmcloud fn action invoke
{: #cli_action_invoke}

Run an action to test it.

```
ibmcloud fn action invoke <em>ACTION_NAME</em> [--blocking] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--result]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of the action. This value is required. </dd>

   <dt>--blocking, -b</dt>
   <dd>Blocking invocations use a request and response style to wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time <em>LIMIT</em> <em>VALUE</em>](/docs/openwhisk?topic=cloud-functions-limits). This flag is optional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

   <dt>--result, -r</dt>
   <dd>The result of the app code is displayed as the output of the command. If this option is not specified, the activation ID is displayed. The invocation is blocking when this option is specified. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />
### ibmcloud fn action list
{: #cli_action_list}

List all of the actions that you created or a specific number of actions.

```
ibmcloud fn action list <em>ACTION_NAME</em> [--limit <em>NUMBER_OF_ACTIONS</em>] [--name-sort] [--skip <em>NUMBER_OF_ACTIONS</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of a package of actions. This value is optional. If not specified, all actions are listed.</dd>

   <dt>--limit <em>NUMBER_OF_ACTIONS</em>, -l <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>List a specified number of actions. The default is 30 actions.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned actions by name, otherwise the list is sorted by creation date.</dd>

   <dt>--skip <em>NUMBER_OF_ACTIONS</em>, -s <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>Exclude a specified number of the most recently created actions from the result.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />
### ibmcloud fn action update
{: #cli_action_update}

Update an action or the app within an action.

```
ibmcloud fn action update <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>Command options</strong>:

  <dl>
  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>A JSON file that contains annotation in a <em>KEY</em> VALE format. This flag is optional.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>The name of the action. To include the action in a package, enter the name in the format <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em>. This value is required. </dd>

  <dt><em>APP_FILE</em></dt>
  <dd>The path to the app file or package to run as an action. This option is required when you want to update your app within the action.</dd>

  <dt>--copy</dt>
  <dd>Treat the action as the name of an existing action.</dd>

  <dt>--docker DOCKER_HUB_USERNAME/IMAGE_NAME</dt>
  <dd>The Docker Hub user name and the name of the Docker image in Docker Hub to run the action. This flag is required for creating actions from Docker images.</dd>

  <dt>--kind LANGUAGE</dt>
  <dd>The runtime for your app. This flag is optional. If no <em>VALUE</em> is specified, the default version for the detected runtime is used.
    Possible values for the --kind option:
    <table>
  <tr>
    <th>Language</th>
    <th>Kind identifier</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (default), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (default)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (default)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (default)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (default)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (default)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (default)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (default)</td>
  </tr>
  <tr>
    <td>Other languages are supported by using Docker actions.</td>
  </tr>
</table>
{: caption="Table 1. Supported runtimes" caption-side="top"}
      </dd>

  <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
  <dd>The maximum log size in MB for the action. The default value is 10 MB.</dd>

  <dt>--main ENTRY_METHOD_NAME</dt>
  <dd>If the action's entry method is not `main`, specify the custom name. This flag is required when the entry method is not `main`. For some runtimes, such as Java, the name must be the fully-qualified method.</dd>

  <dt>--native</dt>
  <dd>You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. This argument allows you to create and deploy an executable that runs inside the standard Docker action SDK.
      <ol><li>When you create a Docker image, an executable is created inside the container located at `/action/exec`. Copy the `/action/exec` file to your local file system and compress it into `exec.zip`.</li>
      <li>Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.</li></ol>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

  <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
  <dd>Create an action sequence by specifying the name of related actions.</dd>

  <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
  <dd>The timeout limit in milliseconds. The default value is 60000 milliseconds. When the timeout is reached, the action is terminated.</dd>

  <dt>--web yes|true|raw|no|false</dt>
  <dd>Treat the action as a web action, a raw HTTP web action, or as a standard action. Specify <code>yes</code> or <code>true</code> for a web action, <code>raw</code> for a raw HTTP web action, or <code>no</code> or <code>false</code> for a standard action. To secure your web action, also include the `--web-secure` option.</dd>

  <dt>--web-secure <em>SECRET</em></dt>
  <dd>Secure the web action. The <em>VALUE</em> for <em>SECRET</em> can be <em>true</em>, <em>false</em>, or any string. This option can be used only with the `--web` option.</dd>
  </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Activation commands
{: #cli_activation}


### ibmcloud fn activation get
{: #cli_activation_get}

Get metadata that describes a specific activation.

```
ibmcloud fn activation get [<em>ACTIVATION_ID</em>] [<em>FIELD_FILTER</em>] [--last] [--summary]
```
{: pre}


<br /><strong>Command options</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified..</dd>

  <dt><em>FIELD_FILTER</em></dt>
  <dd>A field in the metadata to display information from. For example, to display the logs field, run `ibmcloud fn activation get ACTIVATION_ID logs`. This value is optional.</dd>

  <dt>--last, -l</dt>
  <dd>Display the metadata for the most recent activation. This flag is optional.</dd>

  <dt>--summary, -s</dt>
  <dd>Display the result response only from the activation details. This flag is optional.</dd>
  </dl>


<br /><strong>Example</strong>:
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation list
{: #cli_activation_list}

List all of the activation IDs for all of the actions in a package.

```
ibmcloud fn activation list [--full] [--limit <em>NUMBER_OF_ACTIVATIONS</em>] [--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>] [--skip <em>NUMBER_OF_ACTIVATIONS</em>] [--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>]
```
{: pre}


<br /><strong>Command options</strong>:

  <dl>
  <dt>--full, -f</dt>
  <dd>Display the full activation description</dd>

  <dt>--limit <em>NUMBER_OF_ACTIVATIONS</em>, -l <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>List a specified number of activations. The default is 30 activations and the maximum is 200 activations.</dd>

  <dt>--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>List activations that were created since the date specified. Measured in milliseconds. Example: Th, 01, Jan 1970</dd>

  <dt>--skip <em>NUMBER_OF_ACTIVATIONS</em>, -s <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>Exclude a specified number of the most recent activations from the result.</dd>

  <dt>--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>List activations that were created before the date specified. Measured in milliseconds. Example: Th, 01, Jan 1970</dd>
  </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn activation list
```
{: pre}

Output:
```
activations
44794bd6aab74415b4e42a308d880e5b         hello
6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />
### ibmcloud fn activation logs
{: #cli_activation_logs}

Get logs for a specific activation.

```
ibmcloud fn activation logs [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}

<br /><strong>Command options</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified.</dd>

  <dt>--last, -l</dt>
  <dd>Display the logs for the most recent activation. This flag is optional.</dd>

  <dt>--strip, -r</dt>
  <dd>Display the log message only; exclude the timestamp and stream information. This flag is optional.</dd>
  </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation poll
{: #cli_activation_poll}

View a streaming, live list of activations for an action or a namespace. You can press `CTRL+C` to exit the polling.

```
ibmcloud fn activation poll [/<em>NAMESPACE</em>] [<em>ACTION_NAME</em>] [--exit <em>SECONDS</em>] [--since-days <em>DAYS</em>] [-since-hours <em>HOURS</em>] [--since-minutes <em>MINUTES</em>] [--since-seconds <em>SECONDS</em>]
```
{: pre}

<br /><strong>Command options</strong>:

  <dl>
  <dt>/<em>NAMESPACE</em></dt>
  <dd>A namespace, beginning with /. Poll activations for a namespace, an action, or a space. This value is optional. If a namespace or action is not specified, the space is polled.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>Poll activations for a namespace, an action, or a space. This value is optional. If a namespace or action is not specified, the space is polled.</dd>

  <dt>--exit <em>SECONDS</em>, -e <em>SECONDS</em></dt>
  <dd>Poll activations for a specified number of seconds and then exit. This flag is optional.</dd>

  <dt>--since-days <em>DAYS</em></dt>
  <dd>Start polling for activations a specified number of days ago. This flag is optional.</dd>

  <dt>--since-hours <em>HOURS</em></dt>
  <dd>Start polling for activations a specified number of hours ago. This flag is optional.</dd>

  <dt>--since-minutes <em>MINUTES</em></dt>
  <dd>Start polling for activations a specified number of minutes ago. This flag is optional.</dd>

  <dt>--since-seconds <em>SECONDS</em></dt>
  <dd>Start polling for activations a specified number of seconds ago. This flag is optional.</dd>
  </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn activation poll
```
{: pre}


<br />
### ibmcloud fn activation result
{: #cli_activation_result}

Get the result from a specific activation.

```
ibmcloud fn activation result [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<br /><strong>Command options</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified.</dd>

  <dt>--last, -l</dt>
  <dd>Display the result for the most recent activation. This flag is optional.</dd>

  </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## API commands
{: #cli_api}


### ibmcloud fn api create
{: #cli_api_create}

Create an API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB <em>ACTION_NAME</em>] [--apiname API_NAME] [--config-file <em>FILE</em>] [--response-type TYPE]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>The base path for the API.</dd>

   <dt>API_NAME</dt>
   <dd>The name of the API. The API name might be the same as the base path.</dd>

   <dt>API_VERB</dt>
   <dd>The verb for the API, such as `get` or `post`.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of the action.</dd>

   <dt><em>--apiname API_NAME, -n API_NAME</em></dt>
   <dd>The name of the API. This flag is ignored when a configuration file is specified. The default name is the BASE_PATH. This flag is optional.</dd>

   <dt>--config-file <em>FILE</em>, -c <em>FILE</em></dt>
   <dd>A JSON file that contains the Swagger API configuration. When this flag is used, the API name flag is ignored. This flag is required.</dd>

   <dt>--response-type TYPE</dt>
   <dd>Set the web action response type as `html`, `http`, `json`, `text`, or `svg`. The default value is `json`. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Output:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />
### ibmcloud fn api delete
{: #cli_api_delete}

Delete an API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>The base path for the API.</dd>

   <dt>API_NAME</dt>
   <dd>The name of the API. The API name might be the same as the base path.</dd>

   <dt>API_PATH</dt>
   <dd>The path to the API</dd>

   <dt>API_VERB</dt>
   <dd>The verb for the API, such as `get` or `post`.</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>Specify the API output type as `json` or `yaml`. The default value is `json`.</dd>

   <dt>--full, -f</dt>
   <dd>Display the full API configuration details.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />
### ibmcloud fn api get
{: #cli_api_get}

Get the metadata for an API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>The base path for the API.</dd>

   <dt>API_NAME</dt>
   <dd>The name of the API. The API name might be the same as the base path.</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>Specify the API output type as `json` or `yaml`. The default value is `json`.</dd>

   <dt>--full, -f</dt>
   <dd>Display the full API configuration details.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />
### ibmcloud fn api list
{: #cli_api_list}

List all of the APIs that you created or a specific number of APIs. If no name or base path is specified, all of the APIs are listed.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>The base path for the API.</dd>

   <dt>API_NAME</dt>
   <dd>The name of the API. The API name might be the same as the base path.</dd>

   <dt>API_PATH</dt>
   <dd>The path to the API</dd>

   <dt>API_VERB</dt>
   <dd>The verb for the API, such as `get` or `post`.</dd>

   <dt>--full, -f</dt>
   <dd>Display the full API details. This flag is optional. </dd>

   <dt>--limit NUMBER_OF_APIS, -l NUMBER_OF_APIS</dt>
   <dd>List a specified number of APIs. The default is 30 APIs. This flag is optional. </dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned APIs by name, otherwise the list is sorted by creation date. This flag is optional. </dd>

   <dt>--skip NUMBER_OF_APIS, -s NUMBER_OF_APIS</dt>
   <dd>Exclude a specified number of the most recently created APIs from the result. This flag is optional. </dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn api list
  ```
  {: pre}




<br /><br />
## Deployment commands
{: #cli_deploy_cmds}


### ibmcloud fn deploy
{: #cli_deploy}

Use a manifest file to deploy a collection of packages, actions, triggers and rules.

```
ibmcloud fn deploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>--apihost HOST</dt>
   <dd>The <code>wsk</code> API host. This flag is optional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>The configuration file. The default is <code>$HOME/.wskprops</code>.</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>The path to the deployment file.</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>The path to the manifest file. This flag is required if the manifest.yaml is not in the current directory.</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>The name or ID for a namespace.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

   <dt>--preview </dt>
   <dd>Display the deployment plan before deploying.</dd>

   <dt>--project PATH</dt>
   <dd>The path to the serverless project. The default is <code>.</code> (current directory).</dd>

   <dt>--strict</dt>
   <dd>Allow a user-defined runtime version.</dd>

   <dt>--verbose, -v</dt>
   <dd>View verbose output.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />
### ibmcloud fn undeploy
{: #cli_undeploy}

Use a manifest file to undeploy a collection of packages, actions, triggers and rules.

```
ibmcloud fn undeploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE<em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt>--apihost HOST</dt>
   <dd>The <code>wsk</code> API host. This flag is optional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>The configuration file. The default is <code>$HOME/.wskprops</code>.</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>The path to the deployment file.</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>The path to the manifest file. This flag is required if the manifest.yaml is not in the current directory.</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>The name or ID for a namespace.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

   <dt>--preview </dt>
   <dd>Display the result of the command without running the command.</dd>

   <dt>--project PATH</dt>
   <dd>The path to the serverless project. The default is <code>.</code> (current directory).</dd>

   <dt>--strict</dt>
   <dd>Allow a user-defined runtime version.</dd>

   <dt>--verbose, -v</dt>
   <dd>View verbose output.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## List command
{: #cli_list_cmd}


### ibmcloud fn list
{: #cli_list}

View a grouped list of the packages, actions, triggers, and rules in the namespace.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt>--name-sort, -n</dt>
   <dd>Sort each group of returned entities by name, otherwise each group is sorted by creation date.</dd>
   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Namespace commands
{: #cli_namespace}


### ibmcloud fn namespace create
{: #cli_namespace_create}

Create an IAM namespace.

```
ibmcloud fn namespace create <em>NAMESPACE</em> [--description DESCRIPTION] 
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>The name for a namespace. Do not include hyphens (-) in the name. This value is required.</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>Write your own unique description to help you identify the namespace. If your description is more than one word, include quotation marks (") around your description. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />
### ibmcloud fn namespace delete
{: #cli_namespace_delete}

Delete an IAM namespace.

```
ibmcloud fn namespace delete <em>NAMESPACE</em>
```
{: pre}


<br /><strong>Example</strong>:

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />
### ibmcloud fn namespace get
{: #cli_namespace_get}

Get the entities for or the metadata information from a Cloud Foundry or IAM namespace.

```
ibmcloud fn namespace list <em>NAMESPACE</em> [--auth <em>KEY</em>] [--name-sort] [--properties] 
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>The name or ID for a namespace. This value is required.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned namespaces by name, otherwise the list is sorted by creation date. This flag is optional. </dd>

   <dt>--properties</dt>
   <dd>Display the namespace properties instead of the entities contained within it. This flag is optional. </dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  Output:
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />
### ibmcloud fn namespace list
{: #cli_namespace_list}

List the available Cloud Foundry and IAM namespaces.

```
ibmcloud fn namespace list [--auth <em>KEY</em>] [--cf] [--iam] [--limit NUMBER_OF_<em>NAMESPACE</em>S] [--name-sort] [--skip NUMBER_OF_<em>NAMESPACE</em>S] 
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--cf</dt>
   <dd>Display the Cloud Foundry namespaces only. IAM namespaces are not displayed. This flag is optional.</dd>

   <dt>--iam</dt>
   <dd>Display the IAM namespaces only. Cloud Foundry namespaces are not displayed. This flag is optional.</dd>

   <dt>--limit NUMBER_OF_<em>NAMESPACE</em>S, -l NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>List a specified number of namespaces. The default is 30 namespaces. This flag is optional. </dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned namespaces by name, otherwise the list is sorted by creation date. This flag is optional. </dd>

   <dt>--skip NUMBER_OF_<em>NAMESPACE</em>S, -s NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>Exclude a specified number of the most recently created namespaces from the result. This flag is optional. </dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />
### ibmcloud fn namespace update
{: #cli_namespace_update}

Change the name or description of an IAM namespace.

```
ibmcloud fn namespace update <em>NAMESPACE</em> [NEW_<em>NAMESPACE</em>_NAME] [--description DESCRIPTION] 
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>The name for a namespace. Do not include hyphens (-) in the name. This value is required.</dd>

   <dt>NEW_<em>NAMESPACE</em>_NAME</dt>
   <dd>The new name for a namespace. Do not include hyphens (-) in the name. This value is optional.</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>Write your own unique description to help you identify the namespace. If your description is more than one word, include quotation marks (") around your description. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Package commands
{: #cli_pkg}


### ibmcloud fn package bind
{: #cli_pkg_bind}

Bind parameters to a package. All of the actions within the package inherit those parameters unless otherwise specified.

```
ibmcloud fn package bind <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>The name of the package. This value is required. </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>A JSON file that contains annotation in a <em>KEY</em> VALE format. This flag is optional.</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>
  </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />
### ibmcloud fn package create
{: #cli_pkg_create}

Create a package designed to contain one or more actions. To add an action in the package, include the package name with the action name when you create or update the action.

```
ibmcloud fn package create <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>The name of the package. This value is required. </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>A JSON file that contains annotation in a <em>KEY</em> VALE format. This flag is optional.</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>A JSON file that contains parameter <em>KEYS</em> and C. This flag is optional.</dd>

  <dt>--shared yes|no</dt>
  <dd>When specified without a value or with a value of yes, the package is shared with other users.</dd>
  </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Output:
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />
### ibmcloud fn package delete
{: #cli_pkg_delete}

You can clean up your namespace by deleting packages that you do not want to use any longer.

```
ibmcloud fn package delete <em>PACKAGE_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  Output:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn package get
{: #cli_pkg_get}

Get metadata that describes a specific package.

```
ibmcloud fn package get <em>PACKAGE_NAME</em> [--summary]
```
{: pre}

<br /><strong>Command options</strong>:

  <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>The name of an package. This value is required.</dd>

   <dt>--summary</dt>
   <dd>Get a summary of the package details. Parameters with the prefix "*" are bound. This flag is optional.</dd>
   </dl>

<br /><strong>Example</strong>:

```
ibmcloud fn package get hello
```
{: pre}


<br />
### ibmcloud fn package list
{: #cli_pkg_list}

List all of the packages that you created or a specific number of packages.

```
ibmcloud fn package list [<em>NAMESPACE</em>] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>NAMESPACE</em></dt>
   <dd>List the packages in a specific namespace. This value is optional. If not specified, all   packages are listed.</dd>

   <dt>--limit NUMBER_OF_PACKAGES, -l NUMBER_OF_PACKAGES</dt>
   <dd>List a specified number of packages. The default is 30 packages.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned packages by name, otherwise the list is sorted by creation date.</dd>

   <dt>--skip NUMBER_OF_PACKAGES, -s NUMBER_OF_PACKAGES</dt>
   <dd>Exclude a specified number of the most recently created packages from the result.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Run `ibmcloud fn package list /whisk.system` to view a list of pre-installed packages.
  {: tip}


<br />
### ibmcloud fn package refresh
{: #cli_pkg_refresh}

Refresh the package bindings for all of the packages within a specific namespace.

```
ibmcloud fn package refresh /<em>NAMESPACE</em>
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>/<em>NAMESPACE</em></dt>
   <dd>A namespace, beginning with /. This flag is required. Run <code>ibmcloud fn namespace list</code> to get a list of namespaces to choose from.</dd>
   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />
### ibmcloud fn package update
{: #cli_pkg_update}

Update a package designed to contain one or more actions. To add an action in the package, include the package name with the action name when you create or update the action.

```
ibmcloud fn package update <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt><em>PACKAGE_NAME</em></dt>
   <dd>The name of the package. This value is required. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>A JSON file that contains annotation in a <em>KEY</em> VALE format. This flag is optional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

   <dt>--shared yes|no</dt>
   <dd>When specified without a value or with a value of <code>yes</code>, the package is shared with other users.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Output:
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## Property commands
{: #cli_prop}

Set global properties for your CLI environment or view properties about the <code>wsk</code> CLI, which runs as part of the `ibmcloud fn` CLI.

### ibmcloud fn property get
{: #cli_prop_get}

View the metadata details for a property from the <code>wsk</code> CLI.

```
ibmcloud fn property get [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt>--all</dt>
   <dd>View all properties for the <code>wsk</code> CLI. This flag is optional.</dd>

   <dt>---apibuild</dt>
   <dd>The <code>wsk</code> API build information. This flag is optional.</dd>

   <dt>--apibuildno</dt>
   <dd>The <code>wsk</code> API build number. This flag is optional.</dd>

   <dt>--apihost <em>HOST</em></dt>
   <dd>The <code>wsk</code> API host. This flag is optional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>The <code>wsk</code> API version. This flag is optional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>The <code>wsk</code> client certificate. This flag is optional.</dd>

   <dt>--cliversion</dt>
   <dd>The <code>wsk</code> CLI version. This flag is optional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>The <code>wsk</code> client <em>KEY</em>. This flag is optional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>An IAM namespace. This flag cannot be set for Cloud Foundry namespaces. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn property get --cliversion
  ```
  {: pre}


<br />
### ibmcloud fn property set
{: #cli_prop_set}

Set a property. At least one flag is required.

```
ibmcloud fn property set [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd>The <code>wsk</code> API host. This flag is optional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>The <code>wsk</code> API version. This flag is optional.</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>The <code>wsk</code> client certificate. This flag is optional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>The <code>wsk</code> client <em>KEY</em>. This flag is optional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>An IAM namespace. This flag cannot be set for Cloud Foundry namespaces. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  Output:
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### ibmcloud fn property unset
{: #cli_prop_unset}

Unset a property for the <code>wsk</code> CLI. At least one flag is required.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd>The <code>wsk</code> API host. This flag is optional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>The <code>wsk</code> API version. This flag is optional.</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd>The <code>wsk</code> authorization <em>KEY</em>. This flag is optional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>The <code>wsk</code> client certificate. This flag is optional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>The <code>wsk</code> client <em>KEY</em>. This flag is optional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>An IAM namespace. This flag cannot be set for Cloud Foundry namespaces. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn property unset --key my<em>KEY</em>
  ```
  {: pre}



<br /><br />
## Rule commands
{: #cli_rule}


### ibmcloud fn rule create
{: #cli_rule_create}

Create a rule to associate a trigger with an action. Before you can create a rule, create a trigger and an action first.

```
ibmcloud fn rule create <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  Output:
  ```
  ok: created myrule
  ```
  {: screen}


<br />
### ibmcloud fn rule delete
{: #cli_rule_delete}

To clean up your namespace, remove rules you no longer need.

```
ibmcloud fn rule delete <em>RULE_NAME</em> [--disable]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>The name of an rule. This value is required.</dd>

  <dt>--disable</dt>
  <dd>Disable the rule before deleting it.</dd>
  </dl>


<br /><strong>Example</strong>:

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />
### ibmcloud fn rule disable
{: #cli_rule_disable}

Change the status of a rule to inactive and stop it from running an action when a trigger is fired.

```
ibmcloud fn rule disable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule enable
{: #cli_rule_enable}

Change the status of a rule from inactive to active. When active, an action runs when a trigger is fired.

```
ibmcloud fn rule enable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule get
{: #cli_rule_get}

Get metadata that describes a specific rule.

```
ibmcloud fn rule get <em>RULE_NAME</em> [--summary]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>The name of an rule. This value is required.</dd>

  <dt>--summary</dt>
  <dd>Get a summary of the rule details.</dd>
  </dl>

<br /><strong>Example</strong>:

```
ibmcloud fn rule get myrule
```
{: pre}


<br />
### ibmcloud fn rule list
{: #cli_rule_list}

List all of the rules that you created or a specific number of rules.

```
ibmcloud fn rule list <em>RULE_NAME</em> [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>The name of a rule. This value is optional. If not specified, all rules are listed.</dd>

   <dt>--limit NUMBER_OF_RULES, -l NUMBER_OF_RULES</dt>
   <dd>List a specified number of rules. The default is 30 rules.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned rules by name, otherwise the list is sorted by creation date.</dd>

   <dt>--skip NUMBER_OF_RULES, -s NUMBER_OF_RULES</dt>
   <dd>Exclude a specified number of the most recently created rules from the result.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />
### ibmcloud fn rule status
{: #cli_rule_status}

See whether a rule is active or inactive. Run the `ibmcloud fn rule disable` or `ibmcloud fn run enable` commands to change the status.

```
ibmcloud fn rule status <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />
### ibmcloud fn rule update
{: #cli_rule_update}

To change which triggers are associated with which rules, you can update a rule.

```
ibmcloud fn rule update <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## SDK command
{: #cli_sdk}


### ibmcloud fn <ph class="ignoreSpelling">sdk</ph> install
{: #cli_sdk_install}

Install an SDK.

```
ibmcloud fn sdk install <em>COMPONENT</em> [--limit <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>COMPONENT</em></dt>
   <dd>The SDK component, such as `docker`, `iOS`, and `bashauto`. This value is required.</dd>

   <dt>--stdout, --s</dt>
   <dd>Prints the bash command results to STDOUT. This flag is optional.</dd>


   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Service commands
{: #cli_service}


### ibmcloud fn service bind
{: #cli_service_bind}

Bind service credentials to an action or package.

```
ibmcloud fn service bind SERVICE PACKAGE_or_<em>ACTION_NAME</em> [--instance SERVICE_INSTANCE] [--keyname SERVICE_<em>KEY</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>SERVICE</dt>
   <dd>The name of the service.</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>The name of the package or action to bind the credentials to.</dd>

   <dt>--instance SERVICE_INSTANCE</dt>
   <dd>The service instance name.</dd>

   <dt>--keyname SERVICE_<em>KEY</em></dt>
   <dd>The name of the service <em>KEY</em> credentials to bind.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />
### ibmcloud fn service unbind
{: #cli_service_unbind}

Unbind service credentials from an action or package.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_<em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt>SERVICE</dt>
   <dd>The name of the service.</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>The name of the package or action to unbind the credentials from.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Trigger commands
{: #cli_trigger}


### ibmcloud fn trigger create
{: #cli_trigger_create}

Create a trigger.

```
ibmcloud fn trigger create <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--feed <em>ACTION_NAME</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>The name of the trigger. This value is required. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>A JSON file that contains annotation in a <em>KEY</em> VALE format. This flag is optional.</dd>

   <dt>--feed <em>ACTION_NAME</em>, -f <em>ACTION_NAME</em></dt>
   <dd>Sets the type of trigger as a feed. This flag is optional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>


   </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />
### ibmcloud fn trigger delete
{: #cli_trigger_delete}

Delete a trigger.

```
ibmcloud fn trigger delete <em>TRIGGER_NAME</em>
```
{: pre}

<br /><strong>Example</strong>:

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />
### ibmcloud fn trigger fire
{: #cli_trigger_fire}

Test a trigger by firing it, rather than waiting for it to be triggered automatically.

```
ibmcloud fn trigger fire <em>TRIGGER_NAME</em> [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>

   <dt><em>TRIGGER_NAME</em></dt>
   <dd>The name of the trigger. This value is required. </dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter <em>VALUES</em> in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />
### ibmcloud fn trigger get
{: #cli_trigger_get}

Get metadata that describes a specific trigger.

```
ibmcloud fn trigger get <em>TRIGGER_NAME</em> [--summary]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>The name of an trigger. This value is required.</dd>

  <dt>--summary</dt>
  <dd>Get a summary of the trigger details.</dd>
  </dl>

<br /><strong>Example</strong>:

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### ibmcloud fn trigger list
{: #cli_trigger_list}

List all of the triggers that you created or a specific number of triggers.

```
ibmcloud fn trigger list <em>TRIGGER_NAME</em> [--limit <em>NUMBER_OF_TRIGGERS</em>] [--name-sort] [--skip <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>The name of a trigger. This value is optional. If not specified, all triggers are listed.</dd>

   <dt>--limit <em>NUMBER_OF_TRIGGERS</em>, -l <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>List a specified number of triggers. The default is 30 triggers.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned triggers by name, otherwise the list is sorted by creation date.</dd>

   <dt>--skip <em>NUMBER_OF_TRIGGERS</em>, -s <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>Exclude a specified number of the most recently created triggers from the result.</dd>

   </dl>

<br /><strong>Example</strong>:

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### ibmcloud fn trigger update
{: #cli_trigger_update}

Update a trigger.

```
ibmcloud fn trigger update <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Command options</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>The name of the trigger. This value is required. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Annotations are specified in a <em>KEY</em> <em>VALUE</em> format. To include more than one annotation, specify this option for each annotation. This flag is optional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>A JSON file that contains annotation in a <em>KEY</em> VALE format. This flag is optional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parameter values in the <em>KEY</em> <em>VALUE</em> format. This flag is optional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>A JSON file that contains parameter <em>KEYS</em> and <em>VALUES</em>. This flag is optional.</dd>
   </dl>

<br /><strong>Example</strong>:
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}




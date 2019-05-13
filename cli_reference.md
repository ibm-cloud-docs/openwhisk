---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-13"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

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

# CLI reference
{: #cli_reference}

Run these commands to manage the entities that make up your functions.
{: shortdec}


## ibmcloud fn action create
{: #cli_action_create}

Create an action.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--concurrency ACTIVATION_LIMIT] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web] [--web-secure SECRET]
```
{: pre}

<strong>Command options</strong>:

   <dl>
   <dt>--annotation ANNOTATION_KEY ANNOTATION_VALUE, -a ANNOTATION_KEY ANNOTATION_VALUE</dt>
   <dd>Annotations are specified in a KEY VALUE format. To include more than one annotation, specify this option for each annotation.</dd>

   <dt>--annotation-file FILE, -A FILE</dt>
   <dd>A JSON file that contains annotation in a KEY VALE format.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of the action. To include the action in a package, enter the name in the format PACKAGE_NAME/ACTION_NAME. This value is required. </dd>

   <dt><em>APP_FILE</em></dt>
   <dd>The path to the app file or package to run as an action. This option is required.</dd>

   <dt>--concurrency ACTIVATION_LIMIT, -c ACTIVATION_LIMIT</dt>
   <dd>The maximum intra-container concurrent activation limit for the action. The default value is one activation.</dd>

   <dt>--copy</dt>
   <dd>Treat the action as the name of an existing action.</dd>

   <dt>--docker DOCKER_HUB_USERNAME/IMAGE_NAME</dt>
   <dd>The Docker Hub user name and the name of the Docker image in Docker Hub to run the action. This value is required for creating actions from Docker images.</dd>

   <dt>--kind LANGUAGE</dt>
   <dd>The runtime for your app. This value is optional. If no value is specified, the default version for the detected runtime is used.
     Default runtime versions:
     <ul>
  <li>PHP 7.3</li>
  <li>Python: 2</li>
</ul>

     Other possible values for this option:
     <ul>
  <li><code>ballerina:0.990</code></li>
  <li><code>go:1.11</code></li>
  <li><code>nodejs:8</code></li>
  <li><code>nodejs:10</code></li>
  <li><code>php:7.3</code></li>
  <li><code>python:3</code></li>
  <li><code>python:3.6</code></li>
  <li><code>python:3.7</code></li>
  <li><code>ruby:2.5</code></li>
  <li><code>swift:4.2</code></li>
</ul>

       </dd>

   <dt>--logsize LIMIT, -l LIMIT</dt>
   <dd>The maximum log size in MB for the action. The default value is 10 MB.</dd>

   <dt>--main ENTRY_METHOD_NAME</dt>
   <dd>If the action's entry method is not `main`, specify the custom name. This value is required when the entry method is not `main`. For some runtimes, such as Java, the name must be the fully-qualified method.</dd>

   <dt>--native</dt>
   <dd>You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. This argument allows you to create and deploy an executable that runs inside the standard Docker action SDK.
       1. When you create a Docker image, a binary executable is created inside the container located at `/action/exec`. Copy the `/action/exec` file to your local file system and zip it into `exec.zip`.
       2. Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.

   <dt>--param KEY VALUE, -p KEY VALUE</dt>
   <dd>Parameter values in the KEY VALUE format.</dd>

   <dt>--param-file FILE, -P FILE</dt>
   <dd>A JSON file that contains parameter keys and values.</dd>

   <dt>--sequence ACTION_NAME, ACTION_NAME</dt>
   <dd>Create an action sequence and include the names of related actions separated by commas.</dd>

   <dt>--timeout LIMIT, -t LIMIT</dt>
   <dd>The timeout limit in milliseconds. The default value is 60000 milliseconds. When the timeout is reached, the action is terminated.</dd>

   <dt>--web string <em>yes</em>|<em>raw</em>|<em>no</em></dt>
   <dd>Treat the action as a web action, a raw HTTP web action, or as a standard action. Specify <em>yes</em> or no value for a web action, <em>raw</em> for a raw HTTP web action, or <em>no</em> for a standard action. To secure your web action, also include the `--web-secure` option.</dd>

   <dt>--web-secure SECRET</dt>
   <dd>Secure the web action. The value for SECRET can be <em>true</em>, <em>false</em>, or any string. This option can be used only with the `--web` option.</dd>
   </dl>

<strong>Example</strong>:

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  Output:
  ```
  ok: created hello
  ```
  {: screen}



## ibmcloud fn action delete
{: #cli_action_delete}

You can clean up your namespace by deleting actions that you do not want to use any longer.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}



<strong>Example</strong>:

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  Output:
  ```
  ok: deleted hello
  ```
  {: screen}





## ibmcloud fn action get
{: #cli_action_get}

Get metadata that describes a specific action.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<strong>Command options</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of an action. This value is required.</dd>

   <dt>--save</dt>
   <dd>You can get and locally save code associated with an existing action, except for sequences and Docker actions. The filename corresponds with an existing action name in the current working directory and the file extension  corresponds to the action kind. For example, for action code that is a zip file, an extension of .zip is used. This value is optional.</dd>

  <dt>--save-as FILENAME</dt>
  <dd>Save the code for actions in a custom-named file by providing a file path, filename, and extension. This value is optional.</dd>

  <dt>--summary</dt>
  <dd>Get a summary of the action details. Parameters with the prefix "*" are bound; parameters with the prefix "**" are bound and finalized. This value is optional.</dd>

  <dt>--url</dt>
  <dd>Get the URL only for the action. This value is optional.</dd>
   </dl>

<strong>Example</strong>:

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



## ibmcloud fn action invoke
{: #cli_action_invoke}

Run an action to test it.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<strong>Command options</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of the action. This value is required. </dd>

   <dt>--blocking, -b</dt>
   <dd>Blocking invocations use a request and response style to wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time limit value](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits). This value is optional.</dd>

   <dt>--param KEY VALUE, -p KEY VALUE</dt>
   <dd>Parameter values in the KEY VALUE format.</dd>

   <dt>--param-file FILE, -P FILE</dt>
   <dd>A JSON file that contains parameter keys and values.</dd>

   <dt>--result, -r</dt>
   <dd>The result of the app code is displayed as the output of the command. If this option is not specified, the activation ID is displayed. The invocation is blocking when this option is specified. This value is optional.</dd>

   </dl>

<strong>Example</strong>:
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


## ibmcloud fn action list
{: #cli_action_list}

List all of the actions that you created or a specific number of actions.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<strong>Command options</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>The name of a package of actions. This value is optional. If not specified, all actions are listed.</dd>

   <dt>--limit NUMBER_OF_ACTIONS, -l NUMBER_OF_ACTIONS</dt>
   <dd>List a specified number of actions. The default is 30 actions.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned actions by name, otherwise the list is sorted by creation date.</dd>

   <dt>--skip NUMBER_OF_ACTIONS, -s NUMBER_OF_ACTIONS</dt>
   <dd>Exclude a specified number of the most recently created actions from the result.</dd>

   </dl>

<strong>Example</strong>:

  ```
  ibmcloud fn action list
  ```
  {: pre}


## ibmcloud fn action update
{: #cli_action_update}

Update an action or the app within an action.

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--concurrency ACTIVATION_LIMIT] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web] [--web-secure SECRET]
```
{: pre}

<strong>Command options</strong>:

  <dl>
  <dt>--annotation ANNOTATION_KEY ANNOTATION_VALUE, -a ANNOTATION_KEY ANNOTATION_VALUE</dt>
  <dd>Annotations are specified in a KEY VALUE format. To include more than one annotation, specify this option for each annotation.</dd>

  <dt>--annotation-file FILE, -A FILE</dt>
  <dd>A JSON file that contains annotation in a KEY VALE format.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>The name of the action. To include the action in a package, enter the name in the format PACKAGE_NAME/ACTION_NAME. This value is required. </dd>

  <dt><em>APP_FILE</em></dt>
  <dd>The path to the app file or package to run as an action. This option is required when you want to update your app within the action.</dd>

  <dt>--concurrency ACTIVATION_LIMIT, -c ACTIVATION_LIMIT</dt>
  <dd>The maximum intra-container concurrent activation limit for the action. The default value is one activation.</dd>

  <dt>--copy</dt>
  <dd>Treat the action as the name of an existing action.</dd>

  <dt>--docker DOCKER_HUB_USERNAME/IMAGE_NAME</dt>
  <dd>The Docker Hub user name and the name of the Docker image in Docker Hub to run the action. This value is required for creating actions from Docker images.</dd>

  <dt>--kind LANGUAGE</dt>
  <dd>The runtime for your app. This value is optional. If no value is specified, the default version for the detected runtime is used.
    Default runtime versions:
    <ul>
  <li>PHP 7.3</li>
  <li>Python: 2</li>
</ul>

    Other possible values for this option:
    <ul>
  <li><code>ballerina:0.990</code></li>
  <li><code>go:1.11</code></li>
  <li><code>nodejs:8</code></li>
  <li><code>nodejs:10</code></li>
  <li><code>php:7.3</code></li>
  <li><code>python:3</code></li>
  <li><code>python:3.6</code></li>
  <li><code>python:3.7</code></li>
  <li><code>ruby:2.5</code></li>
  <li><code>swift:4.2</code></li>
</ul>

      </dd>

  <dt>--logsize LIMIT, -l LIMIT</dt>
  <dd>The maximum log size in MB for the action. The default value is 10 MB.</dd>

  <dt>--main ENTRY_METHOD_NAME</dt>
  <dd>If the action's entry method is not `main`, specify the custom name. This value is required when the entry method is not `main`. For some runtimes, such as Java, the name must be the fully-qualified method.</dd>

  <dt>--native</dt>
  <dd>You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. This argument allows you to create and deploy an executable that runs inside the standard Docker action SDK.
      1. When you create a Docker image, a binary executable is created inside the container located at `/action/exec`. Copy the `/action/exec` file to your local file system and zip it into `exec.zip`.
      2. Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.

  <dt>--param KEY VALUE, -p KEY VALUE</dt>
  <dd>Parameter values in the KEY VALUE format.</dd>

  <dt>--param-file FILE, -P FILE</dt>
  <dd>A JSON file that contains parameter keys and values.</dd>

  <dt>--sequence ACTION_NAME, ACTION_NAME</dt>
  <dd>Create an action sequence by specifying the name of related actions.</dd>

  <dt>--timeout LIMIT, -t LIMIT</dt>
  <dd>The timeout limit in milliseconds. The default value is 60000 milliseconds. When the timeout is reached, the action is terminated.</dd>

  <dt>--web <em>yes</em>|<em>raw</em>|<em>no</em></dt>
  <dd>Treat the action as a web action, a raw HTTP web action, or as a standard action. Specify <em>yes</em> or no value for a web action, <em>raw</em> for a raw HTTP web action, or <em>no</em> for a standard action. To secure your web action, also include the `--web-secure` option.</dd>

  <dt>--web-secure SECRET</dt>
  <dd>Secure the web action. The value for SECRET can be <em>true</em>, <em>false</em>, or any string. This option can be used only with the `--web` option.</dd>
  </dl>

<strong>Example</strong>:
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}



## ibmcloud fn activation get
{: #cli_activation_get}

Get metadata that describes a specific activation.

```
ibmcloud fn activation get [<em>ACTIVATION_ID</em>] [<em>FIELD_FILTER</em>] [--last] [--summary]
```
{: pre}


<strong>Command options</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified..</dd>

  <dt><em>FIELD_FILTER</em></dt>
  <dd>A field in the metadata to display information from. For example, to display the logs field, run `ibmcloud fn activation get ACTIVATION_ID logs`. This value is optional.</dd>

  <dt>--last, -l</dt>
  <dd>Display the metadata for the most recent activation. This value is optional.</dd>

  <dt>--summary, -s</dt>
  <dd>Display the result response only from the activation details. This value is optional.</dd>
  </dl>


<strong>Example</strong>:
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


## ibmcloud fn activation list
{: #cli_activation_list}

List all of the activation IDs for all of the actions in a package.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since DAY_OF_THE_WEEK, DAY, MONTH, YEAR] [--skip NUMBER_OF_ACTIVATIONS] [--upto DAY_OF_THE_WEEK, DAY, MONTH, YEAR]
```
{: pre}


<strong>Command options</strong>:

  <dl>
  <dt>--full, -f</dt>
  <dd>Display the full activation description</dd>

  <dt>--limit NUMBER_OF_ACTIVATIONS, -l NUMBER_OF_ACTIVATIONS</dt>
  <dd>List a specified number of activations. The default is 30 activations and the maximum is 200 activations.</dd>

  <dt>--since DAY_OF_THE_WEEK, DAY, MONTH, YEAR</dt>
  <dd>List activations that were created since the date specified. Measured in milliseconds. Example: Th, 01, Jan 1970</dd>

  <dt>--skip NUMBER_OF_ACTIVATIONS, -s NUMBER_OF_ACTIVATIONS</dt>
  <dd>Exclude a specified number of the most recent activations from the result.</dd>

  <dt>--upto DAY_OF_THE_WEEK, DAY, MONTH, YEAR</dt>
  <dd>List activations that were created before the date specified. Measured in milliseconds. Example: Th, 01, Jan 1970</dd>
  </dl>



<strong>Example</strong>:
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



## ibmcloud fn activation logs
{: #cli_activation_logs}

Get logs for a specific activation.

```
ibmcloud fn activation logs [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<strong>Command options</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified.</dd>

  <dt>--last, -l</dt>
  <dd>Display the logs for the most recent activation. This value is optional.</dd>

  <dt>--strip, -r</dt>
  <dd>Display the log message only; exclude the timestamp and stream information. This value is optional.</dd>
  </dl>


<strong>Example</strong>:
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}



## ibmcloud fn activation result
{: #cli_activation_result}

Get the result from a specific activation.

```
ibmcloud fn activation result [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<strong>Command options</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>The ID for a specific activation. Use `ibmcloud fn activation list` to retrieve a list of available IDs. This value is required, unless the `--last` or `-l` option is specified.</dd>

  <dt>--last, -l</dt>
  <dd>Display the result for the most recent activation. This value is optional.</dd>

  </dl>


<strong>Example</strong>:
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}


## ibmcloud fn activation poll
{: #cli_activation_poll}

View a streaming, live list of activations for an action or a namespace. You can press CTRL+C to exit the polling.

```
ibmcloud fn activation poll [/<em>NAMESPACE</em>] [<em>ACTION_NAME</em>] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}


<strong>Command options</strong>:

  <dl>
  <dt>/<em>NAMESPACE</em></dt>
  <dd>A namespace, beginning with /. Poll activations for a namespace, an action, or a space. This value is optional. If a namespace or action is not specified, the space is polled.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>Poll activations for a namespace, an action, or a space. This value is optional. If a namespace or action is not specified, the space is polled.</dd>

  <dt>--exit SECONDS, -e SECONDS</dt>
  <dd>Poll activations for a specified number of seconds and then exit. This value is optional.</dd>

  <dt>--since-days DAYS</dt>
  <dd>Start polling for activations a specified number of days ago. This value is optional.</dd>

  <dt>--since-hours HOURS</dt>
  <dd>Start polling for activations a specified number of hours ago. This value is optional.</dd>

  <dt>--since-minutes MINUTES</dt>
  <dd>Start polling for activations a specified number of minutes ago. This value is optional.</dd>

  <dt>--since-seconds SECONDS</dt>
  <dd>Start polling for activations a specified number of seconds ago. This value is optional.</dd>
  </dl>


<strong>Example</strong>:
```
ibmcloud fn activation poll
```
{: pre}



## ibmcloud fn package bind
{: #cli_pkg_bind}

Bind parameters to a package. All of the actions within the package inherit those parameters unless otherwise specified.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}


<strong>Command options</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>The name of the package. This value is required. </dd>

  <dt>--annotation ANNOTATION_KEY ANNOTATION_VALUE, -a ANNOTATION_KEY ANNOTATION_VALUE</dt>
  <dd>Annotations are specified in a KEY VALUE format. To include more than one annotation, specify this option for each annotation.</dd>

  <dt>--annotation-file FILE, -A FILE</dt>
  <dd>A JSON file that contains annotation in a KEY VALE format.</dd>

  <dt>--param KEY VALUE, -p KEY VALUE</dt>
  <dd>Parameter values in the KEY VALUE format.</dd>

  <dt>--param-file FILE, -P FILE</dt>
  <dd>A JSON file that contains parameter keys and values.</dd>
  </dl>

<strong>Example</strong>:

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}




## ibmcloud fn package create
{: #cli_pkg_create}

Create a package designed to contain one or more actions. To add an action in the package, include the package name with the action name when you create or update the action.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<strong>Command options</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>The name of the package. This value is required. </dd>

  <dt>--annotation ANNOTATION_KEY ANNOTATION_VALUE, -a ANNOTATION_KEY ANNOTATION_VALUE</dt>
  <dd>Annotations are specified in a KEY VALUE format. To include more than one annotation, specify this option for each annotation.</dd>

  <dt>--annotation-file FILE, -A FILE</dt>
  <dd>A JSON file that contains annotation in a KEY VALE format.</dd>

  <dt>--param KEY VALUE, -p KEY VALUE</dt>
  <dd>Parameter values in the KEY VALUE format.</dd>

  <dt>--param-file FILE, -P FILE</dt>
  <dd>A JSON file that contains parameter keys and values.</dd>

  <dt>--shared yes|no</dt>
  <dd>When specified without a value or with a value of yes, the package is shared with other users.</dd>
  </dl>

<strong>Example</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Output:
  ```
  ok: created hellopkg
  ```
  {: screen}



## ibmcloud fn package delete
{: #cli_pkg_delete}

You can clean up your namespace by deleting packages that you do not want to use any longer.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}



<strong>Example</strong>:

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  Output:
  ```
  ok: deleted hello
  ```
  {: screen}





## ibmcloud fn package get
{: #cli_pkg_get}

Get metadata that describes a specific package.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<strong>Command options</strong>:

  <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>The name of an package. This value is required.</dd>

   <dt>--summary</dt>
   <dd>Get a summary of the package details. Parameters with the prefix "*" are bound. This value is optional.</dd>
   </dl>

<strong>Example</strong>:

```
ibmcloud fn package get hello
```
{: pre}





## ibmcloud fn package list
{: #cli_pkg_list}

List all of the packages that you created or a specific number of packages.

```
ibmcloud fn package list PACKAGE_NAME [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<strong>Command options</strong>:

   <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>The name of a package of actions. This value is optional. If not specified, all packages are listed.</dd>

   <dt>--limit NUMBER_OF_PACKAGES, -l NUMBER_OF_PACKAGES</dt>
   <dd>List a specified number of packages. The default is 30 packages.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Sort the list of returned packages by name, otherwise the list is sorted by creation date.</dd>

   <dt>--skip NUMBER_OF_PACKAGES, -s NUMBER_OF_PACKAGES</dt>
   <dd>Exclude a specified number of the most recently created packages from the result.</dd>

   </dl>

<strong>Example</strong>:

  ```
  ibmcloud fn package list
  ```
  {: pre}


## ibmcloud fn package update
{: #cli_pkg_update}

Update a package designed to contain one or more actions. To add an action in the package, include the package name with the action name when you create or update the action.

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<strong>Command options</strong>:

   <dl>

   <dt><em>PACKAGE_NAME</em></dt>
   <dd>The name of the package. This value is required. </dd>

   <dt>--annotation ANNOTATION_KEY ANNOTATION_VALUE, -a ANNOTATION_KEY ANNOTATION_VALUE</dt>
   <dd>Annotations are specified in a KEY VALUE format. To include more than one annotation, specify this option for each annotation.</dd>

   <dt>--annotation-file FILE, -A FILE</dt>
   <dd>A JSON file that contains annotation in a KEY VALE format.</dd>

   <dt>--param KEY VALUE, -p KEY VALUE</dt>
   <dd>Parameter values in the KEY VALUE format.</dd>

   <dt>--param-file FILE, -P FILE</dt>
   <dd>A JSON file that contains parameter keys and values.</dd>

   <dt>--shared yes|no</dt>
   <dd>When specified without a value or with a value of <code>yes</code>, the package is shared with other users.</dd>

   </dl>

<strong>Example</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Output:
  ```
  ok: created hellopkg
  ```
  {: screen}


## ibmcloud fn package refresh
{: #cli_pkg_refresh}

Refresh the package bindings for all of the packages within a specific namespace.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}


<strong>Command options</strong>:

   <dl>

   <dt>/<em>NAMESPACE</em></dt>
   <dd>A namespace, beginning with /. This value is required. Run <code>ibmcloud fn namespace list</code> to get a list of namespaces to choose from.</dd>

<strong>Example</strong>:

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}



</staging>

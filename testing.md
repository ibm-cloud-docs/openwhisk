<staging>---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-30"

keywords: actions, serverless, javascript, node, node.js

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


# Invoking serverless apps
{: #actions_invoking}

Run your action.
{: shortdesc}


## Invoking actions from the CLI
{: #invoke-js}

You can invoke actions for most languages by running the `invoke` command. 

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

Hello world example:
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

Output:
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### Invoking blocking actions 
{: #invoke-block}

The invocation of the action can be blocking or non-blocking. Invocations are non-blocking by default. If you don't need the action result right away, use a non-blocking invocation.

Blocking invocations use a request/response style and wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time limit value](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits).

Run the action in the cloud by running a blocking invocation: 

```
ibmcloud fn action invoke --blocking hello
```
{: pre}

The command outputs the following information:
    * Without the --result option, the activation ID is displayed in the result. The activation ID (`44794bd6aab74415b4e42a308d880e5b`) which can be used to retrieve the logs or the result of the invocation. 
    * The invocation result if it is available within the expected wait period

```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
    "status": "success",
    "success": true
}
```
{: screen}

For tips on monitoring activations, see [Monitoring action output](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output).
{: tip}








### Invoking asynchronous JavaScript actions from the CLI
{: #asynchronous_javascript}

JavaScript functions that run asynchronously can return the activation result after the `main` function returns by returning a promise in your action.


1. Invoke the action.

    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    Example output:

    ```
    {
        "done": true
    }
    ```
    {: screen}

2. Check how long the activation took to complete by getting the activation log.

  1. Get the activation ID.

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Example output:
      ```
      activations
      b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Get the log for the activation ID.

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      The `duration` shows that this activation took slightly over 2 seconds to complete:

      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}




### Invoking Docker actions
{: #actions-docker}

With {{site.data.keyword.openwhisk_short}} Docker actions, you can write your actions in any language.
{: shortdesc}

1. Invoke the action.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Example output:
    ```
    {
        "args": {
            "payload": "Rey"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```
    {: screen}


If there is a warm container that uses a previous version of your Docker image, any new invocations continue to use that image. Update the action so that new invocations begin using the new image.
```
ibmcloud fn action update example --docker <username>/blackboxdemo
```
{: pre}


### Invoking actions in a package from the CLI
{: #openwhisk_package_invoke}

You can invoke actions in a package, as with other actions. The next few steps show how to invoke the `greeting` action in the `/whisk.system/samples` package with different parameters.

1. Get a description of the `/whisk.system/samples/greeting` action.
  ```
  ibmcloud fn action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  Example output:
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  Notice that the `greeting` action takes two parameters: `name` and `place`.

2. Invoke the action without any parameters.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  Example output:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  The output is a generic message because no parameters were specified.

3. Invoke the action with parameters.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  Example output:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Notice that the output uses the `name` and `place` parameters that were passed to the action.
  
  
### Passing parameters to an action during invocation from the CLI
{: #pass-params-action}

Parameters can be passed to an action when it is invoked.

1. Save the following code to a file named `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

2. Create the `hello` action.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. If you previously used this action, ensure the action is cleared of any previously set parameters by updating it.
    ```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. Invoke the action by passing `name` and `place` parameters.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **Note**: You can instead pass a file of JSON-formatted parameters:
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    Example output:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
    }
    ```
    {: screen}

5. You can also pass parameters in a structured objects to your action. For example, update the `hello` action to the following:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
    }
    ```
    {: codeblock}

    Now, the action expects a single `person` parameter to have fields `name` and `place`.

6. Invoke the action with a single `person` parameter that is a valid JSON object.
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    Example output:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
    }
    ```
    {: screen}




### Binding parameters to an action to invoke from the CLI
{: #default-params-action}

Actions can be invoked with multiple named parameters. For example, the basic `hello` action expects two parameters: the `name` of a person and the `place` they are from.

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
}
```
{: screen}

Rather than pass all the parameters to an action every time, you can bind default parameters to the action. The following steps show you how to bind the `place` parameter to the basic `hello` action so that the action defaults to the place "Kansas".

1. Save the following code to a file named `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

2. Create the `hello` action.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Update the action to bind parameter values by using the `--param` flag and a key/value pair.

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **Note**: You can instead pass a file of JSON-formatted parameters:
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    If you modify your non-service credential parameters, running an `action update` command with new parameters removes any parameters that currently exist but are not specified in the `action update` command. For example, if you run `action update -p key1 new-value -p key2 new-value` but omit any other parameters that were set, those parameters no longer exist after the action is updated. Any services that were bound to the action are also removed, so after you update other parameters you must [bind services to your action](/docs/openwhisk?topic=cloud-functions-binding_services) again.
    {: tip}

4. Invoke the action by passing only the `name` parameter.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy
    ```
    {: pre}

    Example output:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
    }
    ```
    {: screen}

    Because you did not specify the `place` parameter when you invoked the action, the value of the bound default parameter, `Kansas`, is used.

5. Bound parameters can be overwritten by specifying the parameter value at invocation time. Invoke the action by passing both the `name` and `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
    ```
    {: pre}

    Example output:
    ```
    {
        "payload": "Hello, Dorothy from Washington, DC"
    }
    ```
    {: screen}
    


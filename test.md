---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Testing serverless apps
{: #test}

Test each entity that you create from the CLI to verify that your serverless app is working or to troubleshoot where an issue might be occurring.
{: shortdesc}


## Testing actions
{: #test-js}

You can test actions by running the `invoke` command. You can test the action with or without parameters.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

**Hello world example**
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

**Output**
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### Testing parameters stored in JSON files
{: #test_json_file}

You can pass a file of JSON-formatted parameters.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

**Example output**
```
{
    "payload": "Hello, Dorothy from Kansas"
}
```
{: screen}


### Testing parameters entered in JSON format
{: #test_json}

You can pass JSON-formatted parameters with your invocation.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

**Example output**
```
{
    "payload": "Hello, Dorothy from Kansas"
}
```
{: screen}


### Testing blocking actions
{: #test-block}

The invocation of the action can be blocking or non-blocking. Invocations are non-blocking by default. If you don't need the action result right away, use a non-blocking invocation.
{: shortdesc}

Blocking invocations use a request-response style and wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time limit value](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits).

Run the action in the cloud by running a blocking invocation.

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


**Example output**
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

The command outputs the following information.
* The invocation result, if it is available within the expected wait period
* Without the `--result` option, the activation ID is displayed in the result. The activation ID (`44794bd6aab74415b4e42a308d880e5b`) which can be used to retrieve the logs or the result of the invocation.


## Testing triggers
{: #test_triggers}

Triggers can be fired, or activated, by using a dictionary of key-value pairs. Sometimes this dictionary is referred to as the event. Triggers can be explicitly fired by a user or fired on behalf of a user by an external event source. As with actions, each firing of a trigger that is associated with a rule results in an activation ID.
{: shortdesc}

1. Fire the trigger.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    A trigger that isn't associated with a rule has no visible effect when it is fired. Because no rule associated with this trigger, the passed parameters are not used as input by any action.

    **Example output**

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. Verify that the action was invoked by checking the most recent activation record.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    **Example output**
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. Get more information on the activation ID from the previous command output.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    **Example output**
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## Testing duration of activations
{: #test_time}

Check how long an activation took to complete by getting the activation log. If the duration is too long or you need to adjust the default timeout to allow the function to run longer, you can update your action with a timeout.
{: shortdesc}

1. Get the activation ID.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Example output:
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. Get the log for the activation ID.

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    The `duration` shows the time in milliseconds. The activation took slightly over 2 seconds to complete.

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

3. Update the action with a timeout in milliseconds.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    Example:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## Testing memory usage
{: #test_memory}

If your app is packaged in a Docker image, you can use Docker commands to check the memory usage of your app.
{: shortdesc}

1. Create a container locally that runs your Docker image.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. Get a list of the containers to get a container ID.

    ```
    docker ps
    ```
    {: pre}

3. Check the statistics of the running container.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. Review the memory usage value for the container. If the value does not fit within the system limits, adjust your script.

5. After you are done reviewing the information, you can stop the running container.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. Remove the container.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}







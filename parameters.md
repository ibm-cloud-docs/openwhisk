---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Working with parameters

In serverless actions, data is supplied by adding parameters to the actions. Parameters are declared as an argument to the main serverless function.
{: shortdesc}

You can supply values for parameters in two ways:
* **Pass parameters to an action during invocation**: Supply parameters when the action is invoked either through CLI flags or through a file. Parameters supplied at invocation override any default parameters that were previously set.
* **Bind parameters to an action or package**: Set default parameters when an action or package is created or updated. This option is useful for data that stays the same on every execution, equivalent to environment variables on other platforms, or for default values that might be overridden at invocation time.

## Passing parameters to an action during invocation
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

## Binding parameters to an action
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

## Binding parameters to a package
{: #default-params-package}

Default parameters can also be set at the package level. Bound parameters serve as the default parameters for actions in the package unless:

- The action itself has a default parameter
- The action has a parameter that is supplied at invocation time

The following example sets a default parameter of `name` on the `MyApp` package and shows an action using it.

1. Create a package, binding the default parameter `name` to it.
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. Save the following code to a file named `helloworld.js`.

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
       }
    ```
    {: codeblock}

3. Create the action in the `MyApp` package.
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    If you modify your non-service credential parameters, running an `action update` command with new parameters removes any parameters that currently exist but are not specified in the `action update` command. For example, if you run `action update -p key1 new-value -p key2 new-value` but omit any other parameters that were set, those parameters no longer exist after the action is updated. Any services that were bound to the action are also removed, so after you update other parameters you must [bind services to your action](/docs/openwhisk?topic=cloud-functions-binding_services) again.
    {: tip}

3. Invoke the action without passing any parameters.
    ```
    ibmcloud fn action invoke --result MyApp/hello
    ```
    {: pre}

    Example output:
    ```
       {
           "payload": "Hello, World"
       }
    ```
    {: screen}

    The default parameter that is bound to the package is used.

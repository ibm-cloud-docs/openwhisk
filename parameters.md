---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Working with parameters

Learn how to set parameters on packages and actions for deployment, and how to pass parameters to actions during invocation. You can also use a file to store parameters and pass the filename to the action, rather than supply each parameter individually on the command line.
{: shortdesc}

With serverless actions, data is supplied by adding parameters to the actions, which are declared as an argument to the main serverless function. All data arrives this way and the values can be set in a few different ways. The first option is to supply parameters when an action or package is created (or updated). This option is useful for data that stays the same on every execution, equivalent to environment variables on other platforms, or for default values that might be overridden at invocation time. The second option is to supply parameters when the action is invoked which overrides any parameters that were previously set.

## Passing parameters to an action during invocation
{: #pass-params-action}

Parameters can be passed to an action when it is invoked. The examples that are provided use JavaScript but all the other languages work in the same way. To see detailed examples, check out the following topics on [Javascript actions](./openwhisk_actions.html#creating-and-invoking-javascript-actions), [Swift actions](./openwhisk_actions.html#creating-swift-actions), [Python actions](./openwhisk_actions.html#creating-python-actions), [Java actions](./openwhisk_actions.html#creating-java-actions), [PHP actions](./openwhisk_actions.html#creating-php-actions), [Docker actions](./openwhisk_actions.html#creating-docker-actions) or [Go actions](./openwhisk_actions.html#creating-go-actions).

1. Use parameters in the action. For example, create a file that is named **hello.js** with the following content:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  The input parameters are passed as a JSON object parameter to the **main** function. Notice how the `name` and `place` parameters are retrieved from the `params` object in this example.

2. Update the **hello** action so it is ready to use:
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

  If you modify your non-service credential parameters, running an `action update` command with new parameters removes any parameters that currently exist but are not specified in the `action update` command. For example, if you run `action update -p key1 new-value -p key2 new-value` but omit any other parameters that were set, those parameters no longer exist after the action is updated. Any services that were bound to the action are also removed, so after you update other parameters you must [bind services to your action](./binding_services.html) again.
  {: tip}

3. Parameters can be provided explicitly by using the command line, or by [supplying a file](./parameters.html#using-parameter-files) that contains the desired parameters.

  To pass parameters directly through the command line, supply a key/value pair to the `--param` flag:
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **Response:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Notice the use of the `--result` option: it implies a blocking invocation where the CLI waits for the activation to complete and then displays only the result. For convenience, this option can be used without `--blocking` which is automatically inferred.

  Additionally, if parameter values that are specified on the command line are valid JSON, then they are parsed and sent to your action as a structured object.

  For example, update the **hello** action to the following:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Now the action expects a single `person` parameter to have fields `name` and `place`.

  Next, invoke the action with a single `person` parameter that is a valid JSON, like in the following example:
  ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **Response:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  The result is the same because the CLI automatically parses the `person` parameter value into the structured object that the action now expects.

## Setting default parameters on an action
{: #default-params-action}

actions can be invoked with multiple named parameters. Recall that the **hello** action from the previous example expects two parameters: the *name* of a person, and the *place* they are from.

Rather than pass all the parameters to an action every time, you can bind certain parameters. The following example binds the *place* parameter so that the action defaults to the place "Kansas":

1. Update the action by using the `--param` option to bind parameter values, or by passing a file that contains the parameters to `--param-file`. (For examples that use files, see the section on [using parameter files](./parameters.html#using-parameter-files).

  To specify default parameters explicitly on the command line, provide a key/value pair to the `param` flag:
  ```
  ibmcloud fn action update hello --param place Kansas
  ```
  {: pre}

2. Invoke the action by passing only the `name` parameter this time.
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

  Notice that you did not need to specify the place parameter when you invoked the action. Bound parameters can still be overwritten by specifying the parameter value at invocation time.

3. Invoke the action by passing both the `name` and `place` values, and observe the output:

  Invoke the action using the `--param` flag:
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

  Parameters set on an action when it was created or updated are always overridden by a parameter that is supplied directly on invocation.
  {: tip}

## Setting default parameters on a package
{: #default-params-package}

Parameters can be set at the package level, and serve as the default parameters for actions _unless_:

- The action itself has a default parameter.
- The action has a parameter that is supplied at invoke time, which is always the "priority" when more than one parameter is available.

The following example sets a default parameter of `name` on the **MyApp** package and shows an action using it.

1. Create a package with a parameter set:

  ```
  ibmcloud fn package update MyApp --param name World
  ```
  {: pre}

2. Create an action in the **MyApp** package:
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Create the action:
  ```
  ibmcloud fn action update MyApp/hello hello.js
  ```
  {: pre}

3. Invoke the action, and observe the default package parameter in use:
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

## Using parameter files
{: #using-parameter-files}

You can put parameters into a file in JSON format, and then pass in the parameters by supplying the filename with the `--param-file` flag. This method can be used for both package and action creation (or updates), and during action invocation.

1. As an example, consider the **hello** example from earlier by using `hello.js` with the following content:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Update the action with the updated contents of `hello.js`:

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. Create a parameter file called `parameters.json` containing JSON-formatted parameters:

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. Use the `parameters.json` filename when invoking the **hello** action, and observe the output:

  ```
  ibmcloud fn action invoke --result hello --param-file parameters.json
  ```

  Example output:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

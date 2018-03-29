---

copyright:
  years: 2018
lastupdated: "2018-03-29"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Working with parameters

When working with serverless Actions, data is supplied by adding parameters to the Actions; these are in the parameter declared as an argument to the main serverless function. All data arrives this way and the values can be set in a few different ways. The first option is to supply parameters when an Action or package is created (or updated). This approach is useful for data that stays the same on every execution, equivalent to environment variables on other platforms, or for default values that might be overridden at invocation time. The second option is to supply parameters when the Action is invoked - and this approach will override any parameters already set.

This page outlines how to configure parameters when deploying packages and Actions, and how to supply parameters when invoking an Action. There is also information on how to use a file to store the parameters and pass the filename, rather than supplying each parameter individually on the comandline.

## Passing parameters to an Action at invoke time
{: #pass-params-action}

Parameters can be passed to the Action when it is invoked. The examples provided use JavaScript but all the other languages work in the same way. To see detailed examples, check out the following topics on [Javascript actions](./openwhisk_actions.html##creating-and-invoking-javascript-actions), [Swift actions](./openwhisk_actions.html#creating-swift-actions), [Python actions](./openwhisk_actions.html#creating-python-actions), [Java actions](./openwhisk_actions.html#creating-java-actions), [PHP actions](./openwhisk_actions.html#creating-php-actions), [Docker actions](./openwhisk_actions.html#creating-docker-actions) or [Go actions](./openwhisk_actions.html#creating-go-actions) as appropriate.

1. Use parameters in the Action. For example, create a file named **hello.js** with the following content:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  The input parameters are passed as a JSON object parameter to the **main** function. Notice how the `name` and `place` parameters are retrieved from the `params` object in this example.

2. Update the **hello** Action so it is ready to use:
  ```
  bx wsk action update hello hello.js
  ```
  {: pre}

  If you need to modify your non-service credential parameters, be aware that doing an `action update` command with new parameters removes any parameters that currently exist, but are not specified in the `action update` command. For example, if there are two parameters aside from the `__bx_creds`, with keys named key1 and key2.  If you run an `action update` command with `-p key1 new-value -p key2 new-value` but omit the `__bx_creds` parameter, the `__bx_creds` parameter will no longer exist after the `action update` completes successfully. You then must re-bind the service credentials. This is a known limitation without a workaround.
  {: tip}  

3. Parameters can be provided explicitly using the command-line, or by supplying a file that contains the desired parameters.

  To pass parameters directly through the command-line, supply a key/value pair to the `--param` flag:
  ```
  bx wsk action invoke --result hello --param name Dorothy --param place Kansas
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

  Additionally, if parameter values that are specified on the command line are valid JSON, then they are parsed and sent to your action as a structured object. For example, update the hello action to the following:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Now the Action expects a single `person` parameter to have fields `name` and `place`.

  Next, invoke the Action with a single `person` parameter that is a valid JSON, like in the following example:
  ```
  bx wsk action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  The result is the same because the CLI automatically parses the `person` parameter value into the structured object that the Action now expects:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

## Setting default parameters on an Action
{: #default-params-action}

Actions can be invoked with multiple named parameters. Recall that the **hello** Action from the previous example expects two parameters: the *name* of a person, and the *place* they are from.

Rather than pass all the parameters to an Action every time, you can bind certain parameters. The following example binds the *place* parameter so that the Action defaults to the place "Kansas":

1. Update the Action by using the `--param` option to bind parameter values, or by passing a file that contains the parameters to `--param-file`. (For examples using files, see the section on [working with parameter files](#working-with-parameter-files).

  To specify default parameters explicitly on the command-line, provide a key/value pair to the `param` flag:
  ```
  bx wsk action update hello --param place Kansas
  ```
  {: pre}

2. Invoke the Action by passing only the `name` parameter this time.
  ```
  bx wsk action invoke --result hello --param name Dorothy
  ```
  {: pre}

  **Output:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Notice that you did not need to specify the place parameter when you invoked the Action. Bound parameters can still be overwritten by specifying the parameter value at invocation time.

3. Invoke the Action by passing both the `name` and `place` values, and observe the output:

  Invoke the Action using the `--param` flag:
  ```
  bx wsk action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  **Output:**
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  Despite setting a parameter on an Action when it was created or updated, it is always overridden by a parameter that is supplied when invoking the Action.
  {: tip}

## Setting default parameters on a Package
{: #default-params-package}

Parameters can be set at the package level, and serve as the default parameters for Actions _unless_:

- The Action itself has a default parameter.
- The Action has a parameter supplied at invoke time, which is always the "priority" when more than one parameter is available.

The following example sets a default parameter of `name` on the **MyApp** package and shows an Action using it.

1. Create a package with a parameter set:

  ```
  bx wsk package update MyApp --param name World
  ```
  {: pre}

2. Create an Action in the **MyApp** package:
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Create the Action:
  ```
  bx wsk action update MyApp/hello hello.js
  ```
  {: pre}

3. Invoke the Action, and observe the default package parameter in use:
  ```
  bx wsk action invoke --result MyApp/hello
  ```
  {: pre}

  **Output:**
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## Working with parameter files
{: #working-with-parameter-files}

You can put parameters into a file in JSON format, and then pass in the parameters by supplying the filename with the `--param-file` flag. This method can be used for both packages and Actions when creating or updating them, and when invoking Actions.

1. As an example, consider the **hello** example from earlier by using `hello.js` with the following content:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Update the Action with the updated contents of `hello.js`:

  ```
  bx wsk action update hello hello.js
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

4. Use the `parameters.json` filename when invoking the Action, and observe the output:

  ```
  bx wsk action invoke --result hello --param-file parameters.json
  ```

  **Output:**
  ```json
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
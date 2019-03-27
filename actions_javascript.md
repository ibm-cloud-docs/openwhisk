---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

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


# Creating JavaScript actions
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

The following sections guide you through working with actions in JavaScript. Start by creating and invoking a simple action. Then, add parameters to the action and invoke that action with parameters. Next, set default parameters and invoke them. Finally, create asynchronous actions.
{: shortdesc}

## Creating and invoking a simple JavaScript action
{: #single_action_js}
{: #openwhisk_single_action_js}

Review the following steps and examples to create your first JavaScript action.

1. Save the following code in a file named `hello.js`.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  The JavaScript file might contain additional functions. However, by convention, a function called `main` must exist to provide the entry point for the action.
  {: shortdesc}

2. Create an action called `hello` using the JavaScript function.

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Example output:
  
  ```
  ok: created action hello
  ```
  {: screen}

  The type of action is determined by using the source file extension. For `.js` source files, the action runs by using a Node.js runtime. You can specify the Node.js runtime version for your JavaScript action by setting the `--kind` parameter to `nodejs:10` or `nodejs:8`. For more information about the Node.js runtime, see [Runtimes](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).
  {: shortdesc}

3. Verify that your `hello` action is in your actions list.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  Example output:
  
  ```
  actions
  hello       private
  ```
  {: screen}

4. Run the action in the cloud by running a blocking invocation. Blocking invocations use a request/response style and wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time limit value](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits).

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    The command outputs the following information:
        * The activation ID (`44794bd6aab74415b4e42a308d880e5b`) which can be used to retrieve the logs or the result of the invocation
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

5. If you don't need the action result right away, you can omit the `--blocking` flag to run a non-blocking invocation.

    1. Run a non-blocking invocation.

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Example output:

        ```
        ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Use the activation ID to obtain the action result.

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        The action result is returned:

        ```
        {
            "payload": "Hello world"
        }
        ```
        {: screen}

6. If you forget to record the activation ID, you can get a list of activations ordered from most to least recent.

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
    
### Javascript function prototype
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript actions run in a Node.js runtime.

Actions that are written in JavaScript must be confined to a single file. The file can contain multiple functions, but by convention, a function that is called `main` must exist, and is the one called when the action is invoked. For example, the following example shows an action with multiple functions.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

The action input parameters are passed as a JSON object as a parameter to the `main` function. The result of a successful activation is also a JSON object but is returned differently depending on whether the action is synchronous or asynchronous as described in the following section.

### Synchronous and asynchronous behavior
{: #openwhisk_ref_javascript_synchasynch}

It is common for JavaScript functions to continue execution in a callback function even after a return. To accommodate this behavior, an activation of a JavaScript action can be *synchronous* or *asynchronous*.

A JavaScript action's activation is **synchronous** if the main function exits under one of the following conditions:

- The main function exits without executing a `return` statement.
- The main function exits by executing a `return` statement that returns any value *except* a Promise.

See the following example of a synchronous action:

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

A JavaScript action's activation is **asynchronous** if the main function exits by returning a Promise. In this case, the system assumes that the action is still running until the Promise is fulfilled or rejected.
Start by instantiating a new Promise object and passing it a callback function. The callback takes two arguments, resolve and reject, which are both functions. All your asynchronous code goes inside that callback.

In the following example, you can see how to fulfill a Promise by calling the resolve function.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
     })
}
```
{: codeblock}

This example shows how to reject a Promise by calling the reject function.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
     })
}
```
{: codeblock}

It is possible for an action to be synchronous on some inputs and asynchronous on others as shown in the following example.
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
        return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
          }, 100);
        })
     }  else {
        // synchronous activation
        return {done: true};
     }
}
```
{: codeblock}

Regardless of whether an activation is synchronous or asynchronous, the invocation of the action can be blocking or non-blocking.

## Creating asynchronous actions
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

JavaScript functions that run asynchronously can return the activation result after the `main` function returns by returning a Promise in your action.

1. Save the following code in a file named `asyncAction.js`.

    ```javascript
    function main(args) {
         return new Promise(function(resolve, reject) {
           setTimeout(function() {
             resolve({ done: true });
           }, 2000);
        })
     }
    ```
    {: codeblock}

    * The `main` function returns a Promise. The Promise indicates that the activation isn't completed yet but is expected to in the future.

    * The `setTimeout()` JavaScript function waits for 2 seconds before calling the Promise's callback function, which represents the asynchronous code.

    * The Promise's callback accepts the arguments `resolve` and `reject`, which are both functions.

      * The call to `resolve()` fulfills the Promise and indicates that the activation completes normally.
      * A call to `reject()` can be used to reject the Promise and signal that the activation completes abnormally.

2. Create an action called `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. Invoke the action.

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

4. Check how long the activation took to complete by getting the activation log.

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

## Using actions to call an external API
{: #apicall_action}
{: #openwhisk_apicall_action}

The examples provided so far are self-contained JavaScript functions. You can also create an action that calls an external API.
{: shortdesc}

The following example invokes the NASA Astronomy Picture of the Day (APOD) service which provides a unique image of our universe every day.

1. Save the following code in a file named `apod.js`.

    ```javascript
    let rp = require('request-promise')

    function main(params) {
        const options = {
            uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
        .then(res => {
            return { response: res }
        })
    }
    ```
    {: codeblock}

2. A call is made to the NASA APOD API, and fields are extracted from the JSON result. For more information about which Node.js packages you can use in your actions, see [Runtimes](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).

3. Create an action named `apod`.
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. Invoke the `apod` action.
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    The following example object is returned:

    ```
    {
      "copyright": "Eric Houck",
      "date": "2018-03-28",
      "explanation": "Does an alignment like this occur only once in a blue moon? ...",
      "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
      "media_type": "image",
      "service_version": "v1",
      "title": "Blue Moon Tree",
      "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
    }
    ```
    {: screen}

## Packaging an action as a Node.js module
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

As an alternative to writing all your action code in a single JavaScript source file, you can write an action as a `npm` package.

For example, consider a directory with the following files:

1. Save the following code in a file named `package.json`.

    ```json
    {
      "name": "my-action",
      "main": "index.js",
      "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. Save the following code in a file named `index.js`.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * The action is exposed through `exports.main`.
    * The action handler can have any name as long as it conforms to the conventional signature of accepting an object and returning an object (or a `Promise` of an object).
    * You must either name this file `index.js` or specify the file name that you prefer as the `main` property in `package.json`.

3. Install all dependencies locally.

    ```
    npm install
    ```
    {: pre}

    **Note**: While most `npm` packages install JavaScript sources on `npm install`, some also install and compile binary artifacts. The archive file upload supports only JavaScript dependencies. Action invocations might fail if the archive includes binary dependencies.

4. Create a `.zip` archive containing all files, including all dependencies.

    ```
    zip -r action.zip *
    ```
    {: pre}

    Using the Windows Explorer action for creating the zip file results in an incorrect structure. {{site.data.keyword.openwhisk_short}} .zip actions must have `package.json` at the root of the zip, while Windows Explorer places it inside a nested folder. The safest option is to use the command line `zip` command.
    {: tip}

5. Create the action. When you create an action from a `.zip` archive, you must set a value for the `--kind` parameter to specify your Node.js runtime version. Choose between `nodejs:8` or `nodejs:10`.

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. Invoke the action.

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    Example output:

    ```
    {
        "padded": [
            ".......................and now",
            "......for something completely",
            ".....................different"
        ]
    }
    ```
    {: screen}

## Packaging an action as a single bundle
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

If packaging the action as a .zip includes too many unnecessary files or if you need a faster deployment, you can write the minimal code into a single `.js` file that includes dependencies.
{: shortdesc}

You can package an action by using a JavaScript module bundler such as [webpack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://webpack.js.org/concepts/). When `webpack` processes your code, it recursively builds a dependency graph that includes every module that your action needs.

1. Save the following code in a file named `package.json`. `webpack` is added as a development dependency.

    ```json
    {
      "name": "my-action",
      "main": "dist/bundle.js",
      "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
      "dependencies": {
        "left-pad": "1.1.3"
      },
      "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Save the following webpack configuration code in a file named `webpack.config.js`.

    ```javascript
    var path = require('path');
    module.exports = {
      entry: './index.js',
      output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
      },
      target: 'node'
    };
    ```
    {: codeblock}

3. Save the following code in a file named `index.js`. The variable `global.main` is set to the main function of the action.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Install all dependencies locally.

    ```
    npm install
    ```
    {: pre}

5. Build the webpack bundle.

    ```
    npm run build
    ```
    {: pre}

    The file `dist/bundle.js` is created and deploys as the action source code.

6. Create the action using the `npm` script or the CLI.

    * Using the `npm` script:

        ```
        npm run deploy
        ```
        {: pre}

    * Using the CLI:

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    The bundle file that is built by `webpack` supports only JavaScript dependencies. Action invocations might fail if the bundle depends on binary file dependencies because this is not included with the file `bundle.js`.
    {: tip}
    




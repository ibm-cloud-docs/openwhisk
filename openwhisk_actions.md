---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creating and invoking Actions
{: #openwhisk_actions}

Actions are stateless code snippets that run on the {{site.data.keyword.openwhisk}} platform. For example, an Action can be used to detect the faces in an image, respond to a database change, aggregate a set of API calls, or post a Tweet. An Action can be written as a JavaScript, Swift, Python, PHP function, Java method, or any binary-compatible executable, including Go programs and custom executables packaged as Docker containers.
{:shortdesc}

Actions can be explicitly invoked, or run in response to an event. In either case, each run of an Action results in an activation record that is identified by a unique activation ID. The input to an Action and the result of an Action are a dictionary of key-value pairs, where the key is a string and the value a valid JSON value. Actions can also be composed of calls to other Actions or a defined sequence of Actions.

Learn how to create, invoke, and debug Actions in your preferred development environment:
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [Arbitrary executables](#creating-actions-arbitrary)

In addition, learn about:
* [Monitoring Action output](#monitor-action-output)
* [Getting Actions](#getting-actions)
* [Listing Actions](#listing-actions)
* [Deleting Actions](#deleting-actions)
* [Large application support](#large-app-support)
* [Accessing Action metadata within the Action body](#accessing-action-metadata-within-the-action-body)

## Creating and invoking JavaScript actions
{: #creating-and-invoking-javascript-actions}

The following sections guide you through working with Actions in JavaScript. You begin with the creation and invocation of a simple action. Then, you move on to adding parameters to an action and invoking that action with parameters. Next, is setting default parameters and invoking them. Then, you create asynchronous Actions, and finally work with action sequences.

### Creating and invoking a simple JavaScript action
{: #openwhisk_single_action_js}

Review the following steps and examples to create your first JavaScript action.

1. Create a JavaScript file with the following content. For this example, name the file **hello.js**.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  The JavaScript file might contain additional functions. However, by convention, a function called **main** must exist to provide the entry point for the action.

2. Create an Action from the following JavaScript function. For this example, the Action is called **hello**.
  ```
  bx wsk action create hello hello.js
  ```
  {: pre}

  **Output:**
  ```
  ok: created action hello
  ```
  {: screen}

  The CLI automatically infers the type of the action by using the source file extension. For `.js` source files, the action runs by using a Node.js 6 runtime. You can also create an action that runs with Node.js 8 by explicitly specifying the parameter `--kind nodejs:8`. For more information, see the Node.js 6 vs 8 [reference](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

3. List the Actions that you created:
  ```
  bx wsk action list
  ```
  {: pre}

  **Output:**
  ```
  actions
  hello       private
  ```
  {: screen}

  You can see the **hello** Action you created.

4. After you create your Action, you can run it in the cloud in with the **invoke** command. You can invoke Actions with a *blocking* invocation (that is, request/response style) or a *non-blocking* invocation by specifying a flag in the command. A blocking invocation request _waits_ for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time limit value](./openwhisk_reference.html#openwhisk_syslimits). The result of the activation is returned if it is available within the wait period. Otherwise, the activation continues processing in the system, and an activation ID is returned so that one can check for the result later, as with non-blocking requests (see [here](#monitor-action-output) for tips on monitoring activations).

  This example uses the blocking parameter, `--blocking`:
  ```
  bx wsk action invoke --blocking hello
  ```
  {: pre}

  The command outputs two important pieces of information:
  * The activation ID (`44794bd6aab74415b4e42a308d880e5b`)
  * The invocation result if it is available within the expected wait period

  **Output displays activation ID:**
  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```
  {: screen}

  **Invocation result:**
  ```
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

  The result in this case is the string `Hello world` returned by the JavaScript function. The activation ID can be used to retrieve the logs or result of the invocation at a future time.

5. If you don't need the Action result right away, you can omit the `--blocking` flag to make a non-blocking invocation. You can get the result later by using the activation ID.

  See the following examples:
  ```
  bx wsk action invoke hello
  ```
  {: pre}

  **Command output:**
  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: screen}

  Now that you know the activation ID, you can specify it to obtain the Action result:
  ```
  bx wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  **Action result:**
  ```
  {
      "payload": "Hello world"
  }
  ```
  {: screen}

6. If you forget to record the activation ID, you can get a list of activations ordered from the most recent to the oldest. Run the following command to get a list of your activations:

  **List activations:**
  ```
  bx wsk activation list
  ```
  {: pre}

  Output:
  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```
  {: screen}

### Creating asynchronous Actions
{: #openwhisk_asynchrony_js}

JavaScript functions that run asynchronously can return the activation result after the `main` function returns by returning a Promise in your Action.

1. Save the following content in a file called **asyncAction.js**.
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

  Notice that the `main` function returns a Promise, which indicates that the activation isn't completed yet, but is expected to in the future.

  The `setTimeout()` JavaScript function in this case waits for 2 seconds prior to calling the callback function, which represents the asynchronous code, and goes inside the Promise's callback function.

  The Promise's callback takes two arguments, resolve and reject, which are both functions.  The call to `resolve()` fulfills the Promise and indicates that the activation completes normally.

  A call to `reject()` can be used to reject the Promise and signal that the activation completes abnormally.

2. Run the following commands to create the Action and invoke it.

  Create an Action called **asyncAction**:
  ```
  bx wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  Invoke the Action:
  ```
  bx wsk action invoke --result asyncAction
  ```
  {: pre}

  **Output:**
  ```
  {
      "done": true
  }
  ```
  {: screen}

  Notice that you performed a blocking invocation of an asynchronous action.

3. Fetch the activation log to see how long the activation took to complete.

  To do this, first list the Action to obtain the activation ID:
  ```
  bx wsk activation list --limit 1 asyncAction
  ```
  {: pre}

  **Output:**
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```
  {: screen}

  Now get the activation log info using the activation ID:
  ```
  bx wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}

  ```
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```
  {: screen}

  Comparing the `start` and `end` time stamps in the activation record, you can see that this activation took slightly over 2 seconds to complete.

### Using Actions to call an external API
{: #openwhisk_apicall_action}

The examples provided so far are self-contained JavaScript functions. You can also create an Action that calls an external API.

The following example invokes the NASA Astronomy Picture of the Day (APOD) service which provides a unique image of our universe every day.

1. Save the following content in a file named **apod.js**.
  ```javascript
  var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

  $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
      $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
      $("#copyright").text("Image Credits: " + "Public Domain");
    }

    if(result.media_type == "video") {
      $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
      $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
    $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
  });
  ```
  {: codeblock}

  A call is made to the NASA APOD API, and extracts fields from the JSON result. The [References](./openwhisk_reference.html#openwhisk_ref_javascript_environments) topic details the Node.js packages that you can use in your Actions.

2. Run the following commands to create the Action and invoke it.

  Create the Action named **apod**:
  ```
  bx wsk action create apod apod.js
  ```
  {: pre}

  Invoke the **apod** Action:
  ```
  bx wsk action invoke --result apod
  ```
  {: pre}

  **Return Object:**
  ```
  {
    "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? No, although it was during a blue moon that this single-shot image was taken.  During a full moon that happened to be the second of the month -- the situation that defines a blue moon -- the photographer created the juxtaposition in late January by quickly moving around to find just the right spot to get the background Moon superposed behind the arc of a foreground tree.  Unfortunately, in this case, there seemed no other way than getting bogged down in mud and resting the camera on a barbed-wire fence.  The arc in the oak tree was previously created by hungry cows in Knight's Ferry, California, USA.  Quirky Moon-tree juxtapositions like this can be created during any full moon though, given enough planning and time.  Another opportunity will arise this weekend, coincidently during another blue moon. Then, the second blue moon in 2018 will occur, meaning that for the second month this year, two full moons will appear during a single month (moon-th).  Double blue-moon years are relatively rare, with the last occurring in 1999, and the next in 2037.",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
  ```
  {: screen}

### Packaging an Action as a Node.js module
{: #openwhisk_js_packaged_action}

As an alternative to writing all your Action code in a single JavaScript source file, you can write an Action as a `npm` package. Consider as an example, a directory with the following files:

**package.json:**
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

**index.js:**
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

The Action is exposed through `exports.main`. The action handler itself can have any name, as long as it conforms to the usual signature of accepting an object and returning an object (or a `Promise` of an object). Per Node.js convention, you must either name this file `index.js` or specify the file name that you prefer as the `main` property in package.json.

To create a {{site.data.keyword.openwhisk_short}} Action from this package:

1. Install all dependencies locally:
  ```
  npm install
  ```
  {: pre}

2. Create a `.zip` archive containing all files (including all dependencies):
  ```
  zip -r action.zip *
  ```
  {: pre}

  Using the Windows Explorer action for creating the zip file results in an incorrect structure. {{site.data.keyword.openwhisk_short}} zip Actions must have `package.json` at the root of the zip, while Windows Explorer places it inside a nested folder. The safest option is to use the command line `zip` command.
  {: tip}

3. Create the action:
  ```
  bx wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  When creating an action from a `.zip` archive with the CLI tool, you must explicitly provide a value for the `--kind` flag by using `nodejs:6` or `nodejs:8`.

4. You can invoke the Action like any other:
  ```
  bx wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}

  **Output:**
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

Finally, note that while most `npm` packages install JavaScript sources on `npm install`, some also install and compile binary artifacts. The archive file upload currently does not support binary dependencies but rather only JavaScript dependencies. Action invocations may fail if the archive includes binary dependencies.

### Packaging an Action as a single bundle
{: #openwhisk_js_webpack_action}

It is convenient to only include the minimal code into a single `.js` file that includes dependencies. This approach allows for faster deployments, and in some circumstances where packaging the action as a zip might be too large because it includes unnecessary files.

You can use a JavaScript module bundler such as [webpack](https://webpack.js.org/concepts/). When webpack processes your code, it recursively builds a dependency graph that includes every module that your action needs.

Here is a quick example using webpack:

Taking the previous example `package.json` add `webpack` as a development depency and add some npm script commands.
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
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

Create the webpack configuration file `webpack.config.js`.
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

Set the variable `global.main` to the main function of the Action.

From the previous example:
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

If your function name is `main`, use the following syntax instead:
```javascript
global.main = main;
```
{: codeblock}

To build and deploy an OpenWhisk Action using `npm` and `webpack`:

1. First, install dependencies locally:
  ```
  npm install
  ```
  {: pre}

2. Build the webpack bundle:
  ```
  npm run build
  ```
  {: pre}

  The file `dist/bundle.js` is created, and is used to deploy as the Action source code.

3. Create the Action using the `npm` script or the CLI.

  Using `npm` script:
  ```
  npm run deploy
  ```
  {: pre}

  Using the CLI:
  ```
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}

Finally, the bundle file that is built by `webpack` doesn't support binary dependencies but rather JavaScript dependencies. So Action invocations will fail if the bundle depends on binary dependencies, because this is not included with the file `bundle.js`.

## Creating Action sequences
{: #openwhisk_create_action_sequence}

You can create an Action that chains together a sequence of Actions.

Several utility Actions are provided in a package that is called `/whisk.system/utils` that you can use to create your first sequence. You can learn more about packages in the [Packages](./openwhisk_packages.html) section.

1. Display the Actions in the `/whisk.system/utils` package.
  ```
  bx wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  **Output:**
  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```
  {: screen}

  You will be using the `split` and `sort` Actions in this example.

2. Create an Action sequence so that the result of one Action is passed as an argument to the next Action.
  ```
  bx wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  This Action sequence converts some lines of text to an array, and sorts the lines.

3. Invoke the Action:
  ```
  bx wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  **Output:**
  ```
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```
  {: screen}

  In the result, you see that the lines are sorted.

**Note**: Parameters that are passed between Actions in the sequence are explicit, except for default parameters.
Therefore, parameters that are passed to the action sequence are only available to the first action in the sequence. The result of the first action in the sequence becomes the input JSON object to the second action in the sequence (and so on). This object does not include any of the parameters that are originally passed to the sequence unless the first action explicitly includes them in its result. Input parameters to an action are merged with the action's default parameters, with the former taking precedence and overriding any matching default parameters. For more information about invoking action sequences with multiple named parameters, see [Setting default parameters on an Action](./parameters.html#default-params-action).

## Creating Python actions
{: #creating-python-actions}

The process of creating Python Actions is similar to that of JavaScript Actions. The following sections guide you through creating and invoking a single Python action, and adding parameters to that action.

### Creating and invoking a Python action
{: #openwhisk_actions_python_invoke}

An Action is simply a top-level Python function. For example, create a file called **hello.py** with the following source code:
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

Python Actions always consume a dictionary and produce a dictionary. The entry method for the action is `main` by default but can be specified explicitly to create the action with the `wsk` CLI by using `--main`, as with any other action type.

You can create {{site.data.keyword.openwhisk_short}} Action called **helloPython** from this function as follows:
```
bx wsk action create helloPython hello.py
```
{: pre}

The CLI automatically infers the type of the action from the source file extension. For `.py` source files, the action runs by using a Python 2 runtime. You can also create an action that runs with Python 3 by explicitly specifying the parameter `--kind python:3`. In addition there is a Python 3 runtime with kind `python-jessie:3` that contains additional packages for IBM Cloud Services like IBM Cloudant, IBM DB2, IBM COS, and IBM Watson.
For more information about packages included in this Python 3 runtime, see the Python runtime [reference](./openwhisk_reference.html#openwhisk_ref_python_environments).

Action invocation is the same for Python Actions as it is for JavaScript Actions:
```
bx wsk action invoke --result helloPython --param name World
```
{: pre}

**Output:**
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### Packaging Python actions in zip files
{: #openwhisk_actions_python_zip}

You can package a Python Action and dependent modules in a zip file.
The filename of the source file that contains the entry point (e.g., `main`) must be `__main__.py`.
For example, to create an Action with a helper module called `helper.py`, first create an archive containing your source files:
```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

Then create the Action:
```bash
bx wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

While these steps are shown for Python 3 (with kind `python:3`), you can do the same with alternative Python kinds `python:2` or `python-jessie:3`.

### Packaging Python actions with a virtual environment in zip files
{: #openwhisk_actions_python_virtualenv}

Another way of packaging Python dependencies is by using a virtual environment (`virtualenv`) which allows you to link additional packages that can be installed by using [`pip`](https://packaging.python.org/installing/) for example.

As with basic zip file support, the name of the source file that contains the main entry point must be `__main__.py`. To clarify, the contents of `__main__.py` is the main function, so for this example you can rename `hello.py` to `__main__.py` from the previous section. In addition, the virtualenv directory must be named `virtualenv`. See the following example scenario for installing dependencies, packaging them in a virtualenv, and creating a compatible OpenWhisk action.

To ensure compatibility with the OpenWhisk runtime container, package installations inside a virtualenv must be done in the target environment using the corresponding image to the kind.
- For kind `python:2` use the docker image `openwhisk/python2action`.
- For kind `python:3` use the docker image `openwhisk/python3action`.
- For kind `python-jessie:3` use the docker image `ibmfunctions/action-python-v3`.

1. Given a [requirements.txt ![External link icon](../icons/launch-glyph.svg "External link icon")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) file that contains the `pip` modules and versions to install, run the following to install the dependencies and create a virtualenv using a compatible Docker image:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. Archive the virtualenv directory and any additional Python files:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Create the Action **helloPython**:
    ```
    bx wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Only add those modules to the `requirements.txt` that are not part of the selected runtime environment. This helps to keep the virtualenv to a minimum size.
{: tip}

## Creating PHP actions
{: #creating-php-actions}

The process of creating PHP Actions is similar to that of JavaScript Actions. The following sections guide you through creating and invoking a single PHP action, and adding parameters to that action.

### Creating and invoking a PHP action
{: #openwhisk_actions_php_invoke}

An action is simply a top-level PHP function. For example, create a file called `hello.php` with the following source code:

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```
{: codeblock}

PHP Actions always consume an associative array and return an associative array. The entry method for the Action is `main` by default but can be specified explicitly when you create the Action with the `bx wsk` CLI by using `--main`, as with any other action type.

You can create a {{site.data.keyword.openwhisk_short}} Action called **helloPHP** from this function as follows:
```
bx wsk action create helloPHP hello.php
```
{: pre}

The CLI automatically infers the type of the action from the source file extension. For `.php` source files, the action runs by using a PHP 7.1 runtime. For more information, see the PHP [reference](./openwhisk_reference.html#openwhisk_ref_php).

Action invocation is the same for PHP Actions as it is for JavaScript Actions:
```
bx wsk action invoke --result helloPHP --param name World
```
{: pre}

**Output:**
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### Packaging PHP actions in zip files
{: #openwhisk_actions_php_zip}

You can package a PHP Action along with other files and dependent packages in a zip file.
The filename of the source file that contains the entry point (for example, `main`) must be `index.php`.

For example, to create an Action that includes a second file that is called `helper.php`, first create an archive that contains your source files:
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Then create the Action **helloPHP**:
```bash
bx wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Creating Swift actions
{: #creating-swift-actions}

The process of creating Swift actions is similar to that of JavaScript actions. The following sections guide you through creating and invoking a single Swift action, and packaging an Action in a zip file.

You can also use the online [Online Swift Playground](http://online.swiftplayground.run) to test your Swift code without having to install Xcode on your machine.

**Attention:** Swift actions run in a Linux environment. Swift on Linux is still in
development, and OpenWhisk generally uses the latest available release, which is not necessarily stable. In addition, the version of Swift that is used with OpenWhisk might be inconsistent with versions of Swift from stable releases of Xcode on MacOS.

### Creating and invoking an Action

#### Swift 3
An Action is simply a top-level Swift function. For example, create a file called
**hello.swift** with the following content:

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

In this example, the Swift action consumes a dictionary and produces a dictionary.

You can create an OpenWhisk action called **helloSwift** from this function as
follows:
```
bx wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}

#### Swift 4

New in Swift 4, in addition to the above main function signature, are two more signatures out of the box which take advantage of the [Codable](https://developer.apple.com/documentation/swift/codable) type. You can learn more about data types that are encodable and decodable for compatibility with external representations such as JSON [here](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

The following example takes an input parameter as **Codable Input** with field `name`, and returns a **Codable output** with a field `greetings`.
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

In this example, the Swift action consumes a Codable and produces a Codable type.
If you don't need to handle any input, you can use the function signature that doesn't take any input, only Codable output.
```swift
struct Output: Codable {
    let greeting: String
}
func main(completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello OpenWhisk!")
    completion(result, nil)
}
```
{: codeblock}

You can create an OpenWhisk action called `helloSwift` from this function as
follows:
```
bx wsk action create helloSwift hello.swift --kind swift:4.1
```
{: pre}

See the Swift [reference](./openwhisk_reference.html#swift-actions) for more information about the Swift runtime.

Action invocation is the same for Swift actions as it is for JavaScript actions:
```
bx wsk action invoke --result helloSwift --param name World
```
{: pre}

**Output:**
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

Find out more about parameters in the [Working with parameters](./parameters.html) topic.

### Packaging an Action as a Swift executable
{: #packaging-an-action-as-a-swift-executable}

When you create an OpenWhisk Swift action with a Swift source file, it has to be compiled into a binary before the action is run. Once done, subsequent calls to the action are much faster until the container holding your action is purged. This delay is known as the cold-start delay.

To avoid the cold-start delay, you can compile your Swift file into a binary and then upload to OpenWhisk in a zip file. As you need the OpenWhisk scaffolding, the easiest way to create the binary is to build it within the same environment as it will be run in.

### Using a script to build Swift packaged Action

You can use a script to automate the packaging of the Action. Create a script file named `compile.sh` using the following sample code.
```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
    exit 2
fi
OUTPUT_DIR="build"
if [ ${2} == "swift:3.1.1" ]; then
  BASE_PATH="/swift3Action"
  DEST_SOURCE="$BASE_PATH/spm-build"
  RUNTIME="openwhisk/action-swift-v3.1.1"
elif [ ${2} == "swift:4.1" ]; then
  RUNTIME="ibmfunctions/action-swift-v4.1"
  BASE_PATH="/swift4Action"
  DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
else
  echo "Error: Kind $2 not recognize"
  exit 3
fi
DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

BUILD_FLAGS=""
if [ -n "$3" ] ; then
    BUILD_FLAGS=${3}
fi

echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

echo 'Setting up build...'
cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

# action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

The script assumes you have a directory called `actions`, with each top level directory representing an Action.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

- Create the `Package.swift` file to add dependencies. **Note:** The syntax is different from Swift 3 to Swift 4 tools.

  Swift 3 example syntax:
  ```swift
  import PackageDescription

  let package = Package(
    name: "Action",
        dependencies: [
            .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
            .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
            .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
            .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
            .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
        ]
  )
  ```
  {: codeblock}

  Swift 4 example syntax:
  ```swift
  // swift-tools-version:4.0
  import PackageDescription

  let package = Package(
      name: "Action",
      products: [
        .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
        .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
      ],
      targets: [
        .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
      ]
  )
  ```
  {: codeblock}

  As you can see, this example adds `example-package-deckofplayingcards` as a dependency. Notice that `CCurl`, `Kitura-net` and `SwiftyJSON` are provided in the standard Swift action, so you should include them in your own `Package.swift` only for Swift 3 actions.

- Build the Action by running the following command for a Swift 3 action:
  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  To compile for Swift 4 use `swift:4.1` instead of `swift:3.1.1`:
  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  This has created `hello.zip` in the `build`.

- Upload it to OpenWhisk with the Action name **helloSwifty**. For Swift 3, use the kind `swift:3.1.1`
  ```
  bx wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  For Swift 4, use the kind `swift:3.1.1`:
  ```
  bx wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

- To check how much faster it is, run the following command:
  ```
  bx wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  The time that it took for the Action to run is in the "duration" property and compare to the time it takes to run with a compilation step in the **hello** Action.

### Error Handling in Swift 4

With the new Codable completion handler, you can pass an Error to indicate a failure in your Action.
[Error handling in Swift](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) resembles exception handling in other languages, with the use of the `try, catch` and `throw` keywords.

The following snippet shows a an example of handling an error:
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}

## Creating Java actions
{: #creating-java-actions}

The process of creating Java Actions is similar to that of JavaScript and Swift Actions. The following sections guide you through creating and invoking a single Java action, and adding parameters to that Action.

In order to compile, test, and archive Java files, you must have a [JDK 8](http://openjdk.java.net/install) installed locally.

### Creating and invoking a Java action
{: #openwhisk_actions_java_invoke}

A Java action is a Java program with a method called `main` that has the exact signature as follows:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

For example, create a Java file called `Hello.java` with the following content:

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

Then, compile `Hello.java` into a JAR file `hello.jar` as follows:
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

[google-gson](https://github.com/google/gson) must exist in your Java CLASSPATH to compile the Java file.
{: tip}

You can create a {{site.data.keyword.openwhisk_short}} Action called **helloJava** from this JAR file as
follows:
```
bx wsk action create helloJava hello.jar --main Hello
```
{: pre}

When you use the command line and a `.jar` source file, you do not need to specify that you are creating a Java Action; the tool determines that from the file extension.

You need to specify the name of the main class by using `--main`. An eligible main class is one that implements a static `main` method. If the class is not in the default package, use the Java fully qualified class name, for example, `--main com.example.MyMain`.

If needed, you can also customize the method name of your Java action. This is done by specifying the Java fully-qualified method name of your Action, for example, `--main com.example.MyMain#methodName`.

Action invocation is the same for Java Actions as it is for Swift and JavaScript Actions:
```
bx wsk action invoke --result helloJava --param name World
```
{: pre}

**Output:**
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

## Creating Docker actions
{: #creating-docker-actions}

With {{site.data.keyword.openwhisk_short}} Docker Actions, you can write your Actions in any language.

Your code is compiled into an executable binary and embedded into a Docker image. The binary program interacts with the system by taking input from `stdin` and replying through `stdout`.

As a prerequisite, you must have a Docker Hub account.  To set up a free Docker ID and account, go to [Docker Hub](https://hub.docker.com).

For the instructions that follow, assume that the Docker user ID is `janesmith` and the password is `janes_password`.  Assuming that the CLI is set up, three steps remain to set up a custom binary for use by {{site.data.keyword.openwhisk_short}}. After that, the uploaded Docker image can be used as an action.

1. Download the Docker skeleton. You can download and install it by using the CLI as follows:
  ```
  bx wsk sdk install docker
  ```
  {: pre}

  The Docker skeleton is now installed at the current directory.
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  **Output:**
  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```
  {: screen}

  The skeleton is a Docker container template where you can inject your code in the form of custom binaries.

2. Set up your custom binary in the blackbox skeleton. The skeleton already includes a C program that you can use.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  You can modify this file as needed, or, add additional code and dependencies to the Docker image.
  If the latter, you can tweak the `Dockerfile` as necessary to build your executable.
  The binary must be located inside the container at `/action/exec`.

  The executable receives a single argument from the command line. It is a string serialization of the JSON object that represents the arguments to the action. The program may log to `stdout` or `stderr`.
  By convention, the last line of output _must_ be a stringified JSON object which represents the result of the action.

3. Build the Docker image and upload it using a supplied script. You must first run `docker login` to authenticate, and then run the script with a chosen image name.
  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  Notice that part of the `example.c` file is compiled as part of the Docker image build process, so you do not need C compiled on your machine. In fact, unless you are compiling the binary on a compatible host machine, it can not run inside the container since the formats do not match.

  Your Docker container can now be used as a {{site.data.keyword.openwhisk_short}} Action:
  ```
  bx wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Notice the use of `--docker` to create an Action. All Docker images are assumed to be hosted on Docker Hub. The action can be invoked as any other {{site.data.keyword.openwhisk_short}} action.
  ```
  bx wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  **Invocation output:**
  ```
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
  {: screen}

  To update the Docker action, run `buildAndPush.sh` to upload the latest image to Docker Hub. This allows the system to pull your new Docker image the next time it runs the code for your action. If there are no warm containers, new invocations use the new Docker image. However, if there is a warm container that uses a previous version of your Docker image, any new invocations continue to use that image unless you run `bx wsk action update`. This indicates to the system, that for new invocations, to execute a docker pull to get your new Docker image.

  **Upload latest image to Docker Hub:**
  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  **Update the Action so that new invocations begin using the new image:***
  ```
  bx wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  You can find more information about creating Docker actions in the [References](./openwhisk_reference.html#openwhisk_ref_docker) section.

  The previous version of the CLI supported `--docker` without a parameter and the image name was a positional argument. In order to allow Docker actions to accept initialization data by using a (zip) file, normalize the user experience for Docker Actions so that a positional argument, if present, must be a file (for example, a zip file) instead. The image name must be specified following the `--docker` option. Thanks to user feedback, the `--native` argument is included as shorthand for `--docker openwhisk/dockerskeleton`, so that executables that run inside the standard Docker action SDK are more convenient to create and deploy.

  For example, this tutorial creates a binary executable inside the container located at `/action/exec`. If you copy this file to your local file system and zip it into `exec.zip`, then you can use the following commands to create a Docker action that receives the executable as initialization data.

  **Create action from zip file:**
  ```
  bx wsk action create example exec.zip --native
  ```
  {: pre}

  Which is equivalent to the following command.
  ```
  bx wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Creating Go actions
{: #creating-go-actions}

The `--native` option allows for packaging of any executable as an Action. This works for Go as an example. As with Docker actions, the Go executable receives a single argument from the command line. It is a string serialization of the JSON object representing the arguments to the Action. The program may log to `stdout` or `stderr`. By convention, the last line of output _must_ be a stringified JSON object which represents the result of the Action.

Here is an example Go action.
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //program receives one argument: the JSON object as a string
    arg := os.Args[1]

    // unmarshal the string to a JSON object
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // can optionally log to stdout (or stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // last line of stdout is the result JSON object as a string
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```
{: codeblock}

Save the code above to a file `sample.go` and cross compile it for OpenWhisk. The executable must be called `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
bx wsk action create helloGo --native exec.zip
```
{: codeblock}

The Action may be run as any other Action.
```bash
bx wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Logs are retrieved in a similar way as well.
```bash
bx wsk activation logs --last --strip
my first Go action.
```

## Creating actions using arbitrary executables
{: #creating-actions-arbitrary}

Using `--native`, you can see that _any_ executable may be run as an OpenWhisk action. This includes `bash` scripts, or cross compiled binaries. For the latter, the constraint is that the binary must be compatible with the `openwhisk/dockerskeleton` image.

## Monitoring Action output
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}} Actions might be invoked by other users, in response to various events, or as part of an action sequence. In such cases, it can be useful to monitor the invocations.

You can use the {{site.data.keyword.openwhisk_short}} CLI to watch the output of Actions as they are invoked.

1. Issue the following command from a shell:
  ```
  bx wsk activation poll
  ```
  {: pre}

  This command starts a polling loop that continuously checks for logs from activations.

2. Switch to another window and invoke an Action:
  ```
  bx wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  **Output:**
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

  Similarly, whenever you run the poll utility, you see in real time the logs for any Actions that are run on your behalf in OpenWhisk.

## Getting Actions
{: #getting-actions}

Metadata that describes existing Actions can be retrieved by using the `bx wsk action` get command.

**Command:**
```
bx wsk action get hello
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

### Getting an Action URL
{: #get-action-url}

An Action can be invoked by using the REST interface through an HTTPS request. To get an Action URL, execute the following command:
```
bx wsk action get actionName --url
```
{: pre}

A URL with the following format is returned for standard Actions:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

For [Web Actions](./openwhisk_webactions.html), a URL is returned in the the following format:
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Note:** For standard Actions, authentication must be provided when invoked through an HTTPS request. For more information regarding Action invocations using the REST interface, see [REST APIs](./openwhisk_rest_api.html).

### Saving Action code
{: #save-action}

Code associated with an existing Action may be retrieved and saved locally. Saving can be performed on all Actions except sequences and Docker actions.

1. Save Action code to a filename that corresponds with an existing Action name in the current working directory. A file extension that corresponds to the Action kind is used, or an extension of .zip will be used for Action code that is a zip file.
  ```
  bx wsk action get actionName --save
  ```
  {: pre}

  **Output:**
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. Instead of allowing the CLI to determine the destination of the code to be saved, a custom file path, filename and extension can be provided by using the `--save-as` flag.
  ```
  bx wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  **Output:**
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## Listing Actions
{: #listing-actions}

You can list all the Actions created using the following command:
```
bx wsk action list
```
{: pre}

As you write more Actions, this list gets longer and it can be helpful to group related Actions into [packages](./openwhisk_packages.html). To filter your list of Actions to just those within a specific package, you can use the following command syntax:
```
bx wsk action list [PACKAGE NAME]
```
{: pre}

## Deleting Actions
{: #deleting-actions}

You can clean up by deleting Actions that you do not want to use.

1. Run the following command to delete an Action:
  ```
  bx wsk action delete hello
  ```
  {: pre}

  **Output:**
  ```
  ok: deleted hello
  ```
  {: screen}

2. Verify that the action no longer appears in the list of Actions.
  ```
  bx wsk action list
  ```
  {: pre}

  **Output:**
  ```
  actions
  ```
  {: screen}

## Large application support
{: #large-app-support}

The maximum code size for an Action is 48 MB. Applications that contain many third-party modules, native libraries, or external tools may run into this limit.

If you happen to create a package Action (zip or jar) that is larger than 48 MB, the solution is to extend the runtime image with dependencies, and then use a single source file or smaller archive than 48 MB.

For example, by building a custom Docker runtime, which includes necessary shared libraries, these dependencies are not required to be present in the archive file. Private source files can still be bundled in the archive and injected at runtime.

Another benefit to reducing archive file sizes, is that deployment times are also improved. Two runtime examples are provided in the following sections to demonstrate how application sizes can be reduced by leveraging this technique.

### Python example

For a Python application, refer to the following steps to reduce it's code size.

1. Place the library `opencv-python` into opencv.
2. Then install the opencv binary into the OS image.
3. You can then use `requirements.txt` and run `pip install requirements.txt` to augment the image with more Python libraries.
4. You can then use `action.py` with the new image.

### Node.js example

In efforts to reduce application size for a Node.js application, see the following steps to install extra packages into the OS image:

1. Install opencv by using `npm`:
   ```
   npm install opencv
   ```
   {: pre}

2. Similarly, if you have a `package.json`, install that by using `npm`:
   ```
   npm install package.json
   ```
   {: pre}

3. Then, proceed to use `action.js` with the new image.

## Accessing Action metadata within the Action body
{: #accessing-action-metadata-within-the-action-body}

The Action environment contains several properties that are specific to the running Action. These properties allow the Action to programmatically work with OpenWhisk assets through the REST API, or set an internal alarm when the Action is about to use up its allotted time budget. The properties are accessible in the system environment for all supported runtimes: Node.js, Python, Swift, Java and Docker Actions when using the OpenWhisk Docker skeleton.

* `__OW_API_HOST` the API host for the OpenWhisk deployment running this Action
* `__OW_API_KEY` the API key for the subject invoking the Action, this key may be a restricted API key
* `__OW_NAMESPACE` the namespace for the _activation_ (this may not be the same as the namespace for the action)
* `__OW_ACTION_NAME` the fully qualified name of the running Action
* `__OW_ACTIVATION_ID` the activation id for this running Action instance
* `__OW_DEADLINE` the approximate time when this Action will have consumed its entire duration quota (measured in epoch milliseconds)

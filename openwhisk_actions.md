---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-17"

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

# Creating and invoking actions
{: #openwhisk_actions}


With {{site.data.keyword.openwhisk}}, you can create stateless code snippets that are set to perform one specific task that is called an action.
{:shortdesc}


**What is an action?**

An action is a small piece of code that can be explicitly invoked or set to automatically run in response to an event. In either case, each run results in a record that is identified by a unique activation ID. The input and the result of an action can be seen as key-value pairs. The key is a string and the value is a valid JSON value. An action can be written in the language of your choice and provided to the service as either source code or a Docker image. The action code runs when it is directly invoked by the Cloud Functions API, CLI, or iOS SDK. An action can automatically respond to events from IBM Cloud or third-party services.

**Why would I use an action?**

By using actions, you limit the amount of time that your code is running, which lowers your overhead costs.

For example, you can use actions to detect faces in an image, respond to changes in a database, aggregate a set of API calls, or even post a tweet.

**Can I use more than one action at a time?**

Yes! You can use actions to call other actions, or you can string actions together to create sequences. To make this work, the output of one action would be the input for another action which would provide an output that can be used to trigger another action and so on. You can even bundle the group of actions that you create to form a package. With a package you can reuse common actions or sequences by calling the package instead of configuring the action or sequence again.


## Creating JavaScript actions
{: #creating-and-invoking-javascript-actions}

The following sections guide you through working with actions in JavaScript. Start by creating and invoking a simple action. Then, add parameters to the action and invoke that action with parameters. Next, set default parameters and invoke them. Finally, create asynchronous actions.

### Creating and invoking a simple JavaScript action
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

  The CLI automatically determines the type of the action by using the source file extension. For `.js` source files, the action runs by using a Node.js runtime. You can specify the Node.js runtime version for your JavaScript action by setting the `--kind` parameter to `nodejs:10` or `nodejs:8`. For more information, see the Node.js [reference](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

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

4. Run the action in the cloud by running a blocking invocation. Blocking invocations use a request/response style and wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's [time limit value](./openwhisk_reference.html#openwhisk_syslimits).
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
    For tips on monitoring activations, see [Monitoring action output](openwhisk_managing.html#monitor-action-output).
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

### Creating asynchronous actions
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

3. Check how long the activation took to complete by getting the activation log.

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

### Using actions to call an external API
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

    A call is made to the NASA APOD API, and fields are extracted from the JSON result. For more information about which Node.js packages you can use in your actions, see [System details and limits](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

2. Create an action named `apod`.
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

### Packaging an action as a Node.js module
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
    * You must either name this file **index.js** or specify the file name that you prefer as the `main` property in **package.json**.

3. Install all dependencies locally.
    ```
    npm install
    ```
    {: pre}
    **Note**: While most `npm` packages install JavaScript sources on `npm install`, some also install and compile binary artifacts. The archive file upload currently supports only JavaScript dependencies. Action invocations might fail if the archive includes binary dependencies.

4. Create a `.zip` archive containing all files, including all dependencies.
    ```
    zip -r action.zip *
    ```
    {: pre}

    Using the Windows Explorer action for creating the zip file results in an incorrect structure. {{site.data.keyword.openwhisk_short}} zip actions must have `package.json` at the root of the zip, while Windows Explorer places it inside a nested folder. The safest option is to use the command line `zip` command.
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

### Packaging an action as a single bundle
{: #openwhisk_js_webpack_action}

If packaging the action as a zip includes too many unnecessary files or if you need a faster deployment, you can write the minimal code into a single `.js` file that includes dependencies.
{: shortdesc}

You can package an action by using a JavaScript module bundler such as [webpack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://webpack.js.org/concepts/). When `webpack` processes your code, it recursively builds a dependency graph that includes every module that your action needs.


1. Save the following code in a file named `package.json`. `webpack` is added as a development depency.
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

    The bundle file that is built by `webpack` supports only JavaScript dependencies. Action invocations might fail if the bundle depends on binary dependencies because this is not included with the file `bundle.js`.
    {: tip}

## Creating Python actions
{: #creating-python-actions}

The following sections guide you through creating and invoking a single Python action and adding parameters to that action.

### Creating and invoking a Python action
{: #openwhisk_actions_python_invoke}

An action is simply a top-level Python function. To create a Python action:

1. Save the following code in a file called `hello.py`.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
        return {"greeting": greeting}
    ```
    {: codeblock}

  * Python actions always consume a dictionary and produce a dictionary.
  * The entry method for the action is `main` by default but can be specified explicitly to create the action with the `wsk` CLI by using the `--main` flag.

2. Create a `helloPython` action.
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Example output:
    ```
    ok: created action helloPython
    ```
    {: screen}

    The CLI automatically infers the type of the action by using the source file extension.
    For `.py` source files, the action runs by using a Python 2 runtime.

    You can also create an action that runs with Python 3.x, for Python 3.6 use the parameter `--kind python:3.6` (previously named `python-jessie:3`), for Python 3.7 use the parameter `--kind python:3.7`, both contain additional packages for IBM Cloud Services like {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}}, and {{site.data.keyword.ibmwatson_notm}}. For more information about the packages  that are included in these Python 3 runtimes, see the Python runtime [reference](./openwhisk_reference.html#openwhisk_ref_python_environments).

3. Invoke the action.
    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Example output:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Packaging Python actions in zip files
{: #openwhisk_actions_python_zip}

You can package a Python action and dependent modules in a zip file. For example, to create an action with a helper module called `helper.py`:

1. Create an archive containing your source files. **Note**: The source file that contains the entry point must be named `__main__.py`.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Create the action.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### Packaging Python actions with a virtual environment in zip files
{: #openwhisk_actions_python_virtualenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. The virtual environment allows you to link additional packages that can be installed by using [`pip` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://packaging.python.org/installing/), for example.

To install dependencies, package them in a virtual environment, and create a compatible OpenWhisk action:

1. Create a [requirements.txt ![External link icon](../icons/launch-glyph.svg "External link icon")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) file that contains the `pip` modules and versions to install.

2. Install the dependencies and create a virtual environment. The virtual environment directory must be named `virtualenv`. To ensure compatibility with the OpenWhisk runtime container, package installations inside a virtual environment must use the image that corresponds to the kind.
    * For kind `python:2` use the docker image `openwhisk/python2action`.
    * For kind `python:3.6` use the docker image `ibmfunctions/action-python-v3.6`.
    * For kind `python:3.7` use the docker image `ibmfunctions/action-python-v3.7`.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Package the `virtualenv` directory and any additional Python files. The source file that contains the entry point must be named `__main__.py`.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Create the action `helloPython`.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Add only modules that are not part of the selected runtime environment to the `requirements.txt`. This helps to keep the `virtualenv` to a minimum size.
{: tip}

## Creating Go actions
{: #creating-go-actions}

The following sections guide you through creating and invoking a single Go action and adding parameters to that action. You can execute Go actions by using Go 1.11. To use this runtime, specify the `ibmcloud fn` CLI parameter
`--kind go:1.11` when creating or updating an action.
{: shortdesc}

### Creating and invoking Go actions
{: #invoking-go-actions}

A Go action is simply a public function from the `main` package. Use a single file for quick testing or development purposes. For production apps, [pre-compile your Go actions into an executable](#packaging-go-actions) for better performance or multiple source file support, including vendor libraries.
{: shortdesc}

To create a Go action:

1. Save the following code in a file named `hello.go`. In the example, the function is named `Main`, but you can change the name by using the `--main` flag. If you rename the function, the name must begin with a capital letter, and cannot be the the term `main`, which is the name of the Go package.
    ```go
    package main

    import "fmt"

    // Main is the function implementing the action
    func Main(params map[string]interface{}) map[string]interface{} {
        // parse the input JSON
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")
        // return the output JSON
        return msg
    }
    ```
    {: codeblock}
    </br>
    The expected signature for a `Main` function is:
    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. Create or update an action called `helloGo`.
    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. Invoke the action.
    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    Example output:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

### Packaging an action as a Go executable
{: #packaging-go-actions}

Although you can create a binary on any Go platform by cross-compiling with `GOOS=Linux` and `GOARCH=amd64`, use the pre-compilation feature that is embedded in the runtime container image. You can package [multiple source files](#multiple-packages-go-actions) or [vendor libraries](#vendor-libs-go-actions).
{: shortdesc}

#### Working with multiple package source files
{: #multiple-packages-go-actions}

To use multiple package source files, use a top level `src` directory, place the source files that belong to the main package at the root of `src` or inside a `main` directory and create directories for other packages. For example, the `hello` package becomes the `src/hello` directory.
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
    │   └── hello.go
    └── main
        └── main.go
```
{: screen}

With this layout, you can import subpackages (`import "hello"`) as shown in the following example.

To compile locally, set your `GOPATH` to parent of the `src` directory. If you use VSCode, you need to enable the `go.inferGopath` setting to `true`.
{: note}

Example of `main/main.go`:
```go
package main

import (
	"fmt"
	"hello"
)

// Main forwading to Hello
func Main(args map[string]interface{}) map[string]interface{} {
	fmt.Println("This is main.Main")
	greetings := "World"
	name, ok := args["name"].(string)
	if ok {
		greetings = name
	}
	return hello.Hello(greetings)
}
```
{: codeblock}

Example of `hello/hello.go`:
```go
package hello

import "fmt"

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	fmt.Println("This is hello.Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello " + name
	return res
}
```
{: codeblock}

To compile by using the runtime environment, zip the content of the `src` directory. Do **not** include the top level project directory `go-action-project/`. To create the zip archive `hello-src.zip`:

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

To compile and package the Go executable as `exec` in the root of the zip build the `hello-bin.zip` archive by running the following command. This assumes you have Docker CLI installed in your workstation and `docker` in your `PATH`.

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

In this example, the main function is `-compile main`. To use a different function as main, change the value for `-compile`.
The main function is selected at compilation time. When you pre-compile, `ibmcloud fn action [update | create]` ignores the `--main`.

The container gets the contents of the source zip in `stdin`, compiles the content, and creates a new zip archive with the executabled `exec` in the root. The zip archive content streams out to `stdout` which gets redirected to the `hello-bin.zip` archive to be deployed as the Go Action.

Now you can update your action for production by using the CLI and new zip archive `hello-bin.zip`.
```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```

#### Working with vendor libraries
{: #vendor-libs-go-actions}

You can include dependencies by populating a `vendor` directory inside the source `zip` when you compile the Go Action. The `vendor` directory does not work at the top level. You need to place the `vendor` directory within `src/` and inside a package directory.
{: shortdesc}


Continuing with the previous example, use the log package `logrus` in `hello.go`.
```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}
</br>
In this example the `vendor` directory is located in `src/hello/vendor`. You can add third-party libraries that are used by the `hello` package. You can use multiple tools such as [dep ![External link icon](../icons/launch-glyph.svg "External link icon")](https://golang.github.io/dep/docs/installation.html) to populate and manage dependencies.

To use `dep`, create a file `src/main/Gopkg.toml` describing the version and location of the libraries.
```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

To populate the `vendor` directory, run `dep ensure`.

To make this an easier process to automate, you can use a deployment tool or script such as a [Makefile](#makefile).

### Using a Makefile for Go actions
{: #makefile}

Continuing with the previous example, your project directory is as follows.
```bash
go-action-hello/
├── Makefile
└── src
    ├── hello
    │   ├── Gopkg.toml
    │   ├── hello.go
    │   └── vendor/
    └── main
        └── main.go
```
{: screen}

Create the file `Makefile` to automate the deployment process.
```Makefile
GO_COMPILER?=openwhisk/actionloop-golang-v1.11
CLI?=ibmcloud fn
MAIN=main
APP=hello
SRCS=$(MAIN)/$(MAIN).go $(APP)/$(APP).go
VENDORS=$(APP)/vendor
NAME=go-action-$(APP)
BINZIP=$(APP)-bin.zip
SRCZIP=$(APP)-src.zip

deploy: $(BINZIP)
	$(CLI) action update $(NAME) $(BINZIP) --main $(MAIN) --kind go:1.11

$(BINZIP): $(SRCZIP)
	docker run -i $(GO_COMPILER) -compile $(MAIN) <$(SRCZIP) >$(BINZIP)

$(SRCZIP): src/$(VENDORS)
	cd src ; zip ../$(SRCZIP) -r $(SRCS) $(VENDORS)

src/%/vendor:
	cd $(@D) ; DEPPROJECTROOT=$(realpath $(@D)/../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}
</br>
To delete the zip archives and vendor directory run:
```bash
make clean
```
{: pre}

To populate the vendor directory, create the source zip, compile the source code, archive the exec into a zip, and deploy the Go action run:
```bash
make deploy
```
{: pre}

Now that the `go-action-hello` is created, invoke the action.
```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

Example output:
```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}

## Creating PHP actions
{: #creating-php-actions}

The following sections guide you through creating and invoking a single PHP action and adding parameters to that action.

### Creating and invoking a PHP action
{: #openwhisk_actions_php_invoke}

An action is simply a top-level PHP function. To create a PHP action:

1. Save the following code in a filed called `hello.php`.
    ```
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

    * PHP actions always consume an associative array and return an associative array.
    * The entry method for the action is `main` by default but can be specified explicitly when you create the action with the `ibmcloud fn` CLI by using the `--main` flag.

2. Create an action called `helloPHP`.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    The CLI automatically infers the type of the action from the source file extension. For `.php` source files, the action runs by using a PHP 7.2 runtime. For more information, see the PHP [reference](./openwhisk_reference.html#openwhisk_ref_php).

3. Invoke the action.
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Example output:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Packaging PHP actions in zip files
{: #openwhisk_actions_php_zip}

You can package a PHP action and other files or dependent packages in a zip file. For example, to package an action with a second file called `helper.php`:

1. Create an archive containing your source files. **Note**: The source file that contains the entry point must be named `index.php`.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}


2. Create the action.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.2 helloPHP.zip
    ```
    {: pre}


## Creating Ruby actions
{: #creating-ruby-actions}

The following sections guide you through creating and invoking a single Ruby action and adding parameters to that action.

Ruby actions are executed using Ruby 2.5. To use this runtime, specify the  `ibmcloud fn` CLI parameter
`--kind ruby:2.5` when creating or updating an action.

### Creating and invoking Ruby actions
{: #openwhisk_actions_ruby_invoke}

An action is simply a top-level Ruby method.

For example, create a file called `hello.rb`.

1. Save the following code in a filed called `hello.rb`.
    ```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

    * Ruby actions always consume a Hash (dictionary-like collection) and return a Hash.
    * The entry method for the action is `main` by default but can be specified explicitly when you create the action with the `ibmcloud fn` CLI by using the `--main` flag.

2. Create an action called `helloRuby`.
    ```
    ibmcloud fn action create helloRuby hello.rb --kind ruby:2.5
    ```
    {: pre}

3. Invoke the action.
    ```
    ibmcloud fn action invoke --result helloRuby --param name World
    ```
    {: pre}

    Example output:
    ```json
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Packaging Ruby actions in zip files
{: #openwhisk_actions_ruby_zip}

You can package a PHP action and other files or dependent packages in a zip file. For example, to package an action with a second file called `helper.rb`:

1. Create an archive containing your source files. **Note**: The source file that contains the entry point must be named `main.rb`.
    ```bash
    zip -r helloRuby.zip main.rb helper.rb
    ```
    {: pre}

2. Create the action.
    ```bash
    ibmcloud fn action create helloRuby --kind ruby:2.5 helloRuby.zip
    ```
    {: pre}


The gems `mechanize` and `jwt` are available in addition to the default and bundled gems.
You can use arbitrary gems so long as you use zipped actions to package all the dependencies.


## Creating Swift actions
{: #creating-swift-actions}

The following sections guide you through creating and invoking a single Swift action and packaging an action in a zip file.

**Note:** Swift actions run in a Linux environment. Swift on Linux is still in development, and {{site.data.keyword.openwhisk_short}} uses the latest available release. These releases might not be stable. The version of Swift that is used with {{site.data.keyword.openwhisk_short}} might be inconsistent with versions of Swift from stable releases of Xcode on MacOS.

For more information about the Swift runtime, see the Swift [reference](./openwhisk_reference.html#swift-actions).
{: tip}

### Creating and invoking an action
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

An action is simply a top-level Swift function. To create a Swift 3 action:

1. Save the following code in a filed called `hello.swift`.
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

2. Create an action called `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    The CLI automatically infers the type of the action from the source file extension. For `.php` source files, the action runs by using a PHP 7.1 runtime. For more information, see the PHP [reference](./openwhisk_reference.html#openwhisk_ref_php).

3. Invoke the action.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Example output:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

In addition to the main function signature, Swift 4 provides two more signatures which take advantage of the [Codable ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/documentation/swift/codable) type. You can learn more about data types that are encodable and decodable for compatibility with external representations such as JSON [here ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

1. Save the following code in a filed called `hello.swift`.
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
    This example takes an input parameter as `Codable Input` with field `name`, and returns a `Codable output` with a field `greetings`.

2. Create an action called `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. Invoke the action.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Example output:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Packaging an action as a Swift executable
{: #packaging-an-action-as-a-swift-executable}

When you create an {{site.data.keyword.openwhisk_short}} Swift action with a Swift source file, the file must be compiled into a binary before the action is run. This delay is known as the cold-start delay. Once the binary is created, subsequent calls to the action are much faster until the container holding your action is purged. To avoid the cold-start delay, you can compile your Swift file into a binary and then upload the binary to {{site.data.keyword.openwhisk_short}} in a zip file.

You can use a script to automate the packaging of the action.

**Prerequisite**: The script used in the following steps assumes that you have a directory called `actions`, with each top level directory representing an action.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. Save the following code in a script file named `compile.sh`.
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

2. To add dependencies, create the `Package.swift` file. The following example adds `example-package-deckofplayingcards` as a dependency. `CCurl`, `Kitura-net` and `SwiftyJSON` are provided in the standard Swift action, so you should include them in your own `Package.swift` only for Swift 3 actions.
    * Swift 3 example syntax:
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

    * Swift 4 example syntax:
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

3. To create a `hello.zip` in the `build`, build the action.
    * Swift 3:
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. Upload the zip to {{site.data.keyword.openwhisk_short}} with the action name `helloSwiftly`.
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. Invoke the action.
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    The time that it took for the action to run is in the `duration` property.

6. You can compare the duration of the pre-compiled action invocation to the duration of a command invocation with a compilation step. Invoke the action from the previous section:
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Error handling in Swift 4
{: #error-handling-swift4}

By using the Codable completion handler, you can pass an error to indicate a failure in your action. [Error handling in Swift ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) resembles exception handling in other languages, with the use of the `try`, `catch`, and `throw` keywords.

The following snippet shows an example of handling an error:
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

The following sections guide you through creating and invoking a single Java action and adding parameters to that action.

In order to compile, test, and archive Java files, you must have [JDK 8 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://openjdk.java.net/install) installed locally.

### Creating and invoking a Java action
{: #openwhisk_actions_java_invoke}

A Java action is a Java program with a method called `main`. `main` must have the following exact signature:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

To create a Java action:

1. Save the following code in a file named `Hello.java`.
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

2. Compile `Hello.java` into a JAR file named `hello.jar`. **Note**: [google-gson ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/google/gson) must exist in your Java CLASSPATH.
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Create an action.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * You must specify the name of the main class by using `--main`. An eligible main class is one that implements a static `main` method. If the class is not in the default package, use the Java fully qualified class name, for example, `--main com.example.MyMain`.
  * You can customize the method name of your Java action. This is done by specifying the fully-qualified method name of your action, for example, `--main com.example.MyMain#methodName`.
  * The CLI automatically infers the type of the action from the source file extension.

4. Invoke the action.
    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Example output:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Including dependencies in Java actions

If you want to use a 3rd party dependency in your Java action, the action jar archive needs to include all libraries.
You can use a build a tool such as [gradle](https://gradle.org) to fetch the libraries from a repository like Maven Central and build a final jar archive that includes your code and all dependencies.

Here is an example using gradle to build a Java action that leverages the library `com.google.zxing` that provides the functionality to genrate a QR code image.
Create a file `build.gradle` specifying the dependencies:
```gradle
apply plugin: 'java'

version = '1.0'

repositories {
    mavenCentral()
}

configurations {
    provided
    compile.extendsFrom provided
}

dependencies {
    provided 'com.google.code.gson:gson:2.6.2'
    compile 'com.google.zxing:core:3.3.0'
    compile 'com.google.zxing:javase:3.3.0'
}

jar {
    dependsOn configurations.runtime

    from {
        (configurations.runtime - configurations.provided).collect {
            it.isDirectory() ? it : zipTree(it)
        }
    }
}
```
{: codeblock}
Run the command `gradle jar`, this will generate a jar archive in the directory `build/libs/`.
You will use this jar archive to deploy your action as described in the previous section.
For more information read the gradle documentation [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies)

Here is an example of a Java Web Action, that takes `text` as input and generates a QR code image. Create a file `Generate.java` in the directory `java_example/src/main/java/qr`
```java
package qr;

import java.io.*;
import java.util.Base64;

import com.google.gson.JsonObject;

import com.google.zxing.*;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;

public class Generate {
  public static JsonObject main(JsonObject args) throws Exception {
    String property = "text";
    String text = "Hello. Try with a 'text' value next time.";
    if (args.has(property)) {
      text = args.get(property).toString();
    }

    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    OutputStream b64os = Base64.getEncoder().wrap(baos);

    BitMatrix matrix = new MultiFormatWriter().encode(text, BarcodeFormat.QR_CODE, 300, 300);
    MatrixToImageWriter.writeToStream(matrix, "png", b64os);
    b64os.close();

    String output = baos.toString("utf-8");

    JsonObject response = new JsonObject();
    JsonObject headers = new JsonObject();
    headers.addProperty("content-type", "image/png; charset=UTF-8");
    response.add("headers", headers);
    response.addProperty("body", output);
    return response;
  }
}
```
{: codeblock}

Build the Web Action jar by running the following command from the directory `java_example` where the file `build.gradle` is located.
```bash
gradle jar
```
{: pre}

Deploy the web action using the jar `build/libs/java_example-1.0.jar`
```bash
ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
```
{: pre}

Retrieve the public url of the web action endpoint and assigned to an environment variable
```bash
ibmcloud fn action get QRGenerate --url
URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
```
{: pre}

You can open a Web Browser using this `URL` and appending the query parameter `text` with the message to be encoded into the QR image. Or you can use an http client like `curl` to download a QR Image:
```bash
curl -o QRImage.png $URL\?text=https://cloud.ibm.com
```
{: pre}




## Creating Docker actions
{: #creating-docker-actions}

With {{site.data.keyword.openwhisk_short}} Docker actions, you can write your actions in any language.
{: shortdesc}

Your code is compiled into an executable binary and embedded into a Docker image. The binary program interacts with the system by taking input from `stdin` and replying through `stdout`.   You can find more information about creating Docker actions in the [References](./openwhisk_reference.html#openwhisk_ref_docker) section.

You can use actions to call images from public registries only, such as an image that is publicly available on Docker Hub. Private registries are not supported.
{: tip}

**Before you begin:**

You must have a Docker Hub account. You can set up a free Docker ID and account on [Docker Hub ![External link icon](../icons/launch-glyph.svg "External link icon")](https://hub.docker.com).

**To set up a Docker action:**

1. Download and install the Docker skeleton. The skeleton is a Docker container template where you can inject your code in the form of custom binaries.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Set up your custom binary in the blackbox skeleton. The skeleton includes a C program that you can use. Part of the `example.c` file is compiled as part of the Docker image build process, so you do not need C compiled on your machine.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Example output:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Optional: Add additional code and dependencies to the Docker image by modifying the `Dockerfile` to build your executable. Note the following requirements and limits:
  * The binary must be located inside the container at `/action/exec`.
  * The executable receives a single argument from the command line. This argument is a string serialization of the JSON object that represents the arguments to the action.
  * The program may log to `stdout` or `stderr`.
  * By convention, the last line of output must be a stringified JSON object which represents the result of the action.

4. Build the Docker image and upload it using a supplied script.
    1. Log in to Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Go to the `dockerSkeleton` directory.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Run the script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Use your Docker container to create an action.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Invoke the action.
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

7. To update the Docker action, upload the latest image to Docker Hub. This allows the system to pull your new Docker image the next time it runs the code for your action.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. If there is a warm container that uses a previous version of your Docker image, any new invocations continue to use that image. Update the action so that new invocations begin using the new image.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Optional: You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. This argument makes it easier to create and deploy executables that run inside the standard Docker action SDK.
    1. The above steps create a binary executable inside the container located at `/action/exec`. Copy the `/action/exec` file to your local file system and zip it into `exec.zip`.
    2. Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Creating action sequences
{: #openwhisk_create_action_sequence}

You can create an action that chains together a sequence of actions. The result of one action is passed as an argument to the next action.
{: shortdesc}

Several utility actions are provided in the `/whisk.system/utils` package that you can use to create your first sequence.

1. List the actions in the `/whisk.system/utils` package.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Example output:
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

2. Using the `split` and `sort` actions, create an action sequence so that the result of `split` is passed as an argument to `sort`. This action sequence converts some lines of text to an array, and sorts the lines.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Invoke the action.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    In the output, the split lines are sorted alphabetically.
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

**Note**:
* Parameters that are passed between actions in the sequence are explicit, except for default parameters. Therefore, parameters that are passed to the action sequence are only available to the first action in the sequence. The result of the first action in the sequence becomes the input JSON object to the second action in the sequence, and so on. This object does not include any of the parameters that are originally passed to the sequence unless the first action explicitly includes them in its result. Input parameters to an action are merged with the action's default parameters, with the former taking precedence and overriding any matching default parameters. For more information about invoking action sequences with multiple named parameters, see [Setting default parameters on an action](./parameters.html#default-params-action).
* A sequence does not have an overall timeout separate from the timeouts of each action within the sequence. Because a sequence is a pipeline of operations, a failure in one action breaks the pipeline. If one action times out, the entire sequence is exited with that failure.

## Managing large actions
{: #large-app-support}

The maximum code size for an action is 48 MB. Applications that contain many third-party modules, native libraries, or external tools might hit this limit. If you create a .zip or .jar package action that is larger than 48 MB, you must extend the runtime image with dependencies, and then use a single source file or smaller archive than 48 MB.

For example, by building a custom Docker runtime that includes necessary shared libraries, dependencies are not required to be present in the archive file. Private source files can still be bundled in the archive and injected at runtime.

### Reducing the size of actions
{: #large-app-reduce}

To reduce the code size of a Python app:

1. Place the library `opencv-python` into `opencv`.
2. Install the opencv binary into the OS image.
3. Augment the image with more Python libraries by running `pip install requirements.txt`.
4. Use `action.py` with the new image.

To reduce the code size of a Node.js app:

1. Install `opencv`.
   ```
   npm install opencv
   ```
   {: pre}

2. Install `package.json`.
   ```
   npm install package.json
   ```
   {: pre}

3. Use `action.js` with the new image.

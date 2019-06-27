---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-27"

keywords: actions, serverless, javascript, node, node.js, functions

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
{:external: target="_blank" .external}


# Preparing apps for actions
{: #prep}

Whether you bring an app with you, or you write a script specifically to respond to an event, your code must meet certain requirements before you create an action from it.
{: shortdesc}

Each programming language has specific requirements to run, but most have the following general requirements:
- The expected name for the entry point into the code is `main` by default. If your entry point is not `main`, a custom name can be specified when the action is created, so take note of that name.
- Input parameters into your app and output results from your app must be formatted into a specific structure that can be passed between entities. The structure depends on your code language. For example, with Python apps, the input parameters must be a dictionary and the result of your app must be structured as a dictionary. Because you can also pass parameters in a structured object to your action. In JSON, for example, you might structure your code to expect an input parameter with JSON values from certain fields, like `name` and `place`.

    **JSON input example**
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    **JavaScript example**
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
    }
    ```
    {: codeblock}
- If your app contains multiple files, they must be combined into one single file to be used in an action. You can either rewrite your code into one file or you can package the files and dependencies into a single archive file. If your runtime is not supported, you can package your app in a Docker image.
- Dependencies must also be packaged with your app. The available runtimes come with some pre-installed packages and extensions. [Review the reference information for your runtime](/docs/openwhisk?topic=cloud-functions-runtimes) to see whether a dependency of your app is already included with the runtime. If your dependency is included, you are not required to package it with your app.

    Code compilation is not required, but if possible for your runtime, compiling your code in advance can improve performance.
    {: tip}

## Preparing apps in Docker images
{: #prep_docker}

With {{site.data.keyword.openwhisk_short}}, you can write your app in any language and package it as a Docker image.
{: shortdesc}

You can use images from public registries only, such as an image that is publicly available on Docker Hub. Private registries are not supported.
{: important}

### Packaging code in Docker images
{: #prep_docker_pkg}

Your code is compiled into an executable and embedded into a Docker image. The executable interacts with the system by taking input from `stdin` and replying through `stdout`.
{: shortdesc}

**Before you begin**
- You must have a Docker Hub account. You can set up a free Docker ID and account on [Docker Hub](https://hub.docker.com){: external}.
- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){:external}.
- [Review the requirements for the Docker runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

To package your app, complete the following steps.

To package your code as a Docker image, run the following command.
1. Download and install the Docker skeleton. The skeleton is a Docker container template where you can inject your code in the form of custom binaries.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Set up your code in the black box skeleton. The skeleton includes a C program that you can use. Part of the `example.c` file is compiled as part of the Docker image build process, so you do not need C compiled on your machine.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  **Example output**
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. (Optional) Add more code and dependencies to the Docker image by modifying the `Dockerfile` to build your executable. Note the following requirements:
  * Your code must be located inside the container at `/action/exec`.
  * The executable receives a single argument from the command line. This argument is a string serialization of the JSON object that represents the arguments to the action.
  * The program can log to `stdout` or `stderr`.
  * By convention, the last line of output must be a <ph class="ignoreSpelling">stringified</ph> JSON object, which represents the result of the action.
  For more information about constructing Dockerfiles, see the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/){: external}.

4. Build the Docker image and upload it using a supplied script.
    1. Log in to Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Navigate to the `dockerSkeleton` directory.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Run the script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}




## Preparing JavaScript apps
{: #prep_js}

Before you create an action, get your JavaScript code ready. Confirm that your code is structured properly, then decide whether it needs packaged.
{: shortdesc}

### Structuring JavaScript code
{: #prep_js_struct}

- The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
- The input parameters are passed as a JSON object.
- The result of a successful activation is also a JSON object but is returned differently depending on whether the action is [synchronous](#prep_js_sync) or [asynchronous](#prep_js_async).



**Example**
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

**Example with multiple functions**

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
  }
  ```
  {: codeblock}


### Structuring JavaScript code with synchronous behavior
{: #prep_js_sync}

The JavaScript activation is synchronous when the main function either exits without executing a `return` statement or exits by executing a `return` statement that returns any value except a promise.
{: shortdesc}

**Example of synchronous code.**

```javascript
// each path results in a synchronous activation
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




### Structuring JavaScript code with asynchronous behavior
{: #prep_js_async}

JavaScript functions can continue execution in a callback function even after a return. The JavaScript activation is asynchronous if the main function exits by returning a promise. In this case, the system assumes that the action is still running until the promise is fulfilled or rejected. JavaScript functions that run asynchronously can return the activation result after the `main` function returns by returning a promise in your action.
{: shortdesc}

Start by instantiating a new promise object and passing a callback function. The callback takes two arguments, resolve and reject, which are both functions. All your asynchronous code goes inside that callback. The action handler can have any name as long as it conforms to the conventional signature of accepting an object and returning an object (or a `Promise` of an object).

In the following example, you can see how to fulfill a promise by calling the resolve function.
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

This example shows how to reject a promise by calling the reject function.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 2000);
     })
}
```
{: codeblock}

In the examples, the following details are executed.
* The `main` function returns a promise. The promise indicates that the activation isn't completed yet but is expected to in the future.
* The `setTimeout()` JavaScript function waits for 2 seconds before calling the promise's callback function, which represents the asynchronous code.
* The promise's callback accepts the arguments `resolve` and `reject`, which are both functions.
  * The call to `resolve()` fulfills the promise and indicates that the activation completes normally.
  * A call to `reject()` can be used to reject the promise and signal that the activation completes abnormally.


### Structuring JavaScript code with synchronous and asynchronous behavior
{: #prep_js_both}

An action can be synchronous on some inputs and asynchronous on others, as shown in the following example.
{: shortdesc}

```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
        return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
          }, 2000);
        })
     }  else {
        // synchronous activation
        return {done: true};
     }
}
```
{: codeblock}





### Example calling an external API with JavaScript
{: #prep_js_api}

The following example invokes the external API for the NASA Astronomy Picture of the Day (APOD) service, which provides a unique image of our universe every day.
{: shortdesc}


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

A call is made to the NASA APOD API, and fields are extracted from the JSON result.

Next, [create](/docs/openwhisk?topic=cloud-functions-actions), and [invoke the action](/docs/openwhisk?topic=cloud-functions-test) to test it. The following example object is returned:

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






### Packaging JavaScript code with the `webpack` module
{: #prep_js_pkg}

You can package an app by using a JavaScript module bundler such as `[webpack ](https://webpack.js.org/concepts/){: external}`. When `webpack` processes your code, it recursively builds a dependency graph that includes every module that your action needs.
{: shortdesc}

Before you begin, [review the packages that are included with the JavaScript runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

1. Create a `package.json` file. Add `webpack` as a development dependency.

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

2. Save the following `webpack` configuration code in a file named `webpack.config.js`.

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

3. Prepare your app code. In this example, which you can save as a file that is named `index.js`, the variable `global.main` is set as the main function of the app.

    **Example**
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

5. Build the `webpack` bundle.

    ```
    npm run build
    ```
    {: pre}

    The file `dist/bundle.js` is created and deploys as the action source code.

6. Create the action using the `npm` script or the CLI.

    * Run the following the `npm` script.

        ```
        npm run deploy
        ```
        {: pre}

    * Run the following CLI command.

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    The bundle file that is built by `webpack` supports only JavaScript dependencies. Action invocations might fail if the bundle has other dependencies because these dependencies are not included with the file `bundle.js`.
    {: tip}



### Packaging JavaScript code as NPM files
{: #prep_js_npm}

As an alternative to writing all your action code in a single JavaScript source file, you can package your code as a `npm` package in a .zip file.
{: shortdesc}

Before you begin, [review the packages that are included with the JavaScript runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

1. In the root directory, create a `package.json` file. 

**Example**

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

2. Install all dependencies locally.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    While most `npm` packages install JavaScript sources on `npm install`, some also install and compile binary artifacts. The archive file upload supports only JavaScript dependencies. If the archive includes binary dependencies, action invocations might not succeed.
    {: note}

3. Create a `.zip` archive that contains all files, including all dependencies.

    ```
    zip -r action.zip *
    ```
    {: pre}

    **Windows users** Using the Windows Explorer action for creating the .zip file results in an incorrect file structure. {{site.data.keyword.openwhisk_short}} .zip actions must have `package.json` at the root of the archive, but Windows Explorer places it inside a nested folder. Use the `zip` command instead.
    {: tip}





## Preparing Go apps
{: #prep_go}

Use a single file for quick testing or development purposes. For production apps, pre-compile your Go actions into an executable for better performance or multiple source file support, including vendor libraries.
{: shortdesc}

Although you can create a compressed file on any Go platform by cross-compiling with `GOOS=Linux` and `GOARCH=amd64`, use the pre-compilation feature that is embedded in the runtime container image. You can package [multiple source files](#prep_go_multi) or [vendor libraries](#prep_go_vendor).
{: tip}


### Structuring Go code
{: #prep_go_struct}

- The expected name for the entry point package is `main`. If the package in your code is not `main`, take note of the name to specify it when the action is created.
- The package must be public.

**Example**
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

### Packaging multiple Go source files
{: #prep_go_multi}

1. Create a top level `src` directory. Either place the source files that belong to the main package in the root of `src` or inside a `main` directory and create subdirectories for other packages. For example, the `hello` package becomes the `src/hello` directory.
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. Import subpackages. Example of `main/main.go` importing the hello subpackage.

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

  Example of `hello/hello.go`.

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

3. Compile the code. Create a .zip archive of the `src` directory. Do not include the top level project directory `go-action-project/`.

  ```bash
  cd src
  zip -r ../hello-src.zip *
  cd ..
  ```
  {: pre}

  You can compile locally by setting your `GOPATH` to the parent of the `src` directory. If you use VS Code, you must change the `go.inferGopath` setting to `true`.
  {: note}

4. Compile and package the Go executable as `exec` in the root of the .zip archive. Build the `hello-bin.zip` archive by running the following command. You must have Docker CLI installed in your workstation and `docker` in your `PATH`.

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  In this example, the main function is `-compile main`. To use a different function as main, change the value for `-compile`. The main function is selected at compilation time. When you pre-compile, `ibmcloud fn action [update | create]` ignores the `--main`.

  The container gets the contents of the source .zip in `stdin`, compiles the content, and creates a new .zip archive with the executable `exec` in the root. The .zip archive content streams out to `stdout` which gets redirected to the `hello-bin.zip` archive to be deployed as the Go Action.




### Packaging Go code with vendor libraries
{: #prep_go_vendor}

You can include dependencies by populating a `vendor` directory inside the source `zip` archive when you compile the Go files. The `vendor` directory does not work at the top level. You must place the `vendor` directory within `src/` and inside a package directory.
{: shortdesc}

Example log package `logrus` in a `hello.go` app.

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
In this example, the `vendor` directory is located in `src/hello/vendor`. You can add third-party libraries that are used by the `hello` package. 

You can use multiple tools such as [<code>dep</code> ](https://golang.github.io/dep/docs/installation.html){: external} to populate and manage dependencies.

You can use `dep` by creating a file `src/main/Gopkg.toml` describing the version and location of the libraries.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Populate the `vendor` directory, run `dep ensure`.




## Preparing Swift apps
{: #prep_swift}

Swift files must be compiled before an action is run. This delay is known as the cold-start delay. To avoid the cold-start delay, you can compile your Swift file and then upload it to {{site.data.keyword.openwhisk_short}} in a .zip file. The Docker runtime includes a compiler to help users compile and package Swift 4.2 actions. Subsequent calls to the action are much faster until the container with your action is purged.

Swift actions run in a Linux environment. Swift on Linux is still in development, and {{site.data.keyword.openwhisk_short}} uses the latest available release. These releases might not be stable. The version of Swift that is used with {{site.data.keyword.openwhisk_short}} might be inconsistent with versions of Swift from stable releases of Xcode on macOS.
{: important}


### Structuring Swift code
{: #prep_swift_struc}

The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.

In addition to the main function signature, Swift 4 provides two more signatures that take advantage of the [<code>Codable</code> ](https://developer.apple.com/documentation/swift/codable){: external} type. You can learn more about data types that are [encodable and decodable for compatibility with external representations such as JSON ](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external}.

**Example**
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


#### Handling errors in Swift
{: #prep_swift_error}

By using the `Codable` completion handler, you can pass an error to indicate a failure in your action. [Error handling in Swift ](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external} resembles exception handling in other languages, with the use of the `try`, `catch`, and `throw` keywords.
{: shortdesc}

The following snippet shows an example of handling an error.

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


### Packaging a Swift 4.2 file
{: #prep_swift42_single}

Compile a single source file that doesn't depend on external libraries. Use the flag `-compile` with the name of the main method.

**Before you begin**
- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Review the packages that are included with the Swift runtime](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Package your app.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

The Docker container reads the content of the file from `stdin`, and writes a .zip archive with the compiled swift executable to `stdout`.



### Packaging Swift 4.2 multi-file projects and dependencies
{: #prep_swift42_multi}

**Before you begin**
- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Review the packages that are included with the Swift runtime](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Package your app.

1. To compile multiple files and include external dependencies, create the following directory structure.

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  The directory `Sources/` contains a file that is named `main.swift`.

  The `Package.swift` must start with a comment that specifies version `4.2` for the Swift tooling:

  ```swift
  // swift-tools-version:4.2
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
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
      targets: [
      .target(
          name: "Action",
          dependencies: ["SwiftyRequest"],
          path: "."
      )
      ]
  )
  ```
  {: codeblock}

2. Create a .zip archive with the contents of the directory.

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. Pass the .zip archive to the Docker container over `stdin`. The `stdout` is a new .zip archive with the compiled executable. The Docker container reads the content of the .zip archive from `stdin` and writes a new .zip archive with the compiled Swift executable to `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  In a Linux based system, you can combine the `zip` and `docker run` steps in a single command:

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Packaging Python apps
{: #prep_python}


### Structuring Python code
{: #prep_python_struct}

- Python apps must consume a dictionary and produce a dictionary.
- The expected name for the entry point method is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
{: shortdesc}

**Example**
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
	return {"greeting": greeting}
```

### Packaging Python code
{: #prep_python_pkg}

Package Python code and dependent modules in a .zip file. In this example, the source file that contains the entry point is `__main__.py` and the helper modules are in a file called `helper.py`.

Before you begin, [review the packages that are included with the Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

To package your app, run the following command.

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Packaging Python code with a virtual environment in .zip files
{: #prep_python_virtenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. By using the virtual environment, you can link more packages that can be installed by using [`pip` ](https://packaging.python.org/tutorials/installing-packages/){: external}.

Before you begin, [review the packages that are included with the Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Package your app by completing the following steps.

1. Create a [requirements.txt ](https://pip.pypa.io/en/latest/user_guide/#requirements-files){: external} file that contains the `pip` modules and versions to install.

  To keep the `virtualenv` to a minimum size, add only the modules that are not part of the selected runtime environment to the `requirements.txt`. For more information about the packages that are included in Python runtimes, see the Python [runtime reference](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments).
  {: tip}

2. Pull one of the following images for your runtime. To ensure compatibility with the runtime container, packaged installations inside a virtual environment must use the image that corresponds to the runtime that is specified.
    * For `python:3.7`, use the Docker image `ibmfunctions/action-python-v3.7`.
    * For `python:3.6`, use the Docker image `ibmfunctions/action-python-v3.6`.
    * For `python:2`, use the Docker image `openwhisk/python2action`.

   **Example**
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. Install the dependencies and create a virtual environment. The virtual environment directory must be named `virtualenv`.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Package the `virtualenv` directory and any additional Python files. The source file that contains the entry point must be named `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}


## Preparing Ruby apps
{: #prep_ruby}

Before you create an action, get your Ruby code ready.

### Structuring Ruby code
{: #prep_ruby_struct}

* Ruby actions always consume a Hash (dictionary-like collection) and return a Hash.
* The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.


**Example**

```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
```
{: codeblock}

### Packaging Ruby code
{: #prep_ruby_pkg}

You can package a Ruby app and dependent packages in a .zip file. For example, you can package an action with a second file called `helper.rb`.

Create an archive that contains your source files. The source file that contains the entry point must be named `main.rb`.

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

The gems `mechanize` and `jwt` are available in addition to the default and bundled gems. You can use arbitrary gems so long as you use zipped actions to package all the dependencies.



## Preparing PHP apps
{: #prep_php}

Before you create an action, get your PHP code ready.

### Structuring PHP code
{: #prep_php_struct}

- PHP actions always consume an associative array and return an associative array.
- The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.

**Example**
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### Packaging PHP code
{: #prep_php_pkg}

You can package PHP files or dependent packages in a .zip file.

Before you begin, [review the packages that are included with the PHP runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

To package your app, run the following command.

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

**Example**
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

## Preparing Java apps
{: #prep_java}

Before you create an action, get your Java code ready.

### Structuring Java code
{: #prep_java_struct}

A Java action is a Java program with a method called `main`. `main` must have the following signature.

**Example**
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* You must specify the name of the main class by using `--main`. An eligible main class is one that implements a static `main` method. If the class is not in the default package, use the Java fully qualified class name, for example, `--main com.example.MyMain`.
* You can customize the method name of your Java action by specifying the fully qualified method name of your action, for example, `--main com.example.MyMain#methodName`.

### Packaging Java code
{: #prep_java_pkg}


**Before you begin**
You must have [JDK 8](http://openjdk.java.net/install/){: external} installed locally. This example uses the [`google-gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/){: external}.

If you are working with a JDK version other than JDK 8, you must specify `--release 8` when you compile your code with the `javac` command.
{: note}

To create a Java action, complete the following steps.

1. Save the following code in a file named `Hello.java`.

  ```java
  import com.google.gson.JsonObject;
  public class Hello {
      public static JsonObject main(JsonObject args) {
          String name = "stranger";
          if (args.has("name"))
              name = args.getAsJsonPrimitive("name").getAsString();
          JsonObject response = new JsonObject();
          response.addProperty("greeting", "Hello, " + name + "!");
          return response;
      }
  }
  ```
  {: codeblock}

2. Download the [`gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/).

3. Add the `gson-2.8.5.jar` to your `ClASSPATH`. This example uses `gson-2.8.5.jar` which is saved in a `test` folder in the `Desktop` directory.
  
  ```
  export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.8.5.jar
  ```
  {: pre}

4. Add the `bin` folder of your JDK to your `CLASSPATH`. This example uses `openjdk-8`.
  
  ```
  export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: pre}

5. Verify the JDK `bin` folder and `gson-2.8.5.jar` are in your `CLASSPATH`.
  ```
  echo $CLASSPATH
  ```
  {: pre}

  **Example output**
  ```
  /Desktop/test/gson-2.8.5.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: screen}

6. Navigate to the folder where your `Hello.java` file is stored. In this example, the `Hello.java` file is saved to the `Desktop/test` folder.
  ```
  cd Desktop/test
  ```
  {: pre}

7. Compile your `Hello.java` file into a class file.
  ```
  javac Hello.java
  ```
  {: pre}

8. Compress the class file into a .jar file named `hello.jar`.

  ```
  jar cvf hello.jar Hello.class
  ```
  {: pre}

**Next steps**
You can create an action with your `hello.jar`. Because the class file you created does not use the default name `main`, you must set the `--main` flag to `Hello` when you create your action. The `--main` flag must match your Java `class`. For more information, see [Creating actions](/docs/openwhisk?topic=cloud-functions-actions).
 
When you update your Java code, you must repeat these steps to recompile your code into a new `.jar` file.
{: important}

### Packaging Java code with Gradle
{: #prep_java_gradle}

Instead of compiling from the command line, you can use a build a tool such as [Gradle](https://gradle.org){: external} to fetch the libraries from a repository like Maven Central. You can use Gradle to fetch and build a final .jar archive that includes your code and all dependencies.

Here is an example using Gradle to build a Java action that leverages the library `com.google.zxing` that provides the functionality to generate a QR code image.

1. Create a file that is named `build.gradle` and specify the dependencies.

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

2. Run the command `gradle jar`, which generates a .jar archive in the directory `build/libs/`.

  For more information, read the Gradle documentation [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external}.


## Preparing .NET Core apps
{: #prep_dotnet}

Before you create an action, get your .NET Core code ready.

### Structuring .NET Core code
{: #prep_dotnet_struct}

A .NET Core action is a .NET Core class library with a method that is expected to be named `Main`. If the method in your code is not `Main`, take note of the name to specify it when the action is created in the format: `--main {Assembly}::{Class Full Name}::{Method}`

**Example**
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### Packaging .NET Core code
{: #prep_dotnet_pkg}

**Before you begin**
To compile, test and archive .NET Core projects, you must:
- Install the [.NET Core SDK](https://dotnet.microsoft.com/download){: external} locally.
- Set the `DOTNET_HOME` environment variable to the location where the `dotnet` executable can be found.



To package your code, run the following commands.

  1. Create a C# project called `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. Navigate to the `Apache.OpenWhisk.Example.Dotnet` directory.

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. Install the [<ph class="ignoreSpelling">Newtonsoft.Json NuGet</ph> package](https://www.nuget.org/packages/Newtonsoft.Json/){: external}.

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. Save the following code in a file named `Hello.cs`.

      ```csharp
      using System;
      using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
      {
          public class Hello
          {
              public JObject Main(JObject args)
              {
                  string name = "stranger";
                  if (args.ContainsKey("name")) {
                      name = args["name"].ToString();
                  }
                  JObject message = new JObject();
                  message.Add("greeting", new JValue($"Hello, {name}!"));
                  return (message);
              }
          }
      }
      ```
      {: codeblock}

  5. Compile `Hello.cs` and any other files and output to `out` directory.

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. Navigate to the `out` directory.

      ```bash
      cd out
      ```
      {: pre}

  7. Compress the published files.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}



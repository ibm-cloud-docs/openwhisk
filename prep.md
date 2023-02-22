---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-22"

keywords: actions, serverless, javascript, node, node.js, functions, apps, java, python, go, swift, ruby, .net core, PHP

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


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

    Code compilation is not required, but if possible for your runtime, compiling your code in advance can improve performance.
    {: tip}

## Preparing JavaScript apps
{: #prep_js}

Before you create an action, get your JavaScript code ready. Confirm that your code is structured properly, then decide whether it needs packaged.
{: shortdesc}

### Structuring JavaScript code
{: #prep_js_struct}

- The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
- The input parameters are passed as a JSON object.
- The result of a successful activation is also a JSON object but is returned differently depending on whether the action is [synchronous](#prep_js_sync) or [asynchronous](#prep_js_async).

Example

```javascript
    function main() {
        return {payload: 'Hello world'};
    }
```
{: codeblock}

Example with multiple functions

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

Example of synchronous code

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

JavaScript functions can continue to run in a callback function even after a return. The JavaScript activation is asynchronous if the main function exits by returning a promise. In this case, the system assumes that the action is still running until the promise is fulfilled or rejected. JavaScript functions that run asynchronously can return the activation result after the `main` function returns by returning a promise in your action.
{: shortdesc}

Start by instantiating a new promise object and passing a callback function. The callback takes two arguments, resolve and reject, which are both functions. All your asynchronous code goes inside that callback. The action handler can have any name that conforms to the conventional signature of accepting an object and returning an object (or a `Promise` of an object).

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
* The `setTimeout()` JavaScript function waits for 2 seconds before it calls the promise's callback function, which represents the asynchronous code.
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

### Example: Calling an external API with JavaScript
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

Next, [create](/docs/openwhisk?topic=openwhisk-actions), and [invoke the action](/docs/openwhisk?topic=openwhisk-test) to test it. The following example object is returned.

```sh
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

Before you begin, [review the packages that are included with the JavaScript runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_javascript_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app. The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.
{: shortdesc}

1. Create a `package.json` file. Add `webpack` as a development dependency.

    ```json
    {
    "name": "my-action",
    "main": "dist/bundle.js",
    "scripts": {
        "prebuild": "NODE_ENV=development npm install",
        "build": "webpack --config webpack.config.js ",
        "deploy": "ibmcloud fn fn action update my-action dist/bundle.js --kind nodejs:16",
        "clean": "rm -rf node_modules package-lock.json dist"
    },
    "dependencies": {
        "left-pad": "1.1.3"
    },
    "devDependencies": {
        "webpack": "^5.72.0",
        "webpack-cli": "^4.9.2"
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

    ```bash
    npm install
    ```
    {: pre}

    ```bash
    npm run prebuild
    ```
    {: pre}

    While most `npm` packages install JavaScript sources on `npm install`, some packages also install and compile platform-dependent binary file artifacts. Because this environment is Linux AMD64-based, the `npm install` must be executed on a similar platform. Otherwise the action invocations might not succeed.
    {: note}

5. Build the `webpack` bundle locally.

    ```bash
    npm run build
    ```
    {: pre}

    The file `dist/bundle.js` is created and deploys as the action source code.

    To avoid compatibility issues, you can use the runtime to build the `webpack`. Use the following command in the source directory to run steps 4 and 5 inside the container.
    ```bash
    docker run --rm -it --entrypoint "/bin/bash" -v $PWD:/nodejsAction ibmfunctions/action-nodejs-v16:1.0.2 -c "npm run prebuild && npm run build"
    ```
    {: pre}


6. Create the action by using the `npm` script or the `ibmcloud fn action update` CLI.

    * Run the following `npm` script.

        ```bash
        npm run deploy
        ```
        {: pre}

    * Or run the following IBM Cloud CLI command.

        ```bash
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:12
        ```
        {: pre}

    The bundle file that is built by `webpack` supports only JavaScript dependencies. Action invocations might fail if the bundle has other dependencies because these dependencies are not included with the file `bundle.js`.
    {: tip}

7. You can clean up the generated artifacts `package-lock.json`, `node_modules` and `dist` by using one of the following options.

    ```bash
    npm run clean
    ```
    {: pre}
    
    or
    
    ```bash
    docker run --rm -it --entrypoint "/bin/bash" -v $PWD:/nodejsAction ibmfunctions/action-nodejs-v16:1.0.2 -c "npm run clean"
    ```
    {: pre}

### Packaging JavaScript code as NPM files
{: #prep_js_npm}

As an alternative to writing all your action code in a single JavaScript source file, you can package your code as a `npm` package in a compressed file.
{: shortdesc}

Before you begin, [review the packages that are included with the JavaScript runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_javascript_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app. The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.

1. In the root directory, create the `package.json` and `my-action.js` file.

    **Example**
    `package.json`
    ```json
    {
        "name": "my-action",
        "main": "my-action.js",
        "dependencies" : {
        "left-pad" : "1.1.3"
        }
    }
    ```
    {: codeblock}

    `my-action.js`
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
        const lines = args.lines || [];
        return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

2. Install all dependencies locally.

    ```bash
    npm install <dependency>
    ```
    {: pre}

    While most `npm` packages install JavaScript sources on `npm install`, some packages also install and compile platform-dependent binary file artifacts. Because this environment is Linux AMD64-based, the `npm install` must be executed on a similar platform. Otherwise the action invocations might not succeed.
    {: note}

3. Create an archive that contains all files, including all dependencies.

    ```bash
    zip -r action.zip *
    ```
    {: pre}

    **Windows users** Using the Windows Explorer action for creating the compressed file results in an incorrect file structure. {{site.data.keyword.openwhisk_short}} archive actions must have `package.json` at the root of the archive, but Windows Explorer places it inside a nested folder. Use the **`zip`** command instead.
    {: tip}

### NPM libraries with native dependencies
{: #prep_js_npm_dependencies}

Node.js libraries can depend on native modules that are compiled during installation for the local runtime by using the `npm install` command. {{site.data.keyword.cloud_notm}} runtimes are based on a Linux AMD64 platform, which requires that the native modules are compiled for that platform. If you are not using a Linux AMD64-based operating system, you can use the `nodejs runtime` Docker container to install the dependencies.

Zipped actions for {{site.data.keyword.openwhisk_short}} are limited to `48MB`. If your zipped action exceeds this limit, you must create a custom docker image for your action.
{: note}

1. Run the following command to fetch the Node.js modules, compile the native dependencies and Create the zipped action code, including the `node_modules` directory.

    ```bash
    docker run --rm -it --entrypoint "/bin/bash" -v $PWD:/nodejsAction ibmfunctions/action-nodejs-v16:1.0.2 -c "npm install && zip action.zip -r *"
    ```
    {: pre}
    
2. Create the action by using the {{site.data.keyword.openwhisk_short}} CLI.

    ```bash
    ibmcloud fn action create my-action action.zip --kind nodejs:16
    ```
    {: pre}

### Using ES6 module in your action
{: #prep_es6-module}

If you want to use new ES6 modules in your Actions you will have to create a wrapper function to import the new modules. This wrapper consists of a global variable for your module you want to import and a function which imports the module as a promise.

To load extra more use promise chaining to load further ES6 modules. Add the variable name (`let module_name`) and the import code to add another ES6 module (`.then( () =>   return import('other_module').then( module => module_name = module.<obj_to_load>))`). 
{: note}


1. Create the `my-action.js` file with the following content.
    
    ```javascript
        let uuidv5;

        function main(params) {
            const MY_NAMESPACE = '1b671a64-40d5-491e-99b0-da01ff1f3341';

            console.log( "uuidv5 =  " + uuidv5)

            const triggerID = uuidv5('Hello, World!', MY_NAMESPACE);
            console.log("----> uuid module successfully loaded and trigger calculated  = ", triggerID)

            return { message: 'Hello World with id = ' + triggerID };
        }


        function main_wrapper(params) {

            return import('uuid').then(module => uuidv5 = module.v5)
            //.then( () =>   return import('xxxxx').then( module => global_var_yy = module.<obj_to_load>))
            .then( () => {
                return main( params )
            })
        }
    ```
    {: codeblock}

2. Create the action and set the main action to `main_wrapper`.

    ```bash
    ibmcloud fn action create my-action my-action.js --main main_wrapper
    ```
    {: pre}

    If you are creating a action with the Cloud Functions UI you must rename the `main_wrapper` function to `main` and the `main` function to something else, as the entry function in the UI is always `main`.
    {: note}

    When you create a zipped action, follow the previous guide and specify the entry point with `exports.main = main_wrapper`
    {: note}

3. Invoke your action.

    ```bash
    ibmcloud fn action invoke my-action
    ```
    {: pre}


## How do I package my Python app for deployment in {{site.data.keyword.openwhisk_short}}
{: #how_to_package_python}

### Structuring Python code
{: #prep_python_struct}

Python apps must consume a dictionary and produce a dictionary. The expected name for the entry point method is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
{: shortdesc}

Example

```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
	return {"greeting": greeting}
```
{: codeblock}

### Packaging multiple Python files into an archive
{: #prep_python_multiple_zip}

When to use this method

Your app uses multiple Python files, but does not require any dependencies or packages outside of the packages that are included with the base Python runtime. You can create a compressed file that includes your Python files and deploy the compressed file when you create your action.

Example command

```bash
ibmcloud fn action create <action_name> <compressed_python_files.zip> --kind python:3.9
```
{: pre}

For more information, see [Packaging multiple Python files into a compressed file](#prep_python_pkg).

### Packaging Python code with a local virtual environment in a compressed file
{: #prep_python_local_virt_zip}

When to use this method

If your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments), you can install those dependencies into a `virtualenv` folder and then compress into a compressed file to deploy in {{site.data.keyword.openwhisk_short}}. Your compressed file must be smaller than the maximum `codeSize` as described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions).

Example command

```bash
ibmcloud fn action create <action_name> <compressed_python_virtualenv.zip> --kind python:3.9
```
{: pre}

For more information, see [Packaging Python code with a local virtual environment in a compressed file](#prep_python_local_virtenv).

### Packaging Python code with a Docker virtual environment in a compressed file
{: #prep_python_docker_virt_zip}

When to use this method

If your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments), you can install those dependencies into a `virtualenv` folder by using the Python environment inside the {{site.data.keyword.openwhisk_short}} Python runtime image. You can then compress the folder into a compressed file to deploy in {{site.data.keyword.openwhisk_short}}. Your compressed file must be smaller than the maximum `codeSize` as described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions).

Example command

You can use the compressed file to create an action. Replace `<file_path>` with the file path to your compressed file.

```bash
ibmcloud fn action create <action_name> <compressed_python_virtualenv.zip> --kind python:3.9
```
{: pre}

For more information, see [Packaging Python code with a Docker virtual environment in a compressed file](#prep_python_virtenv).

### Packaging large Python dependencies in a custom Docker image
{: #packaging_large_python_docker}

When to use this method

Your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments). You can specify a base {{site.data.keyword.openwhisk_short}} image in your Dockerfile and also specify the dependencies to install when you build your Docker image. You can then specify your custom Docker image when you deploy your app in {{site.data.keyword.openwhisk_short}}. Note that only public Docker images are supported.

Example command

```bash
ibmcloud fn action create <action_name> <app_code.py> --docker <dockerhub_username>/<repo_name>:<tag_name>
```
{: pre}

If your app code is larger than the maximum `codeSize` described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions), you can combine this method with [Packaging multiple Python files into a compressed file](#prep_python_pkg).
{: tip}

For more information, see [Packaging large Python dependencies in a custom Docker image](#prep_python_docker).

### Packaging your app within a custom Docker image
{: #packaging_docker_skeleton}

When to use this method

Your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments) and your app code, even when it is compressed into a .zip file, is still larger than the maximum `codeSize` as described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions). If so, you can install those dependencies into a custom Docker image and include your app code in the `action/exec` folder of the Docker skeleton. You can then specify your custom Docker image when you deploy your app in {{site.data.keyword.openwhisk_short}}. During deployment, you do not need to specify for your app code. Note that only public Docker images are supported.

Example command

```bash
ibmcloud fn action create <action_name> --docker <dockerhub_username>/<repo_name>:<tag_name>
```
{: pre}

## Packaging Python code
{: #prep_python}

The following sections provide tutorials for how to package your Python app for deployment with {{site.data.keyword.openwhisk_short}}.

Before you begin

Review [How do I package my Python app for deployment in {{site.data.keyword.openwhisk_short}}?](#how_to_package_python).

### Packaging multiple Python files into a compressed file
{: #prep_python_pkg}

Package Python code and dependent modules in a compressed file. In this example, the source file that contains the entry point is `__main__.py` and the helper modules are in a file called `helper.py`.

Before you begin, [review the packages that are included with the Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

1. Create a `test` directory on your desktop.

2. Save the following code as a file called `__main__.py` in your `test` directory.

    ```python
    from helper import helper
    def main(args):
    return helper(args)
    ```
    {: codeblock}

3. Save the following code as a file called `helper.py` in your `test` directory. This code accepts a `name` parameter and returns a greeting. If no `name` is  specified, the `name` that is returned is `stranger`.

    ```python
    def helper(dict):
        if 'name' in dict:
          name = dict['name']
        else:
          name = "stranger"
        greeting = "Hello from helper.py, " + name + "!"
        return {"greeting": greeting}
    ```
    {: codeblock}

4. To package your app as a compressed file, `cd` to your `test` directory and run the following command. In this example, the archive is called `stranger.zip`.

    ```bash
    zip -r stranger.zip __main__.py helper.py
    ```
    {: pre}

5. You can use then use the compressed file to create an action called `hello`. Replace `<file_path>` with the file path to your compressed file.

    ```bash
    ibmcloud fn action create hello <file_path>/test/stranger.zip --kind python:3.9
    ```
    {: pre}

6. Test the action.

    ```bash
    ibmcloud fn action invoke hello --result
    ```
    {: pre}

    **Example output**

    ```sh
    {
    "greeting": "Hello from helper.py, stranger!"
    }
    ```
    {: screen}

7. Test the action again and specify the `name` parameter. Replace the `<your_name>` parameter with your name.

    ```bash
    ibmcloud fn action invoke hello --result --param name <your_name>
    ```
    {: pre}

    **Example output**

    ```sh
    {
    "greeting": "Hello from helper.py, <your_name>!"
    }
    ```
    {: screen}


### Packaging Python code with a local virtual environment in a compressed file
{: #prep_python_local_virtenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. With the virtual environment, you can link additional packages that can be installed by using [`pip`](https://packaging.python.org/en/latest/tutorials/installing-packages/){: external}. 

The setup of the local Python environment has a major impact on the compressed action file that is created. Some configurations (for example, non-default installation paths or mixed Python installations) can make the compressed action file fail during execution.

To minimize these dependencies on your local environment, use the [Packaging Python code with a Docker virtual environment in a compressed file](#prep_python_virtenv) approach. This approach creates the compressed action file, but also leverages the Python environment inside the {{site.data.keyword.openwhisk_short}} Python runtime image itself so that both the generated action compressed file and the later execution environment fully match.
{: note}

Before you begin

- The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.
- [Review the packages that are included with the Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.
- Make sure that the locally installed Python version to create the compressed action file (for example, Python 3.9.x) matches the {{site.data.keyword.openwhisk_short}} kind that is chosen to later create the action (`--kind python:3.9`).
- Install the `virtualenv` Python package.

    ```bash
    pip install virtualenv
    ```
    {: pre}

To package your app:

1. Create a directory that you can use to create your virtual environment. In this example, a `jokes` directory is created on the desktop. After you create the `jokes` directory, `cd` to it.

    ```bash
    cd desktop; mkdir jokes; cd jokes
    ```
    {: pre}

2. From the `jokes` directory, create a virtual environment named `virtualenv`.

    The virtual environment must be named `virtualenv`.
    {: note}

    ```bash
    virtualenv virtualenv
    ```
    {: pre}

    **Example output**

    ```sh
    created virtual environment CPython3.9.10.final.0-64 in 398ms
        creator CPython3Posix(dest=/action/Projects/python/virtualenv, clear=False, no_vcs_ignore=False, global=False)
        seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/root/.local/share/virtualenv)
            added seed packages: pip==21.2.4, setuptools==58.0.4, wheel==0.37.0
        activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
    ```
    {: screen}

3. From your `jokes` directory, activate your `virtualenv` virtual environment.

    ```bash
    source virtualenv/bin/activate
    ```
    {: pre}

4. Ensure the Python version inside the virtual environment matches the version as specified with the `--kind` option when you create the action later on (for example, `python:3.9`). To check the actual version,

    ```bash
    python --version
    ```
    {: pre}

5. Install the `pyjokes` module.

    ```bash
    (virtualenv) $ pip install pyjokes
    ```
    {: codeblock}

    **Example output**

    ```sh
        Collecting pyjokes
            Using cached pyjokes-0.6.0-py2.py3-none-any.whl (26 kB)
        Installing collected packages: pyjokes
        Successfully installed pyjokes-0.6.0
    ```
    {: screen}

6. Stop the `virtualenv` virtual environment.

    ```bash
    (virtualenv) $ deactivate
    ```
    {: codeblock}

7. Copy the following code and save it into a file called `__main__.py` in your `jokes` directory.

    ```python
    import pyjokes

    def joke(params):
        return {"joke": pyjokes.get_joke()}
    ```
    {: codeblock}

8. From your `jokes` directory, create an archive of the `virtualenv` folder and your `__main__.py` file. These files must be in the top level of your `.zip` file.

    ```bash
    zip -r jokes.zip virtualenv __main__.py
    ```
    {: pre}

    **Example output**

    ```sh
    ...
    adding: virtualenv (stored 0%)
    adding: __main__.py (deflated 18%)
    ...
    ```
    {: screen}

9. Create an action called `jokes` that uses your `jokes.zip` file. You must also specify the entry point as `jokes`. You must also specify the `--kind` flag for the runtime.

    ```bash
    ibmcloud fn action create jokes </path/to/file/>jokes.zip --kind python:3.9 --main joke
    ```
    {: pre}

    **Example output**

    ```sh
    ok: created action jokes
    ```
    {: screen}

10. Invoke the action to verify it is working. Include the `--result` flag to return the result in the command line.

    ```bash
    ibmcloud fn action invoke jokes --result
    ```
    {: pre}

    **Example output**

    ```sh
    {
    "joke": "A QA engineer walks into a bar. Runs into a bar. Crawls into a bar. Dances into a bar. Tiptoes into a bar. Rams a bar. Jumps into a bar."
    }
    ```
    {: screen}

You can use this method to extend the functionality of {{site.data.keyword.openwhisk_short}} actions by using other Python packages.
{: tip}

### Packaging Python code with a Docker virtual environment in an archive
{: #prep_python_virtenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. By using the virtual environment, you can link more packages that can be installed by using [`pip`](https://packaging.python.org/en/latest/tutorials/installing-packages/){: external}.

This approach is recommended when you want to add additional required python packages. It ensures that the generated compressed action file is compatible with the Python runtime used for later execution of the action.
{: note}

Before you begin

- [Review the packages that are included with the Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.
- The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.

Package your app by completing the following steps.

1. Create a directory that you can use to create your virtual environment. In this example, a `test` directory is created on the desktop. After you create the `test` directory, `cd` to it.

    ```bash
    cd desktop; mkdir test; cd test
    ```
    {: pre}

2. Create a [requirements.txt ![External link icon](../icons/launch-glyph.svg "External link icon")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) in your file in your `test` directory that contains the `pip` modules and versions to install.

    ```bash
    touch requirements.txt
    ```
    {: pre}

3. Use vim to edit the `requirements.txt` file. Enter the names of the `pip` modules and versions you want to install. In this example, `pyjokes` is the module that is used.

    ```bash
    vim requirements.txt
    ```
    {: pre}

    **Example requirements.txt**

    ```sh
    pyjokes
    ```
    {: codeblock}

4. Press `ESC`, then `:wq` to save and close your `requirements.txt` file.

    To keep the `virtualenv` to a minimum size, add only the modules that are not part of the selected runtime environment to the `requirements.txt`. For more information about the packages that are included in Python runtimes, see the Python [runtime reference](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments).
    {: tip}

    * For `python:3.9`, use the Docker image `ibmfunctions/action-python-v3.9`.
    * For `python:3.7`, use the Docker image `ibmfunctions/action-python-v3.7`.
    * For `python:3.6`, use the Docker image `ibmfunctions/action-python-v3.6`.

    Example

    ```bash
    docker pull ibmfunctions/action-python-v3.9:1.0.0
    ```
    {: pre}

    Example output

    ```sh
    Using default tag: latest
    latest: Pulling from ibmfunctions/action-python-v3.9:1.0.0
    ```
    {: screen}

5. Create a virtual environment and install the additional Python packages.

    The virtual environment directory must be named `virtualenv` to create a `virtualenv` folder in the `test` directory.
    {: note}

    ```bash
    docker run --rm -v "$PWD:/tmp" --entrypoint "/bin/bash" ibmfunctions/action-python-v3.9:1.0.0 -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

    This command instantiates a container (`docker run`) based on the runtime image selected and mounts the current working directory (`$PWD`) as `/tmp` into the container (`-v "$PWD:/tmp"`). Inside the container, it then changes to the `/tmp` directory `cd /tmp`, creates and activates the `virtualenv` (`virtualenv virtualenv && source virtualenv/bin/activate`), and runs the `pip install` to add the selected packages. The container is deleted when the command completes (`--rm`). The directory structure of the created `virtualenv` and finally, the installed packages can be found in the folder `virtualenv` in your current directory.

    **Example output**

    ```sh
    created virtual environment CPython3.9.10.final.0-64 in 3291ms
        creator CPython3Posix(dest=/tmp/virtualenv, clear=False, no_vcs_ignore=False, global=False)
        seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/root/.local/share/virtualenv)
            added seed packages: pip==21.3.1, setuptools==60.2.0, wheel==0.37.1
        activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
    Collecting pyjokes
        Downloading pyjokes-0.6.0-py2.py3-none-any.whl (26 kB)
    Installing collected packages: pyjokes
    Successfully installed pyjokes-0.6.0
    ```
    {: screen}

6. Save the following code as `__main__.py` in your `test` directory. When you create actions with a compressed file, the source file that contains the entry point must be named `__main__.py`.

    ```python
    import pyjokes

    def joke(params):
        return {"joke": pyjokes.get_joke()}
    ```
    {: codeblock}

7. To deploy this code as an action, you must create a compressed file of the `virtualenv` folder and the `__main__.py` file. 

    Sometimes, the resulting compressed file is larger than the maximum `codeSize` as described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions) allowed by {{site.data.keyword.openwhisk_short}}. To reduce the size of the compressed file, select only the dependencies that you need, rather than selecting the entire `virtualenv` folder. The packages that you need can be found in the `site-packages` directory within the `virtualenv` folder. Note that you must also include the `activate_this.py` file from the `bin` directory of your `virtualenv` folder in your compressed file.
    {: note}

    ```bash
    zip -r pyjoke.zip virtualenv __main__.py
    ```
    {: pre}

	
8. Create an action called `pyjoke` by using the `pyjoke.zip` file. Make sure to use the `--kind` corresponding to the runtime image used to create the compressed action file. Otherwise, the action fails to execute during invoke.

    ```bash
    ibmcloud fn action create pyjoke <file_path>/pyjoke.zip --kind python:3.9
    ```
    {: pre}

9. Invoke the action to test that the `pyjoke` module is working.

    ```bash
    ibmcloud fn action invoke pyjoke --result
    ```
    {: pre}

    **Example output**

    ```sh
    {
        "joke": "A QA engineer walks into a bar. Runs into a bar. Crawls into a bar. Dances into a bar. Tiptoes into a bar. Rams a bar. Jumps into a bar."
    }
    ```
    {: screen}

### Packaging large Python dependencies in a custom Docker image
{: #prep_python_docker}

{{site.data.keyword.openwhisk_short}} has a size limit for the app code, see maximum `codeSize` described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions). However, you can install large packages and dependencies into a custom Docker image and deploy it with your app code when you create an action. You can then import the packages at run time.

In this example, install large Python packages such as `matplotlib` and `seaborn` to build a {{site.data.keyword.openwhisk_short}} web action that generates a PNG file of a joint plot with `seaborn`.

Before you begin

- Review the packages that are included with the [Python runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.
- The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.

Only public Docker images are supported.
{: note}

Package the app in a custom Docker image by completing the following steps.

1. Create a directory that you can use to create your Dockerfile. In this example, a `functions` directory is created on the desktop. After you create the `functions` directory, `cd` to it.

    ```bash
    cd desktop; mkdir functions; cd functions
    ```
    {: pre}

2. Create a [Dockerfile ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/reference/builder/) in your `functions` directory.

    ```bash
    touch Dockerfile
    ```
    {: pre}

3. Use vim to edit the `Dockerfile` file. Enter the names of the `pip` modules and versions you want to install. In this example, several additional Python modules are installed.

    ```bash
    vim Dockerfile
    ```
    {: pre}

4. Paste or enter the following text in your `Dockerfile`.

    ```dockerfile
    FROM ibmfunctions/action-python-v3.9:1.0.0

    RUN pip install \
        --upgrade pip \
        matplotlib \
        seaborn \
        pandas \
        statsmodels
    ```
    {: codeblock}

5. Press `ESC`, then `:wq` and `Enter` to save and close your Dockerfile.

6. From your `functions` directory, you can run `docker build` to build a Docker image by using your `Dockerfile`.

    ```bash
    docker build -t <dockerhub_username>/<repo_name>:<tag_name> .
    ```
    {: pre}

    You can also run `docker build` commands with this format:

    ```bash
    docker build . -t <dockerhub_username>/<repo_name>:<tag_name>
    ```
    {: pre}   

7. The image builds and installs the dependencies that you specified in your Dockerfile.

    ```dockerfile
        [+] Building 0.1s (6/6) FINISHED
        => [internal] load build definition from Dockerfile.ml                                                                                                                    0.0s
        => => transferring dockerfile: 40B                                                                                                                                        0.0s
        => [internal] load .dockerignore                                                                                                                                          0.0s
        => => transferring context: 2B                                                                                                                                            0.0s
        => [internal] load metadata for docker.io/ibmfunctions/action-python-v3.9:1.0.0                                                                                           0.0s
        => [1/2] FROM docker.io/ibmfunctions/action-python-v3.9:1.0.0                                                                                                             0.0s
        => CACHED [2/2] RUN pip install     --upgrade pip     matplotlib     seaborn     pandas     statsmodels                                                                   0.0s
        => exporting to image                                                                                                                                                     0.0s
        => => exporting layers                                                                                                                                                    0.0s
        => => writing image sha256:4a0d140e65fc379d8c25d18fce9aedd580203f768f43da011149993cd57565d4                                                                               0.0s
        => => naming to docker.io/docker-username/repo-name:tag
        ...
        ...
    ``` 
    {: screen}

8. Push your image to Docker Hub.

    ```bash
    docker push <dockerhub_username>/<repo_name>:<tag_name>
    ```
    {: pre}

    Be sure to log in to Docker Hub before you attempt to push your image.
    {: tip}

9. Save the following code as `seaborn.py` in your `functions` directory. This code generates a joint plot in [`seaborn`](https://seaborn.pydata.org/) that uses random data. You can then create a web action with {{site.data.keyword.openwhisk_short}} to return the plot to a {{site.data.keyword.openwhisk_short}} endpoint.

    ```python
    # import modules
    import base64
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

    # optional: set seaborn style
    sns.set(style="dark")
    def main(args):
        #generate a jointplot from random data.
        x, y = np.random.randn(2, 300)
        g = (sns.jointplot(x,y, kind="hex", color="#9b59b6")
                .set_axis_labels("x", "y"))
        # save the plot as a .png so that it can be base64 encoded/decoded.
        plt.savefig('output.png')
        # open, read, and encode the image.
        image = base64.b64encode(open("output.png", "rb").read())
        # decode the image into a string.
        data = image.decode('utf-8')
        # return the string as a JSON web request.
        return {
            'body': data,
            'statusCode': 200,
            'isBase64Encoded': 'true',
            'headers': {'Content-Type': 'image/png'}
        }
    ```
    {: codeblock}   

10. Create a web action called `seaborn` by using the custom Docker image that you created that contains the required Python dependencies to run a joint plot.

    ```bash
    ibmcloud fn action create seaborn --docker <dockerhub_username>/<repo_name>:<tag_name> seaborn.py --web true
    ```
    {: pre}

    **Example output**

    ```sh
    ok: created action seaborn
    ```
    {: screen}

11. Invoke the action to test it. Invoking the action returns the base64 string for the generated joint plot.

    ```bash
    ibmcloud fn action invoke seaborn --result
    ```
    {: pre}

    **Example output**

    ```sh
    <base64_string>,
        "headers": {
            "Content-Type": "image/png"
        },
        "isBase64Encoded": "true",
        "statusCode": 200
    }
    ```
    {: screen}

12. Because this action is a web action, you can use the `action get` command to return the URL.

    ```bash
    ibmcloud fn action get seaborn --url
    ```
    {: pre}

    **Example output**

    ```sh
    ok: got action seaborn
    https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace_ID>/default/seaborn
    ```
    {: screen}

13. Copy and paste the URL into your browser to see the generated joint plot. Refresh the page to invoke the action and generate a new plot.

You can use this method of building custom Docker images to install large dependencies rather than packaging them with your app.
{: tip}

## Preparing apps in Docker images
{: #prep_docker}

With {{site.data.keyword.openwhisk_short}}, you can write your app in any language and package it as a Docker image.
{: shortdesc}

You can use images from public registries only, such as an image that is publicly available on Docker Hub. Private registries are not supported. For more information about some possible workarounds, see [Large Applications on OpenWhisk](http://jamesthom.as/blog/2017/08/04/large-applications-on-openwhisk/){: external} and [Large (Java) Applications on Apache OpenWhisk](http://jamesthom.as/blog/2019/02/05/large-java-applications-on-openwhisk/){: external}.
{: important}

Before you begin

- You must have a Docker Hub account. You can set up a free Docker ID and account on [Docker Hub](https://hub.docker.com){: external}.
- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Review the requirements for the Docker runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_docker).
- The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.

### Creating a custom Docker image for your action
{: #prep_create_custom_docker_action}

In a Dockerfile, you can specify a {{site.data.keyword.openwhisk_short}} base runtime image by using the `FROM` instruction. You can use the `RUN` instruction to specify dependencies and packages to install in your Docker image. For more information about creating a Dockerfile, see the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/){: external}.

You can see a list of `ibmfunctions` Docker base images on [Docker Hub](https://hub.docker.com/u/ibmfunctions){: external}.

{{site.data.keyword.openwhisk_short}} limits app code to the maximum `codeSize` as described in the [Action Limits](/docs/openwhisk?topic=openwhisk-limits#limits_actions).

1. Create a `test` directory on your Desktop and `cd` to it.

    ```bash
    cd desktop; mkdir test; cd test
    ```
    {: pre}

2. Create a Dockerfile in your `test` directory and open it in `vim`.

    ```bash
    touch Dockerfile; vim Dockerfile
    ```
    {: pre}

3. Press the `i` key to edit your Dockerfile.
4. Specify a {{site.data.keyword.openwhisk_short}} base image with the `FROM` argument in your Dockerfile.
5. Install any packages and dependencies by specifying them `RUN` argument followed by the installation commands for your dependencies.
6. Press `ESC`, then `:wq` and `Enter` to save and close your Dockerfile.

    **Example Dockerfile for installing Python dependencies**

    The following example uses `ibmfunctions/action-python-v3.7` as a base image and installs the Python modules: `matplotlib`, `pandas`, and `statsmodels`.

    ```docker
    FROM ibmfunctions/action-python-v3.7

    RUN pip install \
        --upgrade pip \
        matplotlib \
        pandas \
        statsmodels
    ```
    {: codeblock}

7. Build your custom Docker image.

    ```docker
    docker build -t <dockerhub_username>/<repo_name>:<tag_name>
    ```
    {: codeblock}

8. Push your image to Docker Hub.

    ```bash
    docker push <dockerhub_username>/<repo_name>:<tag_name>
    ```
    {: pre}

    Be sure to log in to Docker Hub before you attempt to push your image.
    {: tip}

### Deploying an action with a custom Docker image
{: #prep_deploy_action_custom_docker_image}

When you create your {{site.data.keyword.openwhisk_short}} action, you can combine your app file with a public Docker image to create a custom runtime environment. The action is invoked with the Docker image.

Run the `action create` command and include the `--docker` flag to specify a Docker image for your app to use.

```bash
ibmcloud fn action create <action_name> --docker <dockerhub_username>/<image_name> <app_file>
```
{: pre}

You can also deploy a compressed file with a Docker image to create an action. You can use the previous command and replace `<app_file>` with your compressed file. You can use this method to deploy large app files or incorporate large dependencies.
{: tip}

To see an example deployment of a custom Docker image with a {{site.data.keyword.openwhisk_short}} action, see [Packaging large Python dependencies in a custom Docker image](#prep_python_docker).

## Preparing Go Apps
{: #prep_go}

You can create Actions by using Golang.

Use a single file for quick testing or development purposes. For production apps, pre-compile your Go actions into an executable file for better performance. To deploy actions made up of multiple source files and including third-party libraries, package them as compressed file and deploy the file. When deploying a compressed file, specify the runtime by using the `kind` parameter (`--kind=go:1.19`)
{: shortdesc}

Although you can create a compressed file on any Go platform by cross-compiling with `GOOS=Linux` and `GOARCH=amd64`, use the pre-compilation feature that is embedded in the runtime container image(`docker run -i openwhisk/action-golang-v1.19:nightly ...`). You can package [multiple source files](#prep_go_multi_packages) or [vendor libraries](#prep_go_external_libraries).
{: tip}

The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture. You must install the `ibmcloud cli` to run the commands. Note that some examples also require [Docker](https://www.docker.com/){: external}.

### Structuring Go Apps
{: #prep_go_struct}

When you structure your Go code, note that the expected name for the entry point package is `main`. If the package in your code is not `main`, take note of the name to specify it when the action is created (`--name <your name>`). The package must also be public (start with an upper-case letter).
{: shortdesc}

This example creates a simple `Hello World` action in Go.

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

Actions that are written in Go can be deployed as source code or as pre-compiled executable files in a compressed format.
If your actions require only one source file, you can edit its contents directly in the Functions action window in the [IBM Cloud console](https://cloud.ibm.com/functions){: external} if you create the action without pre-compiling it.   
{: tip}

Use the following steps to create actions that use Go.

1. Create the function that you want to deploy.
2. (`optional`) If you have more than one file, package the files as a compressed file, otherwise skip this step (see the following examples)
3. (`optional`) Compile the `go/zip` file by using the Docker image (`docker run -i openwhisk/action-golang-v1.19:nightly -compile ...`).  This step returns a compressed file that contains the executable file.
4. Create the action by using the `ibmcloud cli`.

These steps are used in each of the following examples.

### Creating a simple Golang Action
{: #prep_go_simple}

You can create a simple action in Go by creating a file that contains a Go function.
{: shortdesc}

1. Create the Go file that contains the function. The default entry name is `Main`. You can change this name to any different public function name (`Uppercase first character`) if it is specified by using the `--main` parameter with the `ibmcloud fn action create <action_name> <action_file> --main <function_name>` command; for example, `--main hello` if the function is named `Hello`.

    **`main.go`**

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

2. (`optional`) If you want to pre-compile the function to an executable file that is stored in a compressed format first,

    ```bash
    docker run -i openwhisk/action-golang-v1.19:nightly -compile main <main.go >main-bin.zip
    ```
    {: pre}

    `<` and  `>` are bash input output redirects and are part of the command.
    {: note}

    Specify the generated compressed file (`main-bin.zip`) as the file for the `action create` command.

3. Create an action by using the {{site.data.keyword.openwhisk_short}} managed `go:1.19` runtime. If your action is not called `main`, specify the function name with `--name <your action name>`.

    With the source code (`main.go`),

    ```bash
    ibmcloud fn action create simple-action main.go
    ```
    {: pre}

    With the pre-compiled compressed file (`main-bin.zip`),

    ```bash
    ibmcloud fn action create simple-action main-bin.zip
    ```
    {: pre}

    Alternatively if you want to pin the runtime image to a fixed runtime image version, use the `--docker` tag.

    ```bash
    ibmcloud fn action create simple-action main.go --docker openwhisk/action-golang-v1.19:nightly
    ```
    {: pre}

    If you pin the action to a fixed runtime, it cannot change or receive security fixes.
    {: note}

### Create a Golang action made up of multiple packages
{: #prep_go_multi_packages}

You can create an action that includes multiple Go packages. Each package must include a `go.mod` file.
{: shortdesc}

```sh
.
├── go.mod
├── hello
│   ├── go.mod
│   └── hello.go
└── main.go
```
{: screen}

1. Create the following example files as shown in the following examples.

    **`main.go`**

    ```go
    package main

    import (
        "fmt"
        "hello"
    )

    func Main(args map[string]interface{}) map[string]interface{} {
        fmt.Println("Main")
        return hello.Hello(args)
    }
    ```
    {: codeblock}

    **`go.mod`**

    ```sh
    module action

    go 1.19

    replace hello => ./hello

    require hello v0.0.0

    ```
    {: codeblock}

    **`hello/hello.go`**

    ```go
    package hello

    import (
        "fmt"
    )

    func Hello(args map[string]interface{}) map[string]interface{} {
        msg := make(map[string]interface{})
        greetings := "world"
        name, ok := args["name"].(string)
        if ok {
            greetings = name
        }
        msg["msg"] = "Hello, " + greetings
        fmt.Printf("Hello, %s\n", greetings)
        return msg
    }
    ```
    {: codeblock}

    **`hello/go.mod`**

    ```sh
    module hello

    go 1.19
    ```
    {: codeblock}

2. Compress the source code files into a compressed file that is called `src.zip`.

    ```bash
    zip -r src.zip main.go go.mod hello/hello.go hello/go.mod
    ```
    {: pre}

    This command compresses the files `main.go go.mod hello/hello.go hello/go.mod` into `src.zip`. For more information about the `zip` command, use `man zip`.

    If you are pre-compiling your code you might have to generate a go sum by running `go mod tidy` and adding it to your zip
    {: note}

3. (`Optional`) If you want to pre-compile the code, you can compile your compressed source code with the Docker runtime image using `-compile`

    Compile the function to an executable file that is stored in a compressed format and uses the go runtime itself.

    ```bash
    docker run -i openwhisk/action-golang-v1.19:nightly -compile main <src.zip >main-bin.zip
    ```
    {: pre} 

    `<` and `>` are bash input output redirects and are part of the command.
    {: note}


4. Create the action. Note that the runtime must be specified as `--kind=go:1.19`.

    **With `src.zip`**

    ```bash
    ibmcloud fn action create multiple-packag-action src.zip --kind=go:1.19
    ```
    {: pre}

    **With pre-compiled code (`main-bin.zip`)**

    ```bash
    ibmcloud fn action create multiple-packag-action main-bin.zip --kind=go:1.19
    ```
    {: pre}

    Alternatively, if you want to pin the runtime image to a fixed runtime image version, use the `--docker` tag.

    ```bash
    ibmcloud fn action create multiple-packag-action src.zip --docker openwhisk/action-golang-v1.19:nightly
    ```
    {: pre}

    If you pin the action to a fixed runtime, the runtime cannot change or receive security fixes.
    {: note}


### Create an action by using external libraries with Go modules
{: #prep_go_external_libraries}

You can create an action by using third-party libraries with Go modules. For more information about Go modules, see [Go module doc](https://go.dev/ref/mod){: external}.
{: shortdesc}

If the action has not been pre-compiled, then the libraries are downloaded at the action execution time.
If you pre-compile the action, then the libraries are already packaged into the binary and don't need to be downloaded during the action execution time.
{: tip}

```sh
.
├── go.mod
└── main.go
```
{: screen}

1. Create the function.

    **`main.go`**

    ```go 
    package main

    import (
        "github.com/rs/zerolog"
        "github.com/rs/zerolog/log"
    )

    func init() {
        zerolog.TimeFieldFormat = ""
    }

    // Main function for the action
    func Main(obj map[string]interface{}) map[string]interface{} {
        name, ok := obj["name"].(string)
        if !ok {
            name = "world"
        }
        log.Debug().Str("name", name).Msg("Hello")
        msg := make(map[string]interface{})
        msg["module-main"] = "Hello, " + name + "!"
        return msg
    }
    ```
    {: codeblock}

    **`go.mod`**

    ```sh
    module action

    go 1.19

    require github.com/rs/zerolog v1.19.0
    ```
    {: codeblock}

2. Compress the source code to a compressed file that is called `src.zip`.

    ```bash
    zip -r src.zip main.go go.mod 
    ```
    {: pre}

    This example compresses `main.go` and `go.mod` files to `src.zip`.

    If you are pre-compiling your code you might have to generate a go sum by running `go mod tidy` and adding it to your zip
    {: note}

3. If you want to pre-compile the code, use the compressed source code (`src.zip`) and compile it with the docker runtime image with the `-compile` command.

    1. Compile the function to an executable file that is stored in a compressed format (`main-bin.zip`).

        ```bash
        docker run -i openwhisk/action-golang-v1.19:nightly -compile main <src.zip >main-bin.zip
        ```
        {: pre} 

        `<` and `>` are bash input output redirects and are part of the command.
        {: note}

    2. Specify the compressed file (`main-bin.zip`) as the file for the `action create` command. The runtime `kind` must be specified when you use a compressed file; for example,  `--kind=go:1.19`.

4. Create the action. The runtime must be specified with `--kind=go:1.19`.

    **With `src.zip`**

    ```bash
    ibmcloud fn action create module-action src.zip --kind=go:1.19
    ```
    {: pre}

    **With pre-compiled code (`main-bin.zip`)**

    ```bash
    ibmcloud fn action create module-action main-bin.zip --kind=go:1.19
    ```
    {: pre}

    Alternatively if you want to pin the runtime image to a fixed runtime image version, use the `--docker` tag.

    ```bash
    ibmcloud fn action create module-action src.zip --docker openwhisk/action-golang-v1.19:nightly
    ```
    {: pre}

    If you pin the action to a fixed runtime, the runtime cannot change or receive security fixes.
    {: note}

## Preparing Swift apps
{: #prep_swift}

Swift files must be compiled before an action is run. This delay is known as the cold start delay. To avoid the cold start delay, you can compile your Swift file and then upload it to {{site.data.keyword.openwhisk_short}} in a compressed file. The Docker runtime includes a compiler to help users compile and package Swift 4.2 actions. Subsequent calls to the action are much faster until the container with your action is purged.
{: shortdesc}

Swift actions run in a Linux environment. Swift on Linux is still in development, and {{site.data.keyword.openwhisk_short}} uses the latest available release. These releases might not be stable. The version of Swift that is used with {{site.data.keyword.openwhisk_short}} might be inconsistent with versions of Swift from stable releases of Xcode on macOS.
{: important}

### Structuring Swift code
{: #prep_swift_struc}

The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
{: shortdesc}

In addition to the main function signature, Swift 4 provides two more signatures that take advantage of the [`Codable`](https://developer.apple.com/documentation/swift/codable){: external} type. You can learn more about data type [encoding and decoding for compatibility with external representations such as JSON](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external}.

Example

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

By using the `Codable` completion handler, you can pass an error to indicate a failure in your action. [Error handling in Swift](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external} resembles exception handling in other languages, with the use of the `try`, `catch`, and `throw` keywords.
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
{: shortdesc}

Before you begin

- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- Package your app dependencies with your app.

Package your app by running the following command.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

The Docker container reads the content of the file from `stdin`, and writes a compressed archive with the compiled Swift executable file to `stdout`.

### Packaging Swift 4.2 multi-file projects and dependencies
{: #prep_swift42_multi}

Package your Swift 4.2 multi-file projects and dependencies by creating a directory structure, compressing the contents, and then passing the compressed file to Docker.
{: shortdesc}

Before you begin

- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- Package your app dependencies with your app.

Package your app.

1. To compile multiple files and include external dependencies, create the following directory structure.

    ```sh
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

2. Create an archive with the contents of the directory.

    ```bash
    zip ../action-src.zip -r *
    ```
    {: codeblock}

3. Pass the compressed archive to the Docker container over `stdin`. The `stdout` is a new compressed archive with the compiled executable file. The Docker container reads the content of the compressed archive from `stdin` and writes a new archive with the compiled Swift executable file to `stdout`.

    ```bash
    docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
    ```
    {: codeblock}

    In a Linux based system, you can combine the `zip` and `docker run` steps in a single command:

    ```bash
    zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
    ```
    {: codeblock}

    ```bash
    docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
    ```
    {: codeblock}

    In a Linux-based system, you can combine the `zip` and `docker run` steps in a single command:

    ```bash
    zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
    ```
    {: codeblock}

## Preparing Ruby apps
{: #prep_ruby}

Before you create an action, get your Ruby code ready.
{: shortdesc}

### Structuring Ruby code
{: #prep_ruby_struct}

When you structure your code, note that the expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created. Ruby actions always consume a Hash (dictionary-like collection) and return a Hash.
{: shortdesc}

Example of Ruby code

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

You can package a Ruby app and dependent packages in a compressed file. For example, you can package an action with a second file called `helper.rb`.
{: shortdesc}

Create an archive that contains your source files. The source file that contains the entry point must be named `main.rb`.

Example

**`main.rb`**

```ruby
    require_relative 'helper'
    def main(args)
        name = args["name"] || "stranger"
        greeting = "Hello #{name}!"
        help = helper(greeting)
        puts help
        { "help" => help }
    end
```
{: codeblock}

**`helper.rb`**

```ruby
    def helper(greeting)
        puts greeting
        "#{greeting} I am here to help"
    end
```
{: codeblock}

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

```bash
ibmcloud fn action create hello-ruby helloRuby.zip --kind=ruby:2.6
```
{: pre}

The gems `mechanize` and `jwt` are available in addition to the default and bundled gems. You can use arbitrary gems if you also use compression actions to package all the dependencies.

## Preparing PHP apps
{: #prep_php}

Before you create an action, get your PHP code ready.
{: shortdesc}

### Structuring PHP code
{: #prep_php_struct}

When you structure your code, note that the expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created. PHP actions always consume an associative array and return an associative array.
{: shortdesc}

Example

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

You can package PHP files or dependent packages in a compressed file.
{: shortdesc}

Before you begin, [review the packages that are included with the PHP runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_php) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

To package your app, run the following command.

```bash
zip -r <archive_name>.zip <file_1>.php <file_2>.php
```
{: pre}

For example, package the following code examples.

1. Create the code examples.

    **`index.php`**

    ```php
    <?php
        include 'helper.php';
        function main(array $args) : array
        {
            $name = $args["name"] ?? "stranger";
            $help = help($name);
            return ["help" => $help];
        }
        ?>
    ```
    {: codeblock}

    **`helper.php`**

    ```php
        <?php
        function help($name) {
            return "Hello " . $name . " the answer to life the universe and everything is 42";
        }
        ?>
    ```
    {: codeblock}


2. Compress the code examples.

    ```bash
    zip -r helloPHP.zip index.php helper.php 
    ```
    {: pre}

3. Create the action.

    ```bash
    ibmcloud fn action create packagePHP helloPHP.zip --kind=php:7.4
    ```
    {: pre}

### Packaging Composer modules
{: #prep_php_composer}

You can package extra Composer modules into your action
{: shortdesc}

Before you begin, [review the packages that are included with the php runtime](/docs/openwhisk?topic=openwhisk-runtimes#openwhisk_ref_php) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app. The following steps assume that you are running the commands on a Linux-based distribution on a processor with AMD64-based architecture.

1. In the root directory, create a `index.php` and `composer.json` file.

    **`index.php`**
    ```php
    <?php
    use Mpociot\ChuckNorrisJokes\JokeFactory;

    function main(array $args) : array
    {
        $jokes = new JokeFactory();
        $joke = $jokes->getRandomJoke();
        return ["joke" => $joke];
    }
    ?>
    ```

    **`composer.json`**
    ```json
    {
        "require": {
            "mpociot/chuck-norris-jokes": "^1.0"
        }
    }

    ```
    {: codeblock}

    

2. Install all dependencies locally.

    ```bash
    composer install
    ```
    {: pre}

    While most `php` packages install PHP sources on `composer install`, some packages also install and compile platform-dependent binary file artifacts. Because this environment is Linux AMD64-based, the `php install` must be executed on a similar platform. Otherwise the action invocations might not succeed.
    {: note}

3. Create an archive that contains all files, including all dependencies.

    ```bash
    zip -r action.zip *
    ```
    {: pre}

    **Windows users** Using the Windows Explorer action for creating the compressed file results in an incorrect file structure. {{site.data.keyword.openwhisk_short}} archive actions must have `index.php` and `composer.json` at the root of the archive, but Windows Explorer places it inside a nested folder. Use the **`zip`** command instead.
    {: tip}

4. Create the Function

    ```bash
    ibmcloud fn action create chuckJoke action.zip --kind=php:7.4
    ```
    {: pre}

## Preparing Java apps
{: #prep_java}

Before you create an action, get your Java code ready.
{: shortdesc}

### Structuring Java code
{: #prep_java_struct}

A Java action is a Java program with a method called `main`. `main` must have the following signature.
{: shortdesc}

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* You must specify the name of the main class by using `--main`. An eligible main class is one that implements a static `main` method. If the class is not in the default package, use the Java fully qualified class name, for example, `--main com.example.MyMain`.
* You can customize the method name of your Java action by specifying the fully qualified method name of your action, for example, `--main com.example.MyMain#methodName`.

### Packaging Java code
{: #prep_java_pkg}

Package your code by creating a `.jar` file.
{: shortdesc}

Before you begin

You must have [JDK 8](http://openjdk.java.net/install/){: external} installed locally. This example uses the [`google-gson-2.9.0.jar`](https://mvnrepository.com/artifact/com.google.code.gson/gson/2.9.0){: external}.

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

2. Download the [`gson-2.9.0.jar`](https://mvnrepository.com/artifact/com.google.code.gson/gson/2.9.0){: external}.

3. Add the `gson-2.9.0.jar` to your `ClASSPATH`. This example uses `gson-2.9.0.jar`, which is saved in a `test` folder in the `Desktop` directory.

    ```bash
    export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.9.0.jar
    ```
    {: pre}

4. Add the `bin` folder of your JDK to your `CLASSPATH`. This example uses `openjdk-8`.

    ```bash
    export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
    ```
    {: pre}

5. Verify the JDK `bin` folder and `gson-2.9.0.jar` are in your `CLASSPATH`.

    ```bash
    echo $CLASSPATH
    ```
    {: pre}

    **Example output**

    ```sh
    /Desktop/test/gson-2.9.0.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
    ```
    {: screen}

6. Navigate to the folder where your `Hello.java` file is stored. In this example, the `Hello.java` file is saved to the `Desktop/test` folder.

    ```bash
    cd Desktop/test
    ```
    {: pre}

7. Compile your `Hello.java` file into a class file.

    ```bash
    javac Hello.java
    ```
    {: pre}

8. Compress the class file into a `.jar` file named `hello.jar`.

    ```bash
    jar cvf hello.jar Hello.class
    ```
    {: pre}

You can create an action with your `hello.jar`. Because the class file you created does not use the default name `main`, you must set the `--main` flag to `Hello` when you create your action. The `--main` flag must match your Java `class`. For more information, see [Creating actions](/docs/openwhisk?topic=openwhisk-actions).

When you update your Java code, you must repeat these steps to recompile your code into a new `.jar` file.
{: note}

### Packaging Java code with Gradle
{: #prep_java_gradle}

Instead of compiling from the command line, you can use a build a tool such as [Gradle](https://gradle.org){: external} to fetch the libraries from a repository like Maven Central. You can use Gradle to fetch and build a final .jar archive that includes your code and all dependencies.

The following example uses Gradle to build a Java action that leverages the library `com.google.zxing` that provides the functionality to generate a QR code image.

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
        provided 'com.google.code.gson:gson:2.9.0'
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

2. Run the command **`gradle jar`**, which generates a .jar archive in the directory `build/libs/`.

For more information, read the Gradle documentation [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external}.

### Packaging Java code by using the Java runtime with Docker
{: #prep_java_docker}

Package your code with Docker by creating a `.jar` file inside the Java runtime.
{: shortdesc}

Before you begin, you must have Docker installed locally.

You won´t need Java installed locally as everything is supplied by the Java runtime.
{: note}

To create a Java action by using Docker, complete the following steps.

1. Create `Hello.java` file
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
    {: pre}

2. Navigate to the folder that contains the `Hello.java` file and run the Docker Java runtime container.

    ```bash
    docker run --rm -it  --entrypoint "/bin/bash" -v $PWD:/tmp openwhisk/java8action:nightly
    ```
    {: pre}

    Use the `nightly` tag for the latest runtime version or a specific tag.
    {: note}

3. Set up the container.

    1. Navigate to the `/tmp` folder that contains your action code mounted from the host system.
    
        ```bash
        cd /tmp
        ```
	{: pre}

    2. Install `curl` to download the dependencies.
        ```bash
        apt update && apt install curl -y
        ```
	{: pre}

    3. Download the `gson` dependency.
        ```bash
        curl -L -o gson-2.9.0.jar https://repo1.maven.org/maven2/com/google/code/gson/gson/2.8.5/gson-2.9.0.jar
        ```
	{: pre}
	
    4. Export the path and add `gson` to it.
        ```bash
        export CLASSPATH=$CLASSPATH:$PWD/gson-2.9.0.jar
        export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
        ```
	{: pre}
	
4. Compile the code

    1. Compile the Java code to a class
        ```bash
        javac Hello.java
        ```
	{: pre}
    
    2. Package the Java class to a deployable `.jar` file.
        ```bash
        jar cvf Hello.jar Hello.class
        ```
	{: pre}

5. Exit the runtime container.

    ```bash
    exit
    ```
    {: pre}

6. Create action called `hello-java`.

    ```bash
    ibmcloud fn action create hello-java Hello.jar --main Hello
    ```
    {: pre}

7. Invoke the action 

    ```bash
    ibmcloud fn action invoke hello-java -b
    ```
    {: pre}

### Packaging Java code with Maven inside Docker
{: #prep_java_maven_docker}

1. Create a maven Project

2. Navigate to the folder that contains the maven Project and run the Docker Java runtime container.

    ```bash
    docker run --rm -it  --entrypoint "/bin/bash" -v $PWD:/tmp openwhisk/java8action:nightly
    ```
    {: pre}

    This command run the java runtime container and map the current directory inside the container to `/tmp`

    Use the `nightly` tag for the latest runtime version or a specific tag.
    {: note}

3. Set up the container.

    Navigate to the `/tmp` folder that contains your action code mounted from the host system.
    ```bash
    cd /tmp
    ```
    {: pre}

4. Install `curl` and `unzip` to install Maven.

    ```bash
    apt update && apt install curl unzip -y
    ```
    {: pre}

5. Download  `Maven`

    ```bash
    curl -L -o apache-maven-3.8.5-bin.zip https://dlcdn.apache.org/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.zip
    ```
    {: pre}

6. Extract Maven

    ```bash
    unzip apache-maven-3.8.5-bin.zip
    ```
    {: pre}

7. Add Maven to path

    ```sh
    export PATH=$PATH:/apache-maven-3.8.5/bin
    ```
    {: pre}

8. Test Maven installation

    ```bash
    mvn -v
    ```
    {: pre}
	
9. Package the action code with maven

    ```bash
    mvn package 
    ```
    {: pre}

10. Exit the runtime container.

    ```bash
    exit
    ```
    {: pre}

    At this point, the current directory contains a `traget` directory containing the action `.jar` file.

11. Create action called .

    ```bash
    ibmcloud fn action create <actionname> <Jar name>.jar --main <package name and class name seperated by dots>
    ```
    {: pre}

    The main name is something similar to `com.package.example.Hello`.
    {: note}

12. Invoke the action 

    ```bash
    ibmcloud fn action invoke <actionname> -b
    ```
    {: pre}


## Preparing .NET Core apps
{: #prep_dotnet}

Before you create an action, get your .NET Core code ready.
{: shortdesc}

### Structuring .NET Core code
{: #prep_dotnet_struct}

A .NET Core action is a .NET Core class library with a method that is expected to be named `Main`. If the method in your code is not `Main`, take note of the name to specify it when the action is created in the format: `--main {Assembly}::{Class Full Name}::{Method}`
{: shortdesc}

```vbnet
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```
{: codeblock}

### Packaging .NET Core code
{: #prep_dotnet_pkg}

Package your code by first compiling it and then compressing the results.
{: shortdesc}

Before you begin

To compile, test, and archive .NET Core projects, you must:

- Install the [.NET Core SDK](https://dotnet.microsoft.com/download){: external} locally.
- Set the `DOTNET_HOME` environment variable to the location where the `dotnet` executable file can be found.



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

3. Install the [`Newtonsoft.Json NuGet` package](https://www.nuget.org/packages/Newtonsoft.Json/){: external}.

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



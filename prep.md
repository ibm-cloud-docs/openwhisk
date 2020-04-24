---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-22"

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

**Example**

```javascript
  function main() {
      return {payload: 'Hello world'};
  }
```
{: codeblock}

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

**Example of synchronous code**

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

You can package an app by using a JavaScript module bundler such as `[fbpack ](https://webpack.js.org/concepts/){: external}`. When `webpack` processes your code, it recursively builds a dependency graph that includes every module that your action needs.
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

6. Create the action by using the `npm` script or the CLI.

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

   ```
   {
     "name": "my-action",
     "dependencies" : {
       "left-pad" : "1.1.3"
      }
   }
   ```
   {: codeblock}

2. Install all dependencies locally.

   ```
   npm install <dependency>
   ```
   {: pre}

   While most `npm` packages install JavaScript sources on `npm install`, some also install and compile binary file artifacts. The archive file upload supports only JavaScript dependencies. If the archive includes binary file dependencies, action invocations might not succeed.
   {: note}

3. Create a `.zip` archive that contains all files, including all dependencies.

   ```
   zip -r action.zip *
   ```
   {: pre}

   **Windows users** Using the Windows Explorer action for creating the .zip file results in an incorrect file structure. {{site.data.keyword.openwhisk_short}} .zip actions must have `package.json` at the root of the archive, but Windows Explorer places it inside a nested folder. Use the `zip` command instead.
   {: tip}
  
## How do I package my Python app for deployment in {{site.data.keyword.openwhisk_short}}
{: #how_to_package_python}

### Structuring Python code
{: #prep_python_struct}

Python apps must consume a dictionary and produce a dictionary. The expected name for the entry point method is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
{: shortdesc}

**Example**

```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
	return {"greeting": greeting}
```
{: codeblock}

### Packaging multiple Python files into a .zip file
When to use this method:

Your app uses multiple Python files, but does not require any dependencies or packages outside of the packages that are included with the base Python runtime. You can create a .zip file that includes your Python files and deploy the .zip file when you create your action.

**Example command**

```
ibmcloud fn action create <action_name> <compressed_python_files.zip>
```
{: pre}

For more information, see [Packaging multiple Python files into a .zip file](#prep_python_pkg).

### Packaging Python code with a local virtual environment in a .zip file
When to use this method:

If your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments), you can install those dependencies into a `virtualenv` folder and then compress into a .zip to deploy in {{site.data.keyword.openwhisk_short}}. Your .zip file must be smaller than 48 MB when compressed.

**Example command**

```
ibmcloud fn action create <action_name> <compressed_python_virtualenv.zip>
```
{: pre}

For more information, see [Packaging Python code with a local virtual environment in a .zip file](#prep_python_local_virtenv).

### Packaging Python code with a Docker virtual environment in a .zip file
When to use this method:

If your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments), you can install those dependencies into a `virtualenv` folder within a Docker image. You can then compress the folder into a .zip to deploy in {{site.data.keyword.openwhisk_short}}. Your .zip file must be smaller than 48 MB when compressed.

**Example command**

You can use the .zip file to create an action. Replace `<file_path>` with the file path to your .zip file.

```
ibmcloud fn action create <action_name> <compressed_python_virtualenv.zip>
```
{: pre}

For more information, see [Packaging Python code with a Docker virtual environment in a .zip file](#prep_python_virtenv).

### Packaging large Python dependencies in a custom Docker image
When to use this method:

Your app requires dependencies that are not included with the base {{site.data.keyword.openwhisk_short}} [Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments). You can specify a base {{site.data.keyword.openwhisk_short}} image in your Docker file and also specify the dependencies to install when you build your Docker image. You can then specify your custom Docker image when you deploy your app in {{site.data.keyword.openwhisk_short}}. Note that only public Docker images are supported.

**Example command**

```
ibmcloud fn action create <action_name> <app_code.py> --docker <dockerhub_username>/<repo_name>:<tag_name>
```
{: pre}

If your app code is larger than 48 MB, you can combine this method with [Packaging multiple Python files into a .zip file](#prep_python_pkg).
{: tip}

For more information, see [Packaging large Python dependencies in a custom Docker image](#prep_python_docker).


## Packaging Python code
{: #prep_python}

The following sections provide tutorials for how to package your Python app for deployment with {{site.data.keyword.openwhisk_short}}.

**Before you begin**

Review [How do I package my Python app for deployment in {{site.data.keyword.openwhisk_short}}?](#how_to_package_python).

### Packaging multiple Python files into a .zip file
{: #prep_python_pkg}

Package Python code and dependent modules in a .zip file. In this example, the source file that contains the entry point is `__main__.py` and the helper modules are in a file called `helper.py`.

Before you begin, [review the packages that are included with the Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

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

4. To package your app as .zip, `cd` to your `test` directory and run the following command. In this example, the .zip archive is called `stranger.zip`.

  ```
  zip -r stranger.zip __main__.py helper.py
  ```
  {: pre}

5. You can use then use the .zip file to create an action called `hello`. Replace `<file_path>` with the file path to your .zip file.

  ```
  ibmcloud fn action create hello <file_path>/test/stranger.zip --kind python:3.7
  ```
  {: pre}

6. Test the action.

  ```
  ibmcloud fn action invoke hello --result
  ```
  {: pre}

  **Output**
  
  ```
  {
    "greeting": "Hello from helper.py, stranger!"
  }
  ```
  {: screen}

7. Test the action again and specify the `name` parameter. Replace the `<your_name>` parameter with your name.

  ```
  ibmcloud fn action invoke hello --result --param name <your_name>
  ```
  {: pre}

  **Example output**
  
  ```
  {
    "greeting": "Hello from helper.py, <your_name>!"
  }
  ```
  {: screen}
  

### Packaging Python code with a local virtual environment in a .zip file
{: #prep_python_local_virtenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. With the virtual environment, you can link additional packages that can be installed by using [`pip` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://packaging.python.org/tutorials/installing-packages/). 

Packages must be available for the Python runtime that is used in {{site.data.keyword.openwhisk_short}}.

Before you begin, [review the packages that are included with the Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Before you begin, install the `virtualenv` Python package.
  ```
  pip install virtualenv
  ```
  {: pre}

To package your app:

1. Create a directory that you can use to create your virtual environment. In this example, a `jokes` directory is created on the desktop. After you create the `jokes` directory, `cd` to it.

    ```
    cd desktop; mkdir jokes; cd jokes
    ```
    {: pre}

2. From the `jokes` directory, create a virtual environment named `virtualenv`. Your virtual environment must be named `virtualenv`.

    ```
    virtualenv virtualenv
    ```
    {: pre}

    **Output**
    
    ```
    Using base prefix '/Library/Frameworks/Python.framework/Versions/3.7'
    New python executable in Desktop/jokes/virtualenv/bin/python3
    Also creating executable in Desktop/jokes/virtualenv/bin/python
    Installing setuptools, pip, wheel...
    done.
    ```
    {: screen}

3. From your `jokes` directory, activate your `virtualenv` virtual environment.

    ```
    source virtualenv/bin/activate
    ```
    {: pre}

4. Install the `pyjokes` module.

    ```
    (virtualenv) $ pip install pyjokes
    ```
    {: codeblock}

    **Output**
    
    ```
    Collecting pyjokes
    Using cached https://files.pythonhosted.org/packages/a0/70/65457d1b197b1a0195a95128632d9a0b59d7d8ced6035c80e8467731dc76/pyjokes-0.5.0-py2.py3-none-any.whl
    Installing collected packages: pyjokes
    Successfully installed pyjokes-0.5.0
    ```
    {: screen}
  
5. Stop the virtualenv.
    ```
    (virtualenv) $ deactivate
    ```
    {: codeblock}

6. Copy the following code and save it into a file called `__main__.py` in your `jokes` directory.

    ```python
    import pyjokes

    def joke(params):
        return {"joke": pyjokes.get_joke()}
    ```
    {: codeblock}

7. From your `jokes` directory, create a .zip archive of the `virtualenv` folder and your `__main__.py` file. These files must be in the top level of your .zip file.

    ```
    zip -r jokes.zip virtualenv __main__.py
    ```
    {: pre}

    **Output**
    
    ```
    ...
    adding: virtualenv/lib/python3.7/abc.py (deflated 72%)
    adding: virtualenv/lib/python3.7/_bootlocale.py (deflated 63%)
    adding: __main__.py (deflated 17%)
    ...
    ...
    ```
    {: screen}

8. Create an action called `jokes` using your `jokes.zip` file. You must also specify the entry point as `jokes`. You must also specify the `--kind` flag for the runtime.

    ```
    ibmcloud fn action create jokes </path/to/file/>jokes.zip --kind python:3.7 --main joke
    ```
    {: pre}

    **Output**
    
    ```
    ok: created action jokes
    ```
    {: screen}

9. Invoke the action to verify it is working. Include the `--result` flag to return the result in the command line.

    ```
    ibmcloud fn action invoke jokes --result
    ```
    {: pre}

    **Example output**
    
    ```
    {
    "joke": "A QA engineer walks into a bar. Runs into a bar. Crawls into a bar. Dances into a bar. Tiptoes into a bar. Rams a bar. Jumps into a bar."
    }
    ```
    {: screen}

You can use this method to extend the functionality of {{site.data.keyword.openwhisk_short}} actions by using other Python packages.
{: tip}


### Packaging Python code with a Docker virtual environment in a .zip archive
{: #prep_python_virtenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. By using the virtual environment, you can link more packages that can be installed by using [`pip` ](https://packaging.python.org/tutorials/installing-packages/){: external}.

Before you begin, [review the packages that are included with the Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Only public Docker images are supported.
{: note}

Package your app by completing the following steps.

1. Create a directory that you can use to create your virtual environment. In this example, a `test` directory is created on the desktop. After you create the `test` directory, `cd` to it.

    ```
    cd desktop; mkdir test; cd test
    ```
    {: pre}

2. Create a [requirements.txt ![External link icon](../icons/launch-glyph.svg "External link icon")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) in your file in your `test` directory that contains the `pip` modules and versions to install.

    ```
    touch requirements.txt
    ```
    {: pre}

3. Use vim to edit the `requirements.txt` file. Enter the names of the `pip` modules and versions you want to install. In this example, `sklearn` is the module that is used.

    ```
    vim requirements.txt
    ```
    {: pre}

    **Example requirements.txt**
    
    ```
    sklearn
    ```
    {: codeblock}

4. Press `ESC`, then `:wq` to save and close your `requirements.txt` file.

    To keep the `virtualenv` to a minimum size, add only the modules that are not part of the selected runtime environment to the `requirements.txt`. For more information about the packages that are included in Python runtimes, see the Python [runtime reference](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments).
    {: tip}


    * For `python:3.7`, use the Docker image `ibmfunctions/action-python-v3.7`.
    * For `python:3.6`, use the Docker image `ibmfunctions/action-python-v3.6`.
    * For `python:2`, use the Docker image `openwhisk/python2action`.

   **Example**
   
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

   **Output**
   
   ```
   Using default tag: latest
   latest: Pulling from ibmfunctions/action-python-v3.7
   ```
   {: screen}

6. Install the dependencies and create a virtual environment. The virtual environment directory must be named `virtualenv` in order to create a `virtualenv` folder in the `test` directory.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

   **Output**
   
   ```
   Using base prefix '/usr/local'
   New python executable in /tmp/virtualenv/bin/python
   Installing setuptools, pip, wheel...
   done
   Collecting sklearn (from -r requirements.txt (line 1))
   Downloading https://files.pythonhosted.org/packages/1e/7a/dbb3be0ce9bd5c8b7e3d87328e79063f8b263b2b1bfa4774cb1147bfcd3f/sklearn-0.0.tar.gz
   Collecting scikit-learn (from sklearn->-r requirements.txt (line 1))
   Downloading https://files.pythonhosted.org/packages/9f/c5/e5267eb84994e9a92a2c6a6ee768514f255d036f3c8378acfa694e9f2c99/scikit_learn-0.21.3-cp37-cp37m-manylinux1_x86_64.whl (6.7MB)
   ...
   ...
   ```
   {: screen}

7. Save the following code as `__main__.py` in your `test` directory. When you create actions with a .zip file, the source file that contains the entry point must be named `__main__.py`.

    ```python
    import sklearn

    def main(params):
        return {
            "sklearn": sklearn.__version__
        }
    ```
    {: codeblock}

8. In order to deploy this code as an action, you must create a .zip file of the `virtualenv` folder and the `__main__.py` file. However, in this case, the resulting .zip file is larger than the 48 MB allowed by {{site.data.keyword.openwhisk_short}}. To reduce the size of the .zip file, select only the dependencies that you need, rather than selecting the entire `virtualenv` folder. The packages that you need can be found in the `site-packages` directory within the `virtualenv` folder. Note that you must also include the `activate_this.py` file from the `bin` directory of your `virtualenv` folder in your .zip file.   

   In this example, you must also include `joblib` package from the `site-packages` folder since it is a dependency of `sklearn`. However, you do not need to include `sklearn` dependencies that are included in {{site.data.keyword.openwhisk_short}} default images, such as `numpy`.

   ```
   zip -r sklearn.zip virtualenv/bin/activate_this.py virtualenv/lib/python3.7/site-packages/joblib virtualenv/lib/python3.7/site-packages/sklearn __main__.py
   ```
   {: pre}
   
   If the archive file that you create with your `virtualenv` and `__main__.py` file is smaller than 48 MB, you can create the action without selecting dependencies from the `site-packages` directory.
   {: tip}
	
9. Create an action called `sklearn` by using the `sklearn.zip` file.

    ```
    ibmcloud fn action create sklearn <file_path>/sklearn.zip --kind python:3.7
    ```
    {: pre}

10. Invoke the action to test that the `sklearn` module is working.

    ```
    ibmcloud fn action invoke sklearn2 --result
    ```
    {: pre}
    
    **Output**

    ```
    {
        "sklearn": "0.21.3"
    }
    ```
    {: screen}

### Packaging large Python dependencies in a custom Docker image
{: #prep_python_docker}

{{site.data.keyword.openwhisk_short}} has a limit of 48 MB for app code. However, you can install large packages and dependencies into a custom Docker image and deploy it with your app code when you create an action. You can then import the packages at run time. 

In this example, install large Python packages such as `matplotlib` and `seaborn` to build a {{site.data.keyword.openwhisk_short}} web action that generates a PNG file of a joint plot with `seaborn`.

Before you begin, review the packages that are included with the [Python runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Only public Docker images are supported.
{: note}

Package the app in a custom Docker image by completing the following steps.

1. Create a directory that you can use to create your Dockerfile. In this example, a `functions` directory is created on the desktop. After you create the `functions` directory, `cd` to it.

    ```
    cd desktop; mkdir functions; cd functions
    ```
    {: pre}

2. Create a [Dockerfile ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/reference/builder/) in your `functions` directory.

    ```
    touch Dockerfile
    ```
    {: pre}

3. Use vim to edit the `Dockerfile` file. Enter the names of the `pip` modules and versions you want to install. In this example, several additional Python modules are installed.

    ```
    vim Dockerfile
    ```
    {: pre}

4. Paste or enter the following text in your `Dockerfile`.

    ```
    FROM ibmfunctions/action-python-v3.7

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

    ```
    docker build -t <dockerhub_username>/<repo_name>:<tag_name> .
    ```
    {: pre}
    
    You can also run `docker build` commands with this format:
    
    ```
    docker build . -t <dockerhub_username>/<repo_name>:<tag_name>
    ```
    {: pre}
    
    

7. The image builds and installs the dependencies that you specified in your Dockerfile.

   ```
   Sending build context to Docker daemon  2.048kB
   Step 1/2 : FROM ibmfunctions/action-python-v3.7
   ---> b84f64e9df1b
   Step 2/2 : RUN pip install       --upgrade pip       matplotlib       seaborn       pandas       statsmodels
   ---> Running in 990eccb27ed0
   Collecting pip
     Downloading https://files.pythonhosted.org/packages/62/ca/94d32a6516ed197a491d17d46595ce58a83cbb2fca280414e57cd86b84dc/pip-19.2.1-py2.py3-none-any.whl (1.4MB)
   Collecting matplotlib
     Downloading https://files.pythonhosted.org/packages/19/7a/60bd79c5d79559150f8bba866dd7d434f0a170312e4d15e8aefa5faba294/matplotlib-3.1.1-cp37-cp37m-manylinux1_x86_64.whl (13.1MB)
     ...
     ...
   ``` 
   {: screen}

8. Push your image to Docker Hub.

   ```
   docker push <dockerhub_username>/<repo_name>:<tag_name>
   ```
   {: pre}

9. Save the following code as `seaborn.py` in your `functions` directory. This code generates a joint plot in [`seaborn`](https://seaborn.pydata.org/) that uses random data. You can then create a web action with {{site.data.keyword.openwhisk_short}} to return the plot to a {{site.data.keyword.openwhisk_short}} endpoint.

   ```python
       # import modules
       import base64
       import pandas as pd
       import matplotlib.pyplot as plt
       import seaborn as sns
   
       # optional: set seaborn style
       sns.set(style="dark") 
       def main(args):
      
          # generate a jointplot from random data.
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
       # main(args)
       # color codes (any hex code)
       # green - "#4CB391"
       # red/orange - "#e74c3c"
       # purple - "#9b59b6"
       # blue - "#16099d"
   ```
   {: codeblock}   

10. Create a web action called `seaborn` by using the custom Docker image that you created that contains the required Python dependencies to run a joint plot.

   ```
   ibmcloud fn action create seaborn --docker <dockerhub_username>/<repo_name>:<tag_name> seaborn.py --web true
   ```
   {: pre}

   **Output**
  
   ```
   ok: created action seaborn
   ```
   {: screen}

11. Invoke the action to test it. Invoking the action returns the base64 string for the generated joint plot.

   ```
   ibmcloud fn action invoke seaborn --result
   ```
   {: pre}

   **Example output**
  
   ```
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

   ```
   ibmcloud fn action get seaborn --url
   ```
   {: pre}

   **Example output**
  
   ```
   ok: got action seaborn
   https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/default/seaborn
   ```
   {: screen}

12. Copy and paste the URL into your browser to see the generated joint plot. Refresh the page to invoke the action and generate a new plot.

You can use this method of building custom Docker images to install large dependencies rather than packaging them with your app.
{: tip}

## Preparing apps in Docker images
{: #prep_docker}

With {{site.data.keyword.openwhisk_short}}, you can write your app in any language and package it as a Docker image.
{: shortdesc}

You can use images from public registries only, such as an image that is publicly available on Docker Hub. Private registries are not supported. For more information about some possible workarounds, see [Large Applications on OpenWhisk](http://jamesthom.as/blog/2017/08/04/large-applications-on-openwhisk/){:external} and [Large (Java) Applications on Apache OpenWhisk](http://jamesthom.as/blog/2019/02/05/large-java-applications-on-openwhisk/){:external}.
{: important}

**Before you begin**

- You must have a Docker Hub account. You can set up a free Docker ID and account on [Docker Hub](https://hub.docker.com){: external}.
- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){:external}.
- [Review the requirements for the Docker runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

### Creating a custom Docker image for your action
In a Dockerfile, you can specify a {{site.data.keyword.openwhisk_short}} base runtime image by using the `FROM` instruction. You can use the `RUN` instruction to specify dependencies and packages to install in your Docker image. For more information about creating a Dockerfile, see the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/){: external}.

You can see a list of `ibmfunctions` Docker base images on [Docker Hub](https://hub.docker.com/u/ibmfunctions){: external}.

{{site.data.keyword.openwhisk_short}} limits app code to 48 MB. 

1. Create a `test` directory on your Desktop and `cd` to it.

  ```
  cd desktop; mkdir test; cd test
  ```
  {: pre}
2. Create a Dockerfile in your `test` directory and open it in `vim`.

  ```
  touch Dockerfile; vim Dockerfile
  ```
  {: pre}
  
3. Press the `i` key to edit your Dockerfile.
4. Specify a {{site.data.keyword.openwhisk_short}} base image with the `FROM` argument in your Dockerfile.
5. Install any packages and dependencies by specifying them `RUN` argument followed by the installation commands for your dependencies.
6. Press `ESC`, then `:wq` and `Enter` to save and close your Dockerfile.

  **Example Dockerfile for installing Python dependencies**
  
  The following example uses `ibmfunctions/action-python-v3.7` as a base image and installs the Python modules: `matplotlib`, `pandas`, and `statsmodels`.

    ```
    FROM ibmfunctions/action-python-v3.7

    RUN pip install \
      --upgrade pip \
      matplotlib \
      pandas \
      statsmodels
    ```
    {: codeblock}

7. Build your custom Docker image.

  ```
  docker build -t <dockerhub_username>/<repo_name>:<tag_name>
  ```
  {: pre}

8. Push your image to Docker Hub.

  ```
  docker push <dockerhub_username>/<repo_name>:<tag_name>
  ```
  {: pre}

### Deploying an action with a custom Docker image
When you create your {{site.data.keyword.openwhisk_short}} action, you can combine your app file with a public Docker image to create a custom runtime environment. The action will be invoked with the Docker image.

Run the `action create` command and include the `--docker` flag to specify a Docker image for your app to use.

```
ibmcloud fn action create <action_name> --docker <dockerhub_username>/<image_name> <app_file>
```
{: pre}

You can also deploy a .zip file with a Docker image to create an action. You can use the previous command and replace `<app_file>` with your .zip file. You can use this method to deploy large app files or incorporate large dependencies.
{: tip}

To see an example deployment of a custom Docker image with a {{site.data.keyword.openwhisk_short}} action, see [Packaging large Python dependencies in a custom Docker image](#prep_python_docker).

## Preparing Go apps
{: #prep_go}

Use a single file for quick testing or development purposes. For production apps, pre-compile your Go actions into an executable file for better performance or multiple source file support, including vendor libraries.
{: shortdesc}

Although you can create a compressed file on any Go platform by cross-compiling with `GOOS=Linux` and `GOARCH=amd64`, use the pre-compilation feature that is embedded in the runtime container image. You can package [multiple source files](#prep_go_multi) or [vendor libraries](#prep_go_vendor).
{: tip}

### Structuring Go code
{: #prep_go_struct}

When you structure your Go code, note that the expected name for the entry point package is `main`. If the package in your code is not `main`, take note of the name to specify it when the action is created. The package must also be public.
{: shortdesc}

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

You can package multiple Go source files by creating a top-level `src` directory, importing the subpackages, and then compiling.
{: shortdesc}

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

2. Import subpackages. The following example shows `main/main.go` importing the `hello` subpackage.

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

   **Example of `hello/hello.go`**

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

4. Compile and package the Go executable file as `exec` in the root of the .zip archive. Build the `hello-bin.zip` archive by running the following command. You must install the Docker CLI on your workstation and put `docker` in your `PATH`.

   ```bash
   docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
   ```
   {: pre}

   In this example, the main function is `-compile main`. To use a different function as main, change the value for `-compile`. The main function is selected at compilation time. When you pre-compile, `ibmcloud fn action [update | create]` ignores the `--main`.

   The container gets the contents of the source .zip in `stdin`, compiles the content, and creates a new .zip archive with the executable `exec` file in the root. The .zip archive content streams out to `stdout`, which gets redirected to the `hello-bin.zip` archive to be deployed as the Go Action.

### Packaging Go code with vendor libraries
{: #prep_go_vendor}

You can include dependencies by populating a `vendor` directory inside the source `zip` archive when you compile the Go files. The `vendor` directory does not work at the top level. You must place the `vendor` directory within `src/` and inside a package directory.
{: shortdesc}

**Example: log package `logrus` in a `hello.go` app**

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

To populate the `vendor` directory, run `dep ensure`.



## Preparing Swift apps
{: #prep_swift}

Swift files must be compiled before an action is run. This delay is known as the cold start delay. To avoid the cold start delay, you can compile your Swift file and then upload it to {{site.data.keyword.openwhisk_short}} in a .zip file. The Docker runtime includes a compiler to help users compile and package Swift 4.2 actions. Subsequent calls to the action are much faster until the container with your action is purged.
{: shortdesc}

Swift actions run in a Linux environment. Swift on Linux is still in development, and {{site.data.keyword.openwhisk_short}} uses the latest available release. These releases might not be stable. The version of Swift that is used with {{site.data.keyword.openwhisk_short}} might be inconsistent with versions of Swift from stable releases of Xcode on macOS.
{: important}

### Structuring Swift code
{: #prep_swift_struc}

The expected name for the entry point function is `main`. If the function in your code is not `main`, take note of the name to specify it when the action is created.
{: shortdesc}

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
{: shortdesc}

**Before you begin**
- [Install Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Review the packages that are included with the Swift runtime](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

Package your app by running the following command.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

The Docker container reads the content of the file from `stdin`, and writes a .zip archive with the compiled swift executable file to `stdout`.

### Packaging Swift 4.2 multi-file projects and dependencies
{: #prep_swift42_multi}

Package your Swift 4.2 multi-file projects and dependencies by creating a directory structure, compressing the contents, and then passing the .zip file to Docker.
{: shortdesc}

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

3. Pass the .zip archive to the Docker container over `stdin`. The `stdout` is a new .zip archive with the compiled executable file. The Docker container reads the content of the .zip archive from `stdin` and writes a new .zip archive with the compiled Swift executable file to `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  In a Linux based system, you can combine the `zip` and `docker run` steps in a single command:

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}

   ```
   docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
   ```
   {: codeblock}

   In a Linux-based system, you can combine the `zip` and `docker run` steps in a single command:

   ```
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
{: shortdesc}

Create an archive that contains your source files. The source file that contains the entry point must be named `main.rb`.

```bash
zip -r helloRuby.zip main.rb helper.rb
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
{: shortdesc}

Before you begin, [review the packages that are included with the PHP runtime](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php) to see whether a dependency of your app is already included with the runtime. If your dependency is not included, you must package it with your app.

To package your app, run the following command.

```bash
zip -r <archive_name>.zip <file_1>.php <file_2>.php
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
{: shortdesc}

### Structuring Java code
{: #prep_java_struct}

A Java action is a Java program with a method called `main`. `main` must have the following signature.
{: shortdesc}

**Example**

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* You must specify the name of the main class by using `--main`. An eligible main class is one that implements a static `main` method. If the class is not in the default package, use the Java fully qualified class name, for example, `--main com.example.MyMain`.
* You can customize the method name of your Java action by specifying the fully qualified method name of your action, for example, `--main com.example.MyMain#methodName`.

### Packaging Java code
{: #prep_java_pkg}

Package your code by creating a .jar file.
{: shortdesc}

**Before you begin**

You must have [JDK 8](http://openjdk.java.net/install/){: external} installed locally. This example uses the [`google-gson-2.8.5.jar`](https://mvnrepository.com/artifact/com.google.code.gson/gson/2.8.5){: external}.

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

2. Download the [`gson-2.8.5.jar`](https://mvnrepository.com/artifact/com.google.code.gson/gson/2.8.5){: external}.

3. Add the `gson-2.8.5.jar` to your `ClASSPATH`. This example uses `gson-2.8.5.jar`, which is saved in a `test` folder in the `Desktop` directory.
  
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
{: shortdesc}

### Structuring .NET Core code
{: #prep_dotnet_struct}

A .NET Core action is a .NET Core class library with a method that is expected to be named `Main`. If the method in your code is not `Main`, take note of the name to specify it when the action is created in the format: `--main {Assembly}::{Class Full Name}::{Method}`
{: shortdesc}

**Example**

```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### Packaging .NET Core code
{: #prep_dotnet_pkg}

Package your code by first compiling it and then compressing the results.
{: shortdesc}

**Before you begin**
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

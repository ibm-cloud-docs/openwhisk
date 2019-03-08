---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: actions, serverless, python

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


# Creating Python actions
{: #creating-python-actions}

The following sections guide you through creating and invoking a single Python action and adding parameters to that action.

## Creating and invoking a Python action
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
  * The entry method for the action is `main` by default but can be specified to create the action with the `wsk` CLI by using the `--main` flag.

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

    The type of action is determined by using the source file extension.
    For `.py` source files, the action runs by using a Python 2 runtime.

You can also create an action that runs with Python 3.x, for Python 3.6 use the parameter `--kind python:3.6` (previously named `python-jessie:3`), for Python 3.7 use the parameter `--kind python:3.7`, both contain additional packages for IBM Cloud Services like {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}}, and {{site.data.keyword.ibmwatson_notm}}. For more information about the packages  that are included in these Python 3 runtimes, see the Python runtime [reference](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_ref_python_environments).

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

## Packaging Python actions in zip files
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

You can package a Python action and dependent modules in a zip file. For example, you can create an action with a helper module called `helper.py`.

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

## Packaging Python actions with a virtual environment in zip files
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

You can package Python dependencies by using a virtual environment, `virtualenv`. The virtual environment allows you to link additional packages that can be installed by using [`pip` ![External link icon](../icons/launch-glyph.svg "External link icon")](https://packaging.python.org/installing/), for example.

You can install dependencies, package them in a virtual environment, and create a compatible OpenWhisk action.

1. Create a [requirements.txt ![External link icon](../icons/launch-glyph.svg "External link icon")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) file that contains the `pip` modules and versions to install.

2. Install the dependencies and create a virtual environment. The virtual environment directory must be named `virtualenv`. To ensure compatibility with the OpenWhisk runtime container, package installations inside a virtual environment must use the image that corresponds to the kind.

    * For kind `python:3.7` use the Docker image `ibmfunctions/action-python-v3.7`.
    * For kind `python:3.6` use the Docker image `ibmfunctions/action-python-v3.6`.
    * For kind `python:2` use the Docker image `openwhisk/python2action`.

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
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

## Reducing the size of a Python app.
{: #reducing_python_app_size}
To reduce the code size of a Python app.

1. Place the library `opencv-python` into `opencv`.
2. Install the opencv binary into the OS image.
3. Augment the image with more Python libraries by running `pip install requirements.txt`.
4. Use `action.py` with the new image.

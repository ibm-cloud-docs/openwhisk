---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

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


# 创建 Python 操作
{: #creating-python-actions}

以下各部分将指导您创建并调用单个 Python 操作，然后向该操作添加参数。

## 创建并调用 Python 操作
{: #openwhisk_actions_python_invoke}

操作其实就是顶级 Python 函数。要创建 Python 操作，请执行以下操作：

1. 将以下代码保存在名为 `hello.py` 的文件中。
```
python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
        return {"greeting": greeting}
```
{: codeblock}
    
Python 操作始终会使用一个字典并生成一个字典。缺省情况下，操作的入口方法为 `main`，但使用 `wsk` CLI 通过 `--main` 标志来创建操作时，可指定入口方法。
{: note}

2. 创建 `helloPython` 操作。
    操作类型使用源文件扩展名来确定。对于 `.py` 源文件，操作会使用 Python 2 运行时运行。

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    示例输出：

    ```
ok: created action helloPython
    ```
    {: screen}

3. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    示例输出：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    还可以创建使用 Python 3.x 运行的操作。 
    * 对于 Python 3.6，请使用 `--kind python:3.6` 参数（先前名为 `python-jessie:3`）。
    * 对于 Python 3.7，请使用 `--kind python:3.7` 参数。
    
    这两种运行时都包含用于 IBM Cloud 服务（如 {{site.data.keyword.cloudant_short_notm}}、{{site.data.keyword.Db2_on_Cloud_long_notm}}、{{site.data.keyword.cos_full_notm}} 和 {{site.data.keyword.ibmwatson_notm}}）的其他包。
    
有关这些 Python 3 运行时中包含的包的更多信息，请参阅 Python [运行时参考](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)。

## 将 Python 操作打包成 zip 文件
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

您可以将 Python 操作和从属模块打包成 zip 文件。例如，可以创建使用名为 `helper.py` 的帮助程序模块的操作。

1. 创建包含源文件的归档。**注**：包含入口点的源文件必须命名为 `__main__.py`。
    

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. 创建操作。

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## 使用虚拟环境将 Python 操作打包成 zip 文件
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

可以使用虚拟环境 `virtualenv` 来打包 Python 依赖项。虚拟环境允许您链接其他包，例如可以使用 [`pip` ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://packaging.python.org/installing/) 安装的包。

可以安装依赖项，在虚拟环境中对其打包，然后创建兼容的 OpenWhisk 操作。

1. 创建包含要安装的 `pip` 模块和版本的 [requirements.txt ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 文件。

2. 安装依赖项并创建虚拟环境。虚拟环境目录必须命名为 `virtualenv`。为了确保与 OpenWhisk 运行时容器的兼容性，虚拟环境内部的包安装必须使用与 kind 对应的映像。

    * 对于 kind `python:3.7`，请使用 Docker 映像 `ibmfunctions/action-python-v3.7`。
    * 对于 kind `python:3.6`，请使用 Docker 映像 `ibmfunctions/action-python-v3.6`。
    * 对于 kind `python:2`，请使用 Docker 映像 `openwhisk/python2action`。

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. 打包 `virtualenv` 目录和任何其他 Python 文件。包含入口点的源文件必须命名为 `__main__.py`。
    

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. 创建 `helloPython` 操作。
    

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

仅将不属于所选运行时环境的模块添加到 `requirements.txt` 中。这有助于使 `virtualenv` 的大小保持在最小。
{: tip}



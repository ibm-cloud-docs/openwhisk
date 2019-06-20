---

copyright:
years: 2017, 2019
lastupdated: "2019-05-15"

keywords: composer, openwhisk, compositions, sequence, branch

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

# Composer
{: #pkg_composer}

现在，{{site.data.keyword.openwhisk}} 支持将 Apache OpenWhisk 的 Composer 用作技术预览。Composer 使用其他组合子（[JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 和 [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)）来扩展 Apache OpenWhisk 序列，从而支持您创建包含条件分支、错误处理和循环的更复杂的流。[Composer Git 存储库](https://github.com/apache/incubator-openwhisk-composer)上提供了 Composer 的完整文档和技术规范。

您还可以使用开放式源代码项目 [Kui](https://github.com/ibm/kui) 更轻松地创建、部署和可视化 Composer 源代码。请参阅 [GitHub 上的 Kui](https://github.com/ibm/kui)，以获取有关如何将 Kui 与 Composer 配合使用的更多信息。
{: note}

## 安装用于 JavaScript 或 Python 3 的 Composer 库
{: #install_composer}

可以将 Composer 用于使用任何语言编写的操作，但只能使用 JavaScript 或 Python 3 来表达组合。安装后，可以使用 Composer `compose/pycompose` 和 `deploy/pydeploy` 命令来[配置并运行组合](#run)。
{: shortdesc}

**对于 JavaScript**：
1. 使用 Node Package Manager 安装 Composer 库 [Node.js 包](https://github.com/apache/incubator-openwhisk-composer)。

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  通过运行 Composer 命令的帮助，从而确认是否已安装该库。

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    示例输出：
      ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**对于 Python 3**：使用 `pip3` 来安装[用于 Python 3 的 Composer](https://github.com/apache/incubator-openwhisk-composer-python) 库。

1.  克隆用于 Python 3 的 Composer GitHub 存储库。
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  导航至 composer 目录。
    ```
    cd composer-python
    ```
    {: pre}
3.  安装 Composer 库。请包含句点 (`.`)，以便命令可在您所在的目录中查找该库。
    ```
    pip3 install -e .
    ```
    {: pre}
4.  通过运行 Composer 命令的帮助，从而确认是否已安装该库。
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    示例输出：
      ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## 在 IBM Cloud Functions 中配置和运行组合
{: #run}

可以使用 JavaScript 或 Python 3 Composer 库在 {{site.data.keyword.openwhisk}} 中创建组合。使用 `compose` 或 `pycompose` 来编译组合源代码，然后使用 `deploy` 或 `pydeploy` 将组合部署到 {{site.data.keyword.openwhisk}}。配置组合后，可以在 {{site.data.keyword.openwhisk}} 中运行该组合。
{: shortdesc}

**开始之前**：
缺省情况下，部署会使用 `~/.wskprops` 中设置的值。通过将两个参数设置为 Composer `deploy` 或 `pydeploy` 命令的输入，可覆盖缺省值。

1.  将 API 主机设置为 {{site.data.keyword.openwhisk}} 端点。
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  添加 `wsk` CLI 认证密钥。
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**要运行组合**，请执行以下操作：

1.  使用 NodeJS 或 Python 3 库创建 Composer 源代码。例如，创建 `demo.js` 文件。
2.  将 Composer 源代码编译成 JSON 文件。
    *   使用 JavaScript：
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   使用 Python 3：
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  将代码部署到 {{site.data.keyword.openwhisk}}。
    *   使用 JavaScript：包含 `-w` 标志以覆盖名为 `demo` 的任何现有部署。
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   使用 Python 3：包含 `-w` 标志以覆盖名为 `demo` 的任何现有部署。
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  执行组合的方式与在 {{site.data.keyword.openwhisk}} 中[调用其他操作](/docs/openwhisk?topic=cloud-functions-triggers)的方式相同。
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} 会执行部署为特殊类型操作的代码。有关更多详细信息，请参阅有关[主导操作](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md)的文档。

## 使用 Composer 扩展序列
{: #extending}

Apache OpenWhisk 允许您通过 `sequence` 按顺序将函数链接在一起，其中一个操作的输出成为另一个操作的输入。

### 不使用 Composer 的序列
{: #sequences-without-composer}
可以在 {{site.data.keyword.openwhisk_short}} 中将名为 `action1` 和 `action2` 的两个函数链接在一起：

`ibmcloud fn action create --sequence mysequence action1 action2`。

此命令的结果是名为 `mysequence` 的函数，此函数是 `action1` 和 `action2` 的组合。您可以像使用 Openwhisk 中的任何函数一样来使用 `mysequence`。

### 使用 Composer 的序列
{: #sequences-with-composer}
在 Composer 中，可以使用源代码而不是命令行来指定更丰富的序列。

对于 JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

对于 Python 3：
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="简单序列" alt="具有两个操作的序列" style="width:250px; border-style: none"/></br> _图 1. 具有两个操作的序列_

在 Composer 中可以执行的操作并不局限于将函数链接在一起。Composer 包含基于 [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 或 [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md) 的组合子系列，用于增强序列的表达性。您可以在以下部分中查看常见示例。

### 错误处理
{: #error-handling}
可以使用 `try-catch-finally` 块向序列添加错误处理。在此示例中，将使用 try 包围序列。如果任一操作返回错误，都将执行 `handleError` 代码。

对于 JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

对于 Python 3：
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Try 序列" alt="具有错误处理的序列" style="width:400px; border-style: none"/></br>
_图 2. 具有错误处理的序列_

### 条件分支
{: #conditional-branch}
可以使用 `if-then-else` 来创建分支序列。此示例演示了 `if-then-else`。`action1` 应该返回布尔值。如果为 `true`，那么将执行 `action2`，否则将执行 `action3`。请注意，`action3` 是可选的，对于 `if-then`，可以将其省略。

对于 JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

对于 Python 3：
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="If 序列" alt="具有条件分支的序列" style="width:250px; border-style: none"/></br>
_图 3. 具有条件分支的序列_

### 循环
{: #loop}
可以在 Composer 中创建循环构造。在此示例中，只要 `action1` 返回 `true`，就会执行 `action2`。Composer 会限制可以在一个组合序列中执行的步骤总数。当前限制为 20。

对于 JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

对于 Python 3：
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="While 序列" alt="具有 while 循环的序列" style="width:250px; border-style: none"/></br> _图 4. 具有 `while` 循环的序列_

### 操作的内联定义
{: #inline-def}
可以在组合代码本身中定义操作。在此示例中，使用 `composer.action()` 创建名为 `hello` 的组合内联操作定义。

对于 JavaScript：
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

对于 Python 3：
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="While 序列" alt="具有内联操作定义的序列" style="width:250px; border-style: none"/></br>
_图 5. 具有内联操作定义的序列_

## 使用其他组合子定义
{: #combinator-def}
请参阅 Apache OpenWhisk 的 Composer（[JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 或 [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)）的文档，以获取组合子定义的完整列表。


---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 管理操作
{: #openwhisk_managing}

通过监视操作输出、获取有关操作的特定信息或删除操作来管理操作。
{: shortdec}

## 获取操作
{: #getting-actions}

创建操作后，可以获取有关操作详细信息的更多信息，并列出名称空间中的操作。
{: shortdesc}

要列出已创建的操作，请运行以下命令：
```
ibmcloud fn action list
```
{: pre}

随着您创建的操作越来越多，将相关操作分组成[包](./openwhisk_packages.html)会非常有用。要过滤操作列表以只列出特定包中的操作，请运行以下命令：
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

要获取描述特定操作的元数据，请运行以下命令：

```
ibmcloud fn action get hello
```
{: pre}

示例输出：
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

<table>
<caption>了解 <code>action get</code> 命令输出</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>action get</code> 命令输出</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>此操作所在的名称空间。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>操作的名称。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>操作的语义版本。</td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code>：操作的类型。可能的值为 nodejs:6、nodejs:8、php:7.1、python:3、python-jessie:3、swift:3.1.1、swift:4.1、java、blackbox 和 sequence。</li>
<li><code>code</code>：kind 为 nodejs 或 swift 时要执行的 Javascript 或 Swift 代码。</li>
<li><code>components</code>：kind 为 sequence 时序列中的操作。这些操作按顺序列出。</li>
<li><code>image</code>：kind 为 blackbox 时的容器映像名称。</li>
<li><code>init</code>：kind 为 nodejs 时的可选 zip 文件引用。</li>
<li><code>binary</code>：操作是否编译成二进制可执行文件。</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>有关此操作的注释。要获取可能的注释的列表，请参阅[操作注释](openwhisk_annotations.html#action)和 [Web 操作注释](openwhisk_annotations.html#annotations-specific-to-web-actions)参考主题。</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>：为操作设置的超时（以毫秒为单位），在此时间后操作会终止。缺省值：6000</li>
<li><code>memory</code>：为操作设置的最大内存限制（以 MB 为单位）。缺省值：256</li>
<li><code>logs</code>：为操作设置的最大日志限制（以 MB 为单位）。缺省值：10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>操作是否公开发布。</td>
</tr>
</tbody></table>

## 查看激活详细信息
{: #activation}

{{site.data.keyword.openwhisk_short}} 操作可以由其他用户调用、响应各种事件或作为操作序列的组成部分。每当调用操作时，都会为该调用创建激活记录。要获取有关操作调用结果的信息，您可以获取有关激活的详细信息。

要获取名称空间中的所有激活记录标识，请运行以下命令：
```
ibmcloud fn activation list
```
{: pre}

要获取有关操作调用生成的特定激活记录的详细信息，请运行以下命令：
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

示例输出：
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "BobsOrg_dev",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
  },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "BobsOrg_dev/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
  },
        {
            "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>了解 <code>activation get</code> 命令输出</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>activation get</code> 命令输出</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>此激活所在的名称空间。这可能与操作所在的名称空间不同。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>操作的名称。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>操作的语义版本。</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>激活该项的用户帐户。</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>此激活记录的标识。</td>
</tr>
<tr>
<td><code>start</code></td>
<td>激活的开始时间。</td>
</tr>
<tr>
<td><code>end
</code></td>
<td>激活的结束时间。</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>完成激活所用时间（以毫秒为单位）。</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>：激活的出口状态。</li>
<li><code>statusCode</code>：状态码。如果操作出错，将为 HTTP 错误代码。</li>
<li><code>success</code>：操作是否成功完成。</li>
<li><code>result</code>：从激活返回的值。</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td><td>此激活的日志。</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>有关此操作的注释。有关可能的激活注释的列表，请参阅[注释参考主题](openwhisk_annotations.html#activation)。</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>操作是否公开发布。</td>
</tr>
</tbody></table>

## 访问操作体中的操作元数据
{: #accessing-action-metadata-within-the-action-body}

操作环境包含多个特定于运行中操作的属性。这些属性允许操作以编程方式通过 REST API 来使用 OpenWhisk 资产，或者设置在操作即将耗尽其分配的时间预算时发出的内部警报。使用 OpenWhisk Docker 框架时，这些属性可供系统环境中所有受支持运行时进行访问：Node.js、Python、Swift、Java 和 Docker。

|属性|描述
|
| -------- | ----------- |
|`__OW_API_HOST`|运行此操作的 OpenWhisk 部署的 API 主机。|
|`__OW_API_KEY`|调用此操作的主体的 API 密钥，此密钥可能是受限制的 API 密钥。|
|`__OW_NAMESPACE`|激活的名称空间。此名称空间可能与操作的名称空间不同。|
|`__OW_ACTION_NAME`|运行中操作的标准名称。|
|`__OW_ACTIVATION_ID`|此运行中操作实例的激活标识。|
|`__OW_DEADLINE`|此操作将耗尽整个持续时间配额时的近似时间（毫秒，以纪元格式表示）。|

## 获取操作 URL
{: #get-action-url}

可以通过 HTTPS 请求使用 REST 接口来调用操作。
{: shortdesc}

要获取操作 URL，请运行以下命令：
```
ibmcloud fn action get actionName --url
```
{: pre}

标准操作的示例输出：
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

[Web 操作](./openwhisk_webactions.html)的示例输出：
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**注：**对于标准操作，当通过 HTTPS 请求进行调用时，必须提供认证。有关使用 REST 接口调用操作的更多信息，请参阅 [REST API 参考](https://console.bluemix.net/apidocs/openwhisk)。


## 保存操作代码
{: #save-action}

您可以获取与现有操作关联的代码，并将其保存在本地。可以保存除序列和 Docker 操作以外的其他所有操作的代码。
{: shortdesc}

将操作码保存到当前工作目录中与现有操作名称相对应的文件名。
```
ibmcloud fn action get actionName --save
```
{: pre}

将使用与操作类型相对应的文件扩展名。对于作为 zip 文件的操作码，将使用扩展名 .zip。示例输出：
```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
{: screen}

可以改为使用 `--save-as` 标志来提供定制文件路径、文件名和扩展名。
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

示例输出：
```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
{: screen}

## 监视操作日志
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}} 操作可以由其他用户调用、响应各种事件或作为操作序列的组成部分。要获取有关调用操作的时间和输出内容的信息，监视操作日志会非常有用。

可以使用 {{site.data.keyword.openwhisk_short}} CLI 在调用操作时监视其输出。

1. 启动轮询循环，以持续检查从激活生成的日志。
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. 切换到其他窗口，并调用操作。
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    示例输出：
    ```
      ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```
    {: screen}

3. 在轮询窗口中，可以查看激活日志。
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
    {: screen}
    您还可以在 {{site.data.keyword.openwhisk_short}} 中实时查看 OpenWhisk 中代表您运行的任何操作的日志。

## 删除操作
{: #deleting-actions}

可以通过删除不想使用的操作来进行清理。

1. 删除操作。
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    示例输出：
    ```
      ok: deleted hello
  ```
    {: screen}

2. 验证该操作是否不再出现在操作列表中。
    ```
    ibmcloud fn action list
    ```
    {: pre}

    示例输出：
    ```
      actions
  ```
    {: screen}

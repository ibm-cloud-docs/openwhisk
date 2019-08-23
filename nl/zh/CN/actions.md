---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, functions, serverless, javascript, node, node.js

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# 创建操作
{: #actions}

创建操作，这是返回 JSON 对象的顶级函数。可以将操作合并到一个包中，以简化对操作的管理。
{: shortdesc}

开始之前：
要创建操作，源代码必须满足特定需求。例如，如果要通过包含在多个文件中的代码创建操作，请在创建操作之前，先将代码打包成单个文件。有关每个运行时的需求的详细信息，请参阅[为无服务器准备应用程序代码](/docs/openwhisk?topic=cloud-functions-prep)。


## 通过 CLI 创建操作
{: #actions_cli}

1. 创建操作。
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  **示例**
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  **示例输出**

  ```
  ok: created action hello
  ```
  {: screen}

  提示：
  - 为了节省成本，您可以设置限制。
      - 要为内存使用量设置限制，请在 create 命令中包含 `--memory VALUE`，其中值以兆字节为单位。
      - 要设置超时，请在 create 命令中包含 `--timeout VALUE`，其中值以毫秒为单位。
  - 如果已将代码打包成 Docker 映像，请在 create 命令中包含 `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG`，而不是包含应用程序的本地路径和 --kind 标志。要妥善管理映像，请尽可能不使用 `latest` 标记。使用 `latest` 标记时，将使用带有该标记的映像，但这不一定是最近创建的映像。        

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  
2. 验证操作是否在操作列表中。

  ```
  ibmcloud fn action list
  ```
  {: pre}

  **示例输出**

  ```
  actions
  hello       private
  ```
  {: screen}


## 通过操作更新应用程序或运行时
{: #actions_update}

每当需要更新应用程序中的代码或迁移到更高版本的运行时，都可以运行 update 命令。例如，由于 Node.js V8 处于维护方式，因此您可能希望将运行时切换为 Node.js 10。

迁移到新的运行时版本时，可能需要更改应用程序中的代码以符合新的运行时版本。在大多数情况下，运行时版本是兼容的。
{: tip}

1. 本地更新应用程序。

2. 如果已将应用程序打包成 Docker 映像，请将最新映像上传到 Docker Hub。这将允许系统在下次运行操作的代码时，拉取新的 Docker 映像。
    如果有使用前版 Docker 映像的运行中容器，那么任何新调用都将继续使用该映像。必须运行 update 命令，这样新调用才能开始在新映像上运行。

3. 更新操作并包含应用程序或 Docker 映像的本地路径。
    

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    **示例**

    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    **示例输出**

    ```
    ok: updated action hello
    ```
    {: screen}

    如果已将代码打包成 Docker 映像，请在 create 命令中包含 `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG`，而不是包含本地应用程序的路径和 `--kind` 标志。要妥善管理映像，请尽可能不使用 `latest` 标记。使用 `latest` 标记时，将使用带有该标记的映像，但这不一定是最近创建的映像。 

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## 将参数绑定到操作
{: #actions_params}

可以将参数绑定到操作来设置缺省参数。绑定的参数会充当操作的缺省参数，除非在调用时提供了参数。
{: shortdesc}

开始之前，请[创建操作](#actions_cli)。

要绑定参数，请执行以下操作：

1. 更新操作并将缺省参数绑定到该操作。

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **示例**

    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    **示例输出**

    ```
    ok: updated action MyApp
    ```
    {: screen}

    如果修改了非服务凭证参数，那么运行带新参数的 `action update` 命令将除去当前存在但未在 `action update` 命令中指定的任何参数。例如，如果运行 `action update -p key1 new-value -p key2 new-value`，但省略了已设置的其他任何参数，那么更新操作后这些参数将不再存在。此外，还会除去已绑定到操作的任何服务。如果已绑定服务，那么必须再次[将服务绑定到操作](/docs/openwhisk?topic=cloud-functions-services)。
    {: tip}

3. 验证参数是否已绑定到操作。
    

    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    **示例输出**

    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

可选：要清除先前绑定的参数，请在不包含任何参数的情况下更新操作。

```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## 将操作链接在一起作为操作序列
{: #actions_seq}

可以创建用于将一序列操作链接在一起的操作。一个操作的结果将作为自变量传递给下一个操作。
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

在序列中的操作之间传递的参数为显式参数，但缺省参数除外。因此，传递到操作序列的参数只可用于序列中的第一个操作。序列中第一个操作的结果成为序列中第二个操作的输入 JSON 对象，依此类推。此对象不包含初始传递到序列的任何参数，除非第一个操作在其结果中包含这些参数。操作的输入参数会与操作的缺省参数合并，并且操作的输入参数优先于并覆盖任何匹配的缺省参数。

序列的总体超时会受该序列中各个操作的超时的影响。因为序列是操作的管道，所以一个操作发生故障会使管道中断。如果一个操作超时，那么将退出整个序列，并返回该故障。

接下来，创建规则或调用操作时，请使用序列的名称。


## 打包操作
{: #actions_pkgs}

在 {{site.data.keyword.openwhisk}} 中，可以使用包将一组相关操作和订阅源捆绑在一起，然后与其他人共享。包还允许在包中的所有实体之间共享参数。
{: shortdesc}

包可以包含*操作*和*订阅源*。
- 操作是在 {{site.data.keyword.openwhisk_short}} 上运行的一段代码。例如，{{site.data.keyword.cloudant}} 包中包含用于在 {{site.data.keyword.cloudant_short_notm}} 数据库中读写记录的操作。
- 订阅源用于配置外部事件源以触发触发器事件。例如，“警报”包中包含可按指定频率触发触发器的订阅源。


1. 创建包。

  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. 获取包的摘要。请注意，该包为空。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **示例输出**

  ```
  package /myNamespace/custom
  ```
  {: screen}

4. 创建操作并将其包含在包中。在包中创建操作需要将包名添加为操作名称的前缀。不允许包嵌套。包只能包含操作，而不能包含其他包。

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. 获取包的摘要。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **示例输出**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## 将参数绑定到包
{: #actions_pkgs_params}

您可通过设置由包中所有操作继承的包级别参数，设置用于该包中所有实体的缺省参数。

绑定的参数充当包中操作的缺省参数，但以下情况例外：

- 操作本身具有缺省参数
- 操作具有在调用时提供的参数

开始之前，请创建至少包含一个操作的包。

1. 更新包并将缺省参数绑定到该包。

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **示例**

    ```
  ibmcloud fn package update MyApp --param name World
  ```
    {: pre}

    **示例输出**

    ```
    ok: updated package MyApp
    ```
    {: screen}

    如果修改了非服务凭证参数，那么运行带新参数的 `package update` 命令将除去当前存在但未在 `package update` 命令中指定的任何参数。例如，如果运行 `package update -p key1 new-value -p key2 new-value`，但省略了已设置的其他任何参数，那么更新包后这些参数将不再存在。还会除去已绑定到包的任何服务，因此在更新其他参数后，必须再次[将服务绑定到包](/docs/openwhisk?topic=cloud-functions-services)。
  {: tip}

3. 验证参数是否已绑定到包。
    

    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    **示例输出**

    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. 验证参数是否已由包继承。
    

    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    **示例输出**

    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## 共享操作包
{: #actions_pkgs_share}

调试并测试组成一个包的操作和订阅源之后，该包可以与所有 {{site.data.keyword.openwhisk_short}} 用户共享。通过共享包，用户可以绑定包，调用包中的操作，以及编写 {{site.data.keyword.openwhisk_short}} 规则和序列操作。共享包中的操作和订阅源是_公共_的。如果包是私有的，那么其所有内容也是私有的。
{: shortdesc}

1. 与所有用户共享包。

  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. 显示包的 `publish` 属性以验证其现在是否为 true。

  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  **示例输出**

  ```
  ok: got package PACKAGE_NAME, displaying field publish

    true
  ```
  {: screen}

3. 获取包的描述，以向他人提供包的标准名称，以便可以绑定包或调用包中的操作。标准名称包含名称空间，在此示例中为 `myNamespace` 名称空间。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **示例输出**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## 操作的环境变量
{: #actions_envvars}

操作环境包含多个特定于运行中操作的环境变量。这些属性可供系统环境中所有受支持运行时进行访问。这些属性允许操作以编程方式通过 REST API 来使用资产，或者设置在操作即将耗尽其分配的时间预算时发出的内部警报。
{: shortdesc}

|属性|描述|
| -------- | ----------- |
|`__OW_API_HOST`|运行此操作的部署的 API 主机。|
|`__OW_API_KEY`|调用此操作的主体的 API 密钥。此变量仅针对基于经典 CF 的名称空间提供。|
|`__OW_NAMESPACE`|名称空间标识 (GUID)。对于基于经典 CF 的名称空间，此标识由组织和空间名称构成。|
|`__OW_NAMESPACE_CRN`|名称空间的云资源名称 [CRN](/docs/overview?topic=overview-crn)。CRN 仅可用于启用 IAM 的名称空间。
|`__OW_ACTION_NAME`|运行中操作的标准名称。|
|`__OW_IAM_NAMESPACE_API_KEY`|启用 IAM 的名称空间的 API 密钥。请参阅[设置访问策略](/docs/openwhisk?topic=cloud-functions-namespaces#namespace-access)以获取用法。|
|`__OW_IAM_API_URL`|用于 IAM 操作（例如，从 API 密钥获取令牌）的服务端点。此变量仅可用于启用 IAM 的名称空间。|
|`__OW_ACTIVATION_ID`|此运行中操作实例的激活标识。|
|`__OW_DEADLINE`|此操作将耗尽整个持续时间配额时的近似时间（毫秒，以纪元格式表示）。|

### 在应用程序中包含操作环境变量
{: #actions_envvars_app}

要查看某个操作的值，请在应用程序代码中包含这些值的显示，并将其输出到结果中。

**针对 Python 的示例**
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

更新并激活操作中的代码后，结果会包含该操作的标准名称。
```bash
"response": {
    "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
{: screen}




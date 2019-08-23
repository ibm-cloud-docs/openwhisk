---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# 安装 CLI 和插件
{: #cli_install}

{{site.data.keyword.openwhisk}} 为 {{site.data.keyword.cloud_notm}} CLI 提供了功能强大的插件，支持对 {{site.data.keyword.openwhisk_short}} 系统进行全面管理。
可以使用 {{site.data.keyword.openwhisk_short}} CLI 插件来管理操作中的代码片段，创建触发器和规则以支持操作对事件进行响应以及将操作捆绑到包中。
{:shortdesc}


## 设置 {{site.data.keyword.cloud_notm}} CLI
{: #cli_setup}

**开始之前**

您必须创建 [{{site.data.keyword.cloud_notm}} 帐户](https://cloud.ibm.com/){: external}。

下载并安装 {{site.data.keyword.cloud_notm}} CLI，然后登录。
{: shortdesc}

1. 下载并安装 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli)。

2. 登录到 {{site.data.keyword.cloud_notm}} CLI。要指定 {{site.data.keyword.cloud_notm}} 区域，请[包含 API 端点](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)。

  ```
  ibmcloud login
  ```
  {: pre}

3. 按照提示选择您的 {{site.data.keyword.cloud_notm}} 帐户。

4. 获取资源组的列表。 

```
ibmcloud resource groups
```
{: pre}

**示例输出**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. 可选：通过运行以下命令将非缺省资源组设定为目标。
```
ibmcloud target -g <resource_group>
```
{: pre}


**示例输出**

```
Targeted resource group <resource_group>
```
{: screen}

## 设置 {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cli_plugin_setup}

要使用 {{site.data.keyword.openwhisk_short}}，请下载并安装 CLI 插件。
{: shortdesc}

可以使用 {{site.data.keyword.openwhisk_short}} CLI 插件来执行以下任务。

* 在 {{site.data.keyword.openwhisk_short}} 上运行代码片段或操作。请参阅[创建和调用操作](/docs/openwhisk?topic=cloud-functions-actions)。
* 创建触发器和规则以支持操作对事件进行响应。请参阅[创建触发器和规则](/docs/openwhisk?topic=cloud-functions-triggers)。
* 捆绑操作以及配置外部事件源。请参阅[创建和使用包](/docs/openwhisk?topic=cloud-functions-pkg_ov)。
* 浏览包的目录，并通过外部服务来增强应用程序的功能。请参阅[添加 {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services)。

完成以下步骤来安装 {{site.data.keyword.openwhisk_short}} CLI 插件

1. 安装 {{site.data.keyword.openwhisk_short}} 插件。
    

  ```
    ibmcloud plugin install cloud-functions
    ```
  {: pre}

2. 验证插件是否已安装。
    

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **输出**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. 登录后，所有 {{site.data.keyword.openwhisk_short}} 命令都以 `ibmcloud fn` 开头。要查看可以使用 {{site.data.keyword.openwhisk_short}} 插件执行的所有操作，请运行不带自变量的 `ibmcloud fn`。

  ```
  ibmcloud fn
  ```
  {: pre}




## 将 {{site.data.keyword.openwhisk_short}} 名称空间设定为目标
{: #cli_regions}
缺省情况下，{{site.data.keyword.openwhisk_short}} 使用[支持 IAM 的名称空间](/docs/iam?topic=iam-iamoverview){: external}。您无法再创建基于 Cloud Foundry 的名称空间。
{: important}

### 创建名称空间或将名称空间设定为目标。
要获取 {{site.data.keyword.openwhisk_short}} 名称空间的列表，请运行 `ibmcloud fn namespace list`。

#### 创建启用 IAM 的名称空间。
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**响应**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### 将启用 IAM 的名称空间设定为目标。 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**响应**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### 将基于 Cloud Foundry 的名称空间设定为目标。 
  
可以使用 `-o` 和 `-s` 标志将特定 `org` 和 `space` 设定为目标，也可以按照提示进行操作。

* 通过在 `target` 命令中包含 `org` 和 `space` 名称，将 Cloud Foundy 名称空间设定为目标。

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* 将 Cloud Foundry 设定为目标，并按照提示选择 `org` 和 `space`。

```
ibmcloud target --cf
```
{: pre}


**响应**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### 可选：创建用于编译打包和生产部署的名称空间。

可以通过为每个预生产（编译打包）和生产 {{site.data.keyword.openwhisk_short}} 部署创建启用 IAM 的名称空间来处理这些部署。运行 [`ibmcloud fn namespace`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) 以在您的组织下创建更多名称空间（例如“staging”和“production”）：

创建编译打包名称空间。
```
ibmcloud fn namespace create staging
```
{: pre}

创建生产名称空间。
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} 对名称空间的名称施加了限制。有关更多信息，请参阅[系统详细信息和限制](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)文档。
{: tip}


## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS 代理
{: #cli_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以设置为使用 HTTPS 代理。要设置 HTTPS 代理，必须创建名为 `HTTPS_PROXY` 的环境变量。该变量必须设置为 HTTPS 代理的地址及其端口，格式如下：`{PROXY IP}:{PROXY PORT}`。

更改 `org` 或 `space` 的名称将基于更改后的名称创建新的名称空间。旧名称空间中的实体不会在新名称空间中显示，并且可能会被删除。
{: important}


## 从 OpenWhisk CLI 迁移到 {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cli_migrate}

现在，可以使用 {{site.data.keyword.openwhisk_short}} CLI 插件与 {{site.data.keyword.openwhisk_short}} 实体进行交互。虽然可以继续使用独立 Openwhisk CLI，但此 CLI 不会有 {{site.data.keyword.openwhisk_short}} 支持的最新功能，例如基于 IAM 的名称空间和 `service bind`。
{: shortdesc}


### 命令语法
{: #cli_syntax}

Cloud Functions CLI 插件中命令的所有命令选项和自变量都与 [OpenWhisk 独立 CLI ](https://github.com/apache/incubator-openwhisk-cli){: external} 的选项相同。但是，请注意以下差异。

* {{site.data.keyword.openwhisk}} 插件会自动利用您当前的登录和目标信息。
* 现在，`wsk` 作为 `ibmcloud fn` 运行。
* 不再需要 `wsk ibmcloud login` 命令。您可以使用 `ibmcloud login` 进行登录。
* 现在，可以使用 `ibmcloud fn api` 来管理 API。

有关更多信息，请参阅 [{{site.data.keyword.openwhisk_short}} CLI 参考](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli)。

### API 认证和主机
{: #cli_api_auth}

使用 {{site.data.keyword.openwhisk_short}} CLI 插件时，您无需显式配置 API 密钥和 API 主机。您可以改为使用 `ibmcloud login` 登录。然后，通过运行 `ibmcloud fn property set --namespace <namespace_name>` 将启用 IAM 的名称空间设定为目标，或通过运行 `ibmcloud target --cf` 将基于 Cloud Foundry 的名称空间设定为目标。登录后，所有命令都以 `ibmcloud fn` 开头。


如果需要将认证 API 密钥用于外部 HTTP 客户机（例如，cURL 或 Postman）中的 {{site.data.keyword.openwhisk_short}}，可以使用以下命令来检索该密钥。

通过运行以下命令来获取当前 API 密钥。
```
ibmcloud fn property get --auth
```
{: pre}

通过运行以下命令来获取当前 API 主机。
```
ibmcloud fn property get --apihost
```
{: pre}

API 密钥是特定于 {{site.data.keyword.openwhisk_short}} CLI 插件设定为目标的每个区域、组织和空间的。
{: tip}


### API 网关认证
{: #cli_apigw_authentication}

OpenWhisk CLI 需要您运行 `wsk ibmcloud login` 才能配置 API 网关授权，以使用 `wsk api` 命令来管理 API。使用 {{site.data.keyword.openwhisk_short}} CLI 插件后，无需运行 `wsk ibmcloud login`。取而代之的是，使用 `ibmcloud login` 命令登录到 {{site.data.keyword.cloud_notm}} 时，{{site.data.keyword.openwhisk}} 插件会自动利用您当前的登录和目标信息。现在，您可以使用 `ibmcloud fn api` 命令来管理 API。


### 迁移部署脚本
{: #cli_migrating_deploy_scripts}

如果您具有将 OpenWhisk CLI 与 `wsk` 命令一起使用的脚本，那么所有命令的工作方式与使用 `ibmcloud fn` 命令相同。您可以修改脚本以使用 {{site.data.keyword.cloud_notm}} CLI 插件，或者创建别名或包装程序，以便使用 `wsk` 的当前命令转换为 `ibmcloud fn`。{{site.data.keyword.cloud_notm}} CLI 中的 `ibmcloud login` 和 `ibmcloud target` 命令以无人照管方式工作。在无人照管方式下，可以先配置环境，然后再运行 `ibmcloud fn` 命令来部署和管理 {{site.data.keyword.openwhisk_short}} 实体。

## CLI 版本历史记录
{: #cli_versions}

版本的历史记录，其中显示了亮点和错误修订。

V1.0.30（2019 年 4 月 3 日）
* 改进了 `service bind` 对 IAM 以及基于组织和空间的服务的处理。
* 针对处理 API 端点 https://cloud.ibm.com，添加了修订。

V1.0.29（2019 年 2 月 6 日）
* 添加了 `deploy` 和 `undeploy` 命令，用于通过清单文件部署或取消部署 Functions 实体的集合。有关更多信息，请参阅[部署](/docs/openwhisk?topic=cloud-functions-deploy#deploy)文档。

V1.0.28（2019 年 1 月 21 日）
* 添加了 `update|delete|get namespace name` 存在多次时的错误消息。

V1.0.27（2018 年 12 月 11 日）
* 添加了 `namespace get` 修订。
* 针对操作为黑匣操作时的 `--save-as`，添加了修订。

V1.0.26（2018 年 11 月 30 日）
* 启用了 `fn property get --auth` 以在新环境中正确返回认证密钥。

V1.0.25（2018 年 11 月 23 日）
* 改进了错误消息结果显示。
* 添加了 `fn namespace get` 修订以正确显示名称空间属性。

1.0.23（2018 年 10 月 15 日）
* 添加了对 ruby (`.rb`) 操作代码识别的支持。

1.0.22（2018 年 8 月 20 日）
* 添加了 us-east 区域支持。

1.0.21（2018 年 8 月 1 日）
* 别名 `fn` 和 `functions` 现在可用于 {{site.data.keyword.openwhisk_short}} 命令：`ibmcloud fn <command>` 和 `ibmcloud fn <command>`. 此外，您仍可以使用 `ibmcloud wsk <command>`.

1.0.19（2018 年 7 月 2 日）
* 小错误修订和改进。

1.0.18（2018 年 6 月 20 日）
* 针对取消绑定用户提供的服务实例，添加了修订。
* 改进了性能。

1.0.17（2018 年 6 月 12 日）
* 添加了对使用 `ibmcloud cf create-user-provided-service` 命令创建的用户提供的服务实例执行绑定 (`ibmcloud wsk service bind`) 和取消绑定 (`ibmcloud wsk service unbind`) 操作的支持。

1.0.16（2018 年 5 月 24 日）
* 小错误修订和改进。

1.0.15（2018 年 5 月 21 日）
* 小错误修订和改进。

1.0.14（2018 年 5 月 17 日）
* 在组织和空间名称中启用了对 `&` 字符的支持。

1.0.13（2018 年 5 月 7 日）
* 小错误修订和错误处理改进。

1.0.12（2018 年 4 月 30 日）
* {{site.data.keyword.cloud_notm}} SDK 更新，以保持 `bx` CLI 兼容性。

1.0.11（2018 年 4 月 23 日）
* 小错误修订和改进。

1.0.10（2018 年 4 月 9 日）
* 向 `ibmcloud wsk action create|update` 命令添加了新的 `--web-secure` 选项，用于保护 Web 操作端点。
* 修正了背靠背路径参数[缺陷](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external}。

1.0.9（2018 年 3 月 16 日）
* 在包级别启用了对服务绑定的支持。

1.0.8（2018 年 2 月 22 日）
* 启用了对 IAM 服务绑定的支持。

1.0.7（2018 年 2 月 2 日）
* 更新了 `ibmcloud wsk api` 以接受路径参数，例如 `/api/{id}`。有关更多信息，请参阅 [API 网关](/docs/openwhisk?topic=cloud-functions-apigateway)。
* 复原了代理支持。
* 除去了 `swift:3`。

1.0.6（2018 年 1 月 30 日）
* 修正了针对包内操作的 `ibmcloud wsk service bind` 命令的错误。




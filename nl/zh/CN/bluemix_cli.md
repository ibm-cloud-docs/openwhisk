---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 设置 {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} 为 {{site.data.keyword.Bluemix_notm}} CLI 提供了功能强大的插件，支持对 {{site.data.keyword.openwhisk_short}} 系统进行全面管理。
{: shortdesc}

## 设置 {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

下载并安装 {{site.data.keyword.Bluemix_notm}} CLI，然后登录。
{: shortdesc}

1. 下载并安装 [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)。

2. 登录到 {{site.data.keyword.Bluemix_notm}} CLI。{{site.data.keyword.openwhisk_short}} 在美国南部和英国 {{site.data.keyword.Bluemix_notm}} 区域中可用。如果未指定 {{site.data.keyword.Bluemix_notm}} API 端点，请使用 `-a` 标志指定 API 端点。

    * 登录到美国南部区域：
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * 登录到英国区域：
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  可以使用 `ibmcloud api` 命令来显式设置 {{site.data.keyword.Bluemix_notm}} API 端点。要显示当前的 API 端点设置，请使用 `ibmcloud target` 命令。
  {: tip}

3. `ibmcloud login` 命令会提示您输入信息，例如组织、空间和密码（如果未指定）。

  您可以在登录时指定组织和空间，以跳过输入这些信息的提示。请使用以下标志：`ibmcloud login -o<ORG> -s <SPACE>`.
  {: tip}

您还可以使用 {{site.data.keyword.Bluemix_notm}} API 密钥来登录。当您的帐户配置为使用联合登录（即要求您使用 `--sso` 标志登录）时，此方法会非常有用。如果要设置持续集成 (CI)，并且要配置无人照管的管道，那么[使用 API 密钥](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key)也非常有用。

1. 创建新的 API 密钥。
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. 使用生成的 API 密钥值来登录。
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
有关 `ibmcloud login` 命令的更多信息，请使用 `ibmcloud login --help` 或查看 [IBM Cloud (bx) 命令](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login)主题。

## 安装 {{site.data.keyword.openwhisk_short}} 插件
{: #cloudfunctions_plugin_setup}

下载并安装 {{site.data.keyword.openwhisk_short}} 插件。
{: shortdesc}

1. 安装 {{site.data.keyword.openwhisk_short}} 插件。
    ```
    ibmcloud plugin install cloud-functions
    ```
    {: pre}

2. 验证插件是否已安装。
    ```
    ibmcloud plugin list cloud-functions
    ```
    {: pre}

    输出：
    ```
    Plugin Name          Version
    Cloud-Functions      1.0.16
    ```
    {: screen}

3. 执行 `echo` 的阻塞性（同步）调用。将 `hello` 作为自变量传递。
    ```
    ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. 验证输出中是否返回了 `hello` 消息。
    示例输出：
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. 可以通过运行以下命令来升级 {{site.data.keyword.openwhisk_short}} 插件：
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

可以使用 {{site.data.keyword.openwhisk_short}} CLI 来执行以下操作：

* 在 {{site.data.keyword.openwhisk_short}} 上运行代码片段或操作。请参阅[创建和调用操作](./openwhisk_actions.html)。
* 使用触发器和规则以支持操作对事件进行响应。请参阅[创建触发器和规则](./openwhisk_triggers_rules.html)。
* 了解包如何捆绑操作以及配置外部事件源。请参阅[创建和使用包](./openwhisk_packages.html)。
* 浏览包的目录，并通过外部服务（例如，[{{site.data.keyword.cloudant}} 事件源](./openwhisk_cloudant.html)）来增强应用程序的功能。

要列出 {{site.data.keyword.openwhisk_short}} 插件的命令，请运行不带任何自变量的 `ibmcloud wsk`。
{: tip}

## 通过操作使用服务
{: #binding_services}

{{site.data.keyword.openwhisk_short}} 提供了 `service bind` 命令，使您的 {{site.data.keyword.Bluemix_notm}} 服务凭证在运行时可用于代码。然后，可以使用 `service bind` 命令将任何 {{site.data.keyword.Bluemix_notm}} 服务绑定到 {{site.data.keyword.openwhisk_short}} 中定义的任何操作。

有关如何通过操作使用服务的详细步骤，请参阅[将服务绑定到操作](./binding_services.html)。

## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS 代理
{: #cli_https_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以设置为使用 HTTPS 代理。要设置 HTTPS 代理，必须创建名为 `HTTPS_PROXY` 的环境变量。该变量必须设置为 HTTPS 代理的地址及其端口，格式如下：`{PROXY IP}:{PROXY PORT}`。

## 切换到不同的区域、组织和空间
{: #region_info}

如果您已经登录，那么可以在 {{site.data.keyword.Bluemix_notm}} CLI 中运行 `ibmcloud target` 命令来切换区域、组织和空间。

{{site.data.keyword.openwhisk_short}} 在美国南部和英国 {{site.data.keyword.Bluemix_notm}} 区域中可用。要更改区域，请使用 `ibmcloud target` 命令。例如，要切换到英国区域和该区域中的空间 `staging`，请运行以下命令：
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

可以通过为每个预生产（编译打包）和生产部署创建空间来处理这些部署。通过创建空间，{{site.data.keyword.openwhisk_short}} 可以具有为您定义的两个不同名称空间。运行 [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) 以在您的组织下创建更多空间（例如“staging”和“production”）：

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} 对名称空间的名称施加了限制。有关更多信息，请参阅[系统详细信息和限制](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities)文档。
{: tip}

## 从 OpenWhisk CLI 迁移到 {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cli_migration}

引入 {{site.data.keyword.openwhisk_short}} CLI 插件后，不再需要 OpenWhisk 独立 CLI。

### 命令语法
{: #command_syntax}

除了不再需要的 `wsk bluemix login` 命令外，其他所有 `wsk` 命令的工作方式与使用 `ibmcloud wsk` 命令相同。所有命令选项和自变量也都相同。

### API 认证和主机
{: #api_authentication}

OpenWhisk CLI 需要配置认证 API 密钥和 API 主机。使用 {{site.data.keyword.openwhisk_short}} CLI 插件时，您无需显式配置 API 密钥和 API 主机。取而代之的是，您可以使用 `ibmcloud login` 登录，然后使用 `ibmcloud target` 命令将您的区域和名称空间设定为目标。登录后，所有命令都以 `ibmcloud wsk` 开头。

如果需要将认证 API 密钥用于外部 HTTP 客户机（例如，cURL 或 Postman）中的 {{site.data.keyword.openwhisk_short}}，可以使用以下命令来检索该密钥：

要获取当前 API 密钥，请运行以下命令：
```
ibmcloud wsk property get --auth
```
{: pre}

要获取当前 API 主机，请运行以下命令：
```
ibmcloud wsk property get --apihost
```
{: pre}

API 密钥是特定于 {{site.data.keyword.openwhisk_short}} CLI 插件设定为目标的每个区域、组织和空间的。
{: tip}

### API 网关认证
{: #apigw_authentication}

OpenWhisk CLI 需要您运行 `wsk bluemix login` 才能配置 API 网关授权，以使用 `wsk api` 命令来管理 API。使用 {{site.data.keyword.openwhisk_short}} CLI 插件后，无需运行 `wsk bluemix login`。取而代之的是，使用 `ibmcloud login` 命令登录到 {{site.data.keyword.Bluemix_notm}} 时，{{site.data.keyword.openwhisk}} 插件会自动利用您当前的登录和目标信息。现在，您可以使用 `ibmcloud wsk api` 命令来管理 API。

### 迁移部署脚本
{: #migrating_deploy_scripts}

如果您具有将 OpenWhisk CLI 与 `wsk` 二进制文件一起使用的脚本，那么所有命令的工作方式与使用 `ibmcloud wsk` 命令相同。您可以修改脚本以使用 {{site.data.keyword.Bluemix_notm}} CLI 插件，或者创建别名或包装程序，使当前使用 `wsk` 的命令转换为 `ibmcloud wsk`。{{site.data.keyword.Bluemix_notm}} CLI 中的 `ibmcloud login` 和 `ibmcloud target` 命令以无人照管方式工作。在无人照管方式下，可以先配置环境，然后再运行 `ibmcloud wsk` 命令来部署和管理 {{site.data.keyword.openwhisk_short}} 实体。

## 版本历史记录
{: #version_history}

版本的历史记录，其中显示了亮点和错误修订。

1.0.18（2018 年 6 月 20 日）
* 针对取消绑定用户提供的服务实例，进行了修订。
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
* {{site.data.keyword.Bluemix_notm}} SDK 更新，以保持 `bx` CLI 兼容性。

1.0.11（2018 年 4 月 23 日）
* 小错误修订和改进。

1.0.10（2018 年 4 月 9 日）
* 向 `ibmcloud wsk action create|update` 命令添加了新的 `--web-secure` 选项，用于保护 Web 操作端点。
* 修正了背靠背路径参数[缺陷](https://github.com/apache/incubator-openwhisk-cli/issues/237)。

1.0.9（2018 年 3 月 16 日）
* 在包级别启用了对服务绑定的支持。

1.0.8（2018 年 2 月 22 日）
* 启用了对 IAM 服务绑定的支持。

1.0.7（2018 年 2 月 2 日）
* 现在，`ibmcloud wsk api` 接受诸如 `/api/{id}` 之类的路径参数。有关信息，请参阅 [API 网关](./openwhisk_apigateway.html)。
* 恢复了代理支持。
* 除去了 `swift:3`。

1.0.6（2018 年 1 月 30 日）
* 修正了针对包内操作的 `ibmcloud wsk service bind` 命令的错误。

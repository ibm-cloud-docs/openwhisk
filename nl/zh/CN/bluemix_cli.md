---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} 为 {{site.data.keyword.Bluemix_notm}} CLI 提供了功能强大的插件，支持对 {{site.data.keyword.openwhisk_short}} 系统进行全面管理。
{: shortdesc}

## 设置 {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

下载并安装 [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)。

## 安装 {{site.data.keyword.openwhisk_short}} 插件
{: #cloudfunctions_plugin_setup}

要安装 {{site.data.keyword.openwhisk_short}} 插件，请运行以下命令：
```
ic plugin install cloud-functions
```
{: pre}


要验证 {{site.data.keyword.openwhisk_short}} 插件是否成功安装，请运行以下命令：
```
ic plugin list cloud-functions
```
{: pre}


输出会显示安装的 {{site.data.keyword.openwhisk_short}} 版本信息：
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

可以通过运行以下命令来升级 {{site.data.keyword.openwhisk_short}} 插件：
```
ic plugin update Cloud-Functions
```
{: pre}


有关 plugin 命令的更多信息，请使用 `ic plugin --help` 或查看以下文档：
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}} CLI 认证
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} 在两个 {{site.data.keyword.Bluemix_notm}} 区域中可用。

登录到 {{site.data.keyword.Bluemix_notm}} CLI 时，可以指定目标区域的 {{site.data.keyword.Bluemix_notm}} API 端点以及组织和空间。

登录到美国南部区域：
```
ic login -a api.ng.bluemix.net
```
{: pre}


登录到英国区域：
```
ic login -a api.eu-gb.bluemix.net
```
{: pre}


`-a` 标志指定要使用的 {{site.data.keyword.Bluemix_notm}} API 端点。如果已指定 API 端点，那么 `-a` 选项不是必需的。可以使用 `ic api` 命令来显式设置 {{site.data.keyword.Bluemix_notm}} API 端点。要显示当前的 API 端点设置，请使用 `ic target` 命令。

`login` 命令提示您输入信息，例如组织、空间和密码（如果未指定）。您可以在命令行中指定组织和空间以跳过输入这些信息的提示。
```
ic login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


您还可以使用 {{site.data.keyword.Bluemix_notm}} API 密钥来登录。当您的帐户配置为使用“联合登录”（即要求您使用 `--sso` 标志登录）时，此方法会非常有用。如果要设置“持续集成”(CI)，并且要配置无人照管的管道，那么使用 API 密钥也非常有用。
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

要使用 {{site.data.keyword.Bluemix_notm}} CLI 来创建新的 API 密钥，请运行以下命令：
```
ic iam api-key-create MyKey
```
{: pre}


然后，使用 API 密钥生成的值来登录，如以下示例中所示：
```
ic login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


有关 login 命令的更多信息，请使用 `ic login --help` 或查看以下文档：
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## 使用 {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cloudfunctions_plugin_usage}

验证设置。执行回传的阻塞性（同步）调用，并将 `hello` 作为自变量传递，如以下示例中所示：
```
ic wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


请参阅以下示例输出：
```
{
    "message":"hello"
}
```

配置环境后，可以使用 {{site.data.keyword.openwhisk_short}} CLI 来执行以下任务：

* 在 {{site.data.keyword.openwhisk_short}} 上运行代码片段或操作。请参阅[创建和调用操作](./openwhisk_actions.html)。
* 使用触发器和规则以支持操作对事件进行响应。请参阅[创建触发器和规则](./openwhisk_triggers_rules.html)。
* 了解包如何捆绑操作以及配置外部事件源。请参阅[创建和使用包](./openwhisk_packages.html)。
* 浏览包的目录，并通过外部服务（例如，[Cloudant 事件源](./openwhisk_cloudant.html)）来增强应用程序的功能。请参阅[使用启用了 {{site.data.keyword.openwhisk_short}} 的服务](./openwhisk_catalog.html)。

要获取 {{site.data.keyword.openwhisk_short}} 插件的命令列表，请运行不带任何自变量的 `ic wsk`。

## 通过操作使用服务
{: #binding_services}

{{site.data.keyword.openwhisk_short}} 提供了 `service bind` 命令，使您的 {{site.data.keyword.Bluemix_notm}} 服务凭证在运行时可用于代码。然后，可以利用 `service bind` 命令，以支持将任何 {{site.data.keyword.Bluemix_notm}} 服务绑定到 {{site.data.keyword.openwhisk_short}} 中定义的任何操作。

有关如何通过操作使用服务的详细步骤，请参阅[通过操作使用服务](./binding_services.html)主题。

## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS 代理
{: #cli_https_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以设置为使用 HTTPS 代理。要设置 HTTPS 代理，必须创建名为 `HTTPS_PROXY` 的环境变量。该变量必须设置为 HTTPS 代理的地址及其端口，格式如下：`{PROXY IP}:{PROXY PORT}`。

## 使用区域、组织和空间
{: #region_info}

可以通过为每个预生产（编译打包）和生产部署创建空间来处理这些部署。通过创建空间，{{site.data.keyword.openwhisk_short}} 可以具有为您定义的两个不同名称空间。

可以使用 `ic iam space-create` 在您的组织下创建更多空间，例如“staging”和“production”。请参阅以下示例：
```
ic iam space-create "staging"
ic iam space-create "production"
```

有关 `ic iam` 命令的更多信息，请参阅以下文档：https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} 对名称空间的名称施加了限制。有关这些限制的更多信息，请参阅以下文档：ttps://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

如果您已经登录，那么可以在 {{site.data.keyword.Bluemix_notm}} CLI 中运行 `ic target` 命令来切换区域、组织和空间。

使用以下命令来显示所有 {{site.data.keyword.Bluemix_notm}} 区域。

仅 `us-south` 和 `eu-gb` 区域中支持 {{site.data.keyword.openwhisk_short}}。
{: tip}

```
ic regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

使用 `ic target` 命令可更改区域。例如，如果要切换到英国区域和空间 `staging`，请运行以下命令：
```
ic target -r eu-gb -s staging
```
{: pre}


如果需要更改同一区域中的空间（例如，从 staging 更改为 production），请运行以下命令：
```
ic target -s production
```
{: pre}


有关 `target` 命令的更多信息，请使用 `ic target --help` 或查看以下文档：
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## 从 OpenWhisk CLI 迁移到 {{site.data.keyword.openwhisk_short}} CLI 插件
{: #cli_migration}

引入 {{site.data.keyword.openwhisk_short}} CLI 插件后，不再需要 OpenWhisk CLI。

### 命令语法
{: #command_syntax}

除了不再需要的 `wsk bluemix login` 命令外，其他所有 `wsk` 命令的工作方式与使用 `ic wsk` 命令相同。所有命令选项和自变量也都相同。

### API 认证和主机
{: #api_authentication}

OpenWhisk CLI 需要配置认证 API 密钥和 API 主机。使用 {{site.data.keyword.openwhisk_short}} CLI 插件时，您无需显式配置 API 密钥和 API 主机。您需要使用 `ic login` 登录，然后使用 `ic target` 命令将您的区域和名称空间设定为目标。在此之后，所有后续命令都以 `ic wsk` 开头。以这种方式进行认证还可避免访问 {{site.data.keyword.Bluemix_notm}} Web 页面来获取用于配置 CLI 环境的特定信息。

如果需要获取认证 API 密钥以通过外部 HTTP 客户机（例如，cURL 或 Postman）使用 {{site.data.keyword.openwhisk_short}}，可以使用以下命令来检索这些信息：

要获取当前 API 密钥，请运行以下命令：
```
ic wsk property get --auth
```
{: pre}


要获取当前 API 主机，请运行以下命令：
```
ic wsk property get --apihost
```
{: pre}


API 密钥是特定于 {{site.data.keyword.openwhisk_short}} CLI 插件设定为目标的每个区域、组织和空间的。
{: tip}

### API 网关认证
{: #apigw_authentication}

OpenWhisk CLI 目前需要您运行 `wsk bluemix login` 才能配置 API 网关授权，以使用 `wsk api` 命令来管理 API。

使用 {{site.data.keyword.openwhisk}} CLI 插件后，不再需要运行 `wsk bluemix login`。请改为使用 `ic login` 命令登录到 {{site.data.keyword.Bluemix_notm}}，随后 {{site.data.keyword.openwhisk}} 插件会自动利用您当前的登录和目标信息。现在，您可以使用 `ic wsk api` 命令来管理 API。

### 迁移部署脚本
{: #migrating_deploy_scripts}

如果您具有将 OpenWhisk CLI 与 `wsk` 二进制文件一起使用的脚本，那么所有命令的工作方式与使用 `ic wsk` 命令相同。您可以修改脚本以使用 {{site.data.keyword.Bluemix_notm}} CLI 插件，或者创建别名或包装程序，使当前执行的 `wsk` 转换为 `ic wsk`。{{site.data.keyword.Bluemix_notm}} CLI 中的 `ic login` 和 `ic target` 命令以无人照管方式工作。在无人照管方式下，可以先配置环境，然后再执行 `ic wsk` 命令来部署和管理 {{site.data.keyword.openwhisk_short}} 实体。


## 版本历史记录
{: #version_history}

版本的历史记录，其中显示了亮点和错误修订。

1.0.7（2018 年 2 月 2 日）
* 现在，`ic wsk api` 接受路径参数，例如 `/api/{id}`。有关信息，请参阅 [API 网关](./openwhisk_apigateway.html)
* 恢复了代理支持。
* 除去了 `swift:3`。

1.0.6（2018 年 1 月 30 日）
* 修正了针对包内操作的 `ic wsk service bind` 命令的错误。

---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} 独立 CLI

**自 2018 年 3 月 9 日起，{{site.data.keyword.openwhisk_short}} 独立 CLI 将不再可供下载。要在此日期后继续管理 {{site.data.keyword.openwhisk_short}} 实体，请使用 {{site.data.keyword.Bluemix}} CLI 的 [{{site.data.keyword.openwhisk_short}} CLI 插件](./bluemix_cli.html)。**

{{site.data.keyword.openwhisk}} 分布式 **wsk** 命令行界面支持管理所有 {{site.data.keyword.openwhisk_short}} 实体。
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* 配置 {{site.data.keyword.openwhisk_short}} 认证密钥
  * 无需访问 {{site.data.keyword.openwhisk_short}} 控制台来获取认证密钥
  * 在区域、组织和空间之间进行切换时，将自动生成认证密钥
* 刷新到期的 {{site.data.keyword.openwhisk_short}} 认证密钥
* 将 CLI 更新为更高版本


以下管理任务需要使用该插件。

* API 管理
  * 配置 API 网关访问令牌
  * 刷新到期的 API 网关访问令牌

## 设置 {{site.data.keyword.openwhisk_short}} CLI 
{: #openwhisk_cli_setup}

可以使用 {{site.data.keyword.openwhisk_short}} 命令行界面 (CLI) 来设置名称空间和授权密钥。转至[配置 CLI](https://console.bluemix.net/openwhisk/cli?loadWsk=true)，然后遵循指示信息来进行安装。

首先，配置以下两个属性：

1. 要使用的 {{site.data.keyword.openwhisk_short}} 部署的 **API 主机**（名称或 IP 地址）。
2. **授权密钥**（用户名和密码），用于授予您对 {{site.data.keyword.openwhisk_short}} API 的访问权。

有两个 {{site.data.keyword.Bluemix_notm}} 区域可用，它们需要自己的唯一 API 主机和授权密钥。

* 美国南部
  * API 主机：`openwhisk.ng.bluemix.net`

* 英国
  * API 主机：`openwhisk.eu-gb.bluemix.net`

运行以下命令来为所需 {{site.data.keyword.Bluemix_notm}} 区域设置 API 主机：

美国南部：
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

英国：
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

如果需要切换区域，必须使用 API 主机和授权密钥来重新配置 CLI，因为授权密钥是特定于每个区域的。
{: tip}

工件（例如，操作、规则和包）是特定于区域的。所以，如果在多个区域中使用相同工件，必须将其部署到每个所需区域。

如果您知道您的授权密钥，那么可以配置 CLI 来予以使用。 

运行以下命令来设置授权密钥：

```
wsk property set --auth <authorization_key>
```
{: pre}

**提示：**缺省情况下，{{site.data.keyword.openwhisk_short}} CLI 将属性集存储在 `~/.wskprops` 中。此文件的位置可以通过设置 `WSK_CONFIG_FILE` 环境变量进行变更。 

要验证 CLI 设置，请尝试[创建并运行操作](./index.html#openwhisk_start_hello_world)。

## 使用 {{site.data.keyword.openwhisk_short}} CLI
{: #using_openwhisk_cli}

配置环境后，可以使用 {{site.data.keyword.openwhisk_short}} CLI 来执行以下任务：

* 在 {{site.data.keyword.openwhisk_short}} 上运行代码片段或操作。请参阅[创建并调用操作](./openwhisk_actions.html)。
* 使用触发器和规则以支持操作对事件进行响应。请参阅[创建触发器和规则](./openwhisk_triggers_rules.html)。
* 了解包如何捆绑操作以及配置外部事件源。请参阅[使用和创建包](./openwhisk_packages.html)。
* 浏览包的目录，并通过外部服务（例如，[Cloudant 事件源](./openwhisk_cloudant.html)）来增强应用程序的功能。请参阅[预安装的 {{site.data.keyword.openwhisk_short}} 包](./openwhisk_catalog.html)。

## 配置 CLI 以使用 HTTPS 代理
{: #cli_https_proxy}

CLI 可以设置为使用 HTTPS 代理。要设置 HTTPS 代理，必须创建名为 `HTTPS_PROXY` 的环境变量。该变量必须设置为 HTTPS 代理的地址及其端口，格式如下：`{PROXY IP}:{PROXY PORT}`。

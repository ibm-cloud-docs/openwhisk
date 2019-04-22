---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: packages, installable packages

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用可安装的包
{: #installable-packages-overview}

可安装的包表示一种新的设计，用于管理 {{site.data.keyword.openwhisk}} 中的包并与其交互。
{: shortdesc}

## 概述
{: #overview}

**什么是可安装的包？**

包是一组捆绑的相关操作和订阅源。每个包针对与服务和事件提供程序的特定交互进行设计。可安装的包是可供您根据自己的需要来选择、安装和编辑的包。

**可安装的包与预安装的包有何不同？**

[预安装的包](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#browse-packages)会在 `/whisk.system` 名称空间中的 {{site.data.keyword.openwhisk_short}} 内自动进行注册。要在预安装的包中存储凭证或其他参数，必须创建[包绑定](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#openwhisk_package_bind)。

可安装的包不位于 {{site.data.keyword.openwhisk_short}} 系统内，而是位于外部单独的 Github 存储库中。您可以使用 `ibmcloud fn deploy` 命令将这些包直接安装到自己的名称空间中，并且可以为包提供定制名称。由于包已安装到您自己的名称空间中，因此可以根据需要修改包中的操作和订阅源。

## 使用 wskDeploy 安装包
{: #installing}

开始之前：

  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

要安装包，请执行以下操作：

1. 克隆包存储库。包存储库可以在此文档集内每个包的各自页面上找到。
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. 导航至包含 `manifest.yaml` 或 `manifest.yml` 文件的包目录。`manifest.yaml` 文件指定包的总体结构，包括要安装到名称空间中的包和操作，以及 `ibmcloud fn deploy` 命令必须包含的任何元数据。要了解有关 `manifest.yaml` 文件的更多信息，请参阅 [wskdeploy 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example)。
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. 部署包。某些包需要特定环境变量才能正确运行。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### 使用 {{site.data.keyword.cos_full_notm}} 包的示例
{: #example}

要了解有关如何安装包的示例，请查看 [{{site.data.keyword.cos_short}} 包](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions)。{{site.data.keyword.cos_full}} 是一种服务，允许用户存储所有类型的文件，例如图像、视频、音乐和文本。为了与这些文件进行交互，基于云的键/值对数据存储将存储在存储区中。所以，要使用 [{{site.data.keyword.cos_short}} 包](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions)，必须首先创建 {{site.data.keyword.cos_short}} 服务实例，然后创建存储区。存储区用作安装此包所需的环境变量。

创建服务实例和存储区之后，安装包需要以下命令：

1. 克隆包存储库。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 导航至包含 `manifest.yaml` 的包目录。在此示例中，使用的是 {{site.data.keyword.cos_short}} 包的 Node.js 运行时版本。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 使用存储区作为环境变量来部署包。利用对 `PACKAGE_NAME` 环境变量的依赖性，可以为此包指定定制名称。
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}

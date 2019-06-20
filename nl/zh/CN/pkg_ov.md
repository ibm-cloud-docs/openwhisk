---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: packages, installable packages

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

# 包含包
{: #pkg_ov}

包是一组捆绑的相关操作和订阅源。每个包针对与服务和事件提供程序的特定交互进行设计。某些包已随 {{site.data.keyword.openwhisk}} 一起安装以供您使用，但您还可以安装其他包。
{: shortdesc}

## 概述
{: #pkg_overview}

[预安装的包](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse)会在 `/whisk.system` 名称空间中的 {{site.data.keyword.openwhisk_short}} 内自动进行注册。您可以直接使用这些包，而无需完成任何安装步骤。

可安装的包是可供您根据自己的需要来安装、编辑和使用的包。可安装的包不位于 {{site.data.keyword.openwhisk_short}} 系统内，而是位于外部单独的 Github 存储库中。

您可以将这些包直接安装到自己的名称空间中，并且可以为包提供定制名称。由于包已安装到您自己的名称空间中，因此可以根据需要修改包中的操作和订阅源。



## 浏览预安装的包
{: #pkg_browse}

已向 {{site.data.keyword.openwhisk_short}} 注册了多个包。您可以获取名称空间中包的列表，列出包中的实体，以及获取包中个别实体的描述。
{: shortdesc}

1. 获取 `/whisk.system` 名称空间中包的列表。
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  包列表输出：
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. 获取包中实体的列表。

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  示例：
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  示例输出：
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  此输出显示 {{site.data.keyword.cloudant_short_notm}} 包中包含多个操作和一个订阅源。例如，包含两个操作（`read` 和 `write`）和一个名为 `changes` 的触发器订阅源。将文档添加到指定的 {{site.data.keyword.cloudant_short_notm}} 数据库时，`changes` 订阅源会使触发器触发。

  {{site.data.keyword.cloudant_short_notm}} 包还定义了参数 `username`、`password`、`host` 和 `port`。必须指定这些参数，操作和订阅源才有意义。例如，这些参数允许操作对特定 {{site.data.keyword.cloudant_short_notm}} 帐户执行。

3. 获取操作或订阅源的描述，以查看所需的参数。

  示例：
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  示例输出：
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  此输出显示了 {{site.data.keyword.cloudant_short_notm}} `read` 操作需要三个参数，包括要检索的数据库和文档标识。



## 将参数绑定到预安装的包
{: #pkg_bind}

虽然可以直接使用包中的实体，但您可能会发现每次都要将相同的参数传递给操作。通过绑定到包并指定缺省参数（由包中的操作继承），可以简化此过程。
{: shortdesc}

例如，在 `/whisk.system/cloudant` 包中，可以在包绑定中设置缺省 `username`、`password` 和 `dbname` 值，并且这些值会自动传递给包中的任何操作。

在以下示例中，将绑定到 `/whisk.system/samples` 包。

1. 绑定到 `/whisk.system/samples` 包，并设置缺省 `place` 参数值。
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  示例输出：
  ```
ok: created binding valhallaSamples
  ```
  {: screen}

2. 获取包绑定的描述。
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  示例输出：
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting：返回友好的问候
   action /myNamespace/valhallaSamples/wordCount：统计字符串中的字数
   action /myNamespace/valhallaSamples/helloWorld：演示日志记录工具
   action /myNamespace/valhallaSamples/curl：对主机 URL 执行 Curl
  ```
  {: screen}

  请注意，`/whisk.system/samples` 包中的所有操作在 `valhallaSamples` 包绑定中都可用。

3. 调用包绑定中的操作。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  请注意，从结果中可以看出，操作继承了创建 `valhallaSamples` 包绑定时所设置的 `place` 参数。

4. 调用操作，并覆盖缺省参数值。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  请注意，使用操作调用指定的 `place` 参数值会覆盖在 `valhallaSamples` 包绑定中设置的缺省值。






## 添加您自己的包
{: #pkg_add}

可以创建本地代码的包或任何 Github 存储库的克隆。
{: shortdesc}

开始之前：
- [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。
- 为应用程序创建 `manifest.yaml` 或 `manifest.yml` 文件，并将其存储在根目录中。`manifest.yaml` 文件指定包的总体结构，包括 `ibmcloud fn deploy` 命令必须包含的任何元数据。要了解有关 `manifest.yaml` 文件的更多信息，请参阅 [wskdeploy 文档 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example)。
    

要添加包，请执行以下操作：

1. 克隆包存储库。
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. 导航至包含 `manifest.yaml` 文件的目录。
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. 部署包。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    某些包需要特定环境变量才能正确运行。
    如果是这样，请在 `deploy` 命令中包含这些环境变量。例如，可以为包选择名称，并使用 PACKAGE_NAME 变量指定该名称。

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### 使用 {{site.data.keyword.cos_full_notm}} 包的示例
{: #pkg_ex}

要了解有关如何安装包的示例，请查看 [{{site.data.keyword.cos_full_notm}} 包](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)。{{site.data.keyword.cos_full}} 是一种服务，允许用户存储所有类型的文件，例如图像、视频、音乐和文本。为了与这些文件进行交互，基于云的键/值对数据存储将存储在存储区中。所以，要使用 [{{site.data.keyword.cos_full_notm}} 包](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)，必须首先创建 {{site.data.keyword.cos_full_notm}} 服务实例，然后创建存储区。存储区用作安装此包所需的环境变量。

创建服务实例和存储区之后，安装包需要以下命令：

1. 克隆包存储库。
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. 导航至包含 `manifest.yaml` 的包目录。在此示例中，使用的是 {{site.data.keyword.cos_full_notm}} 包的 Node.js 运行时版本。
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 使用存储区作为环境变量来部署包。利用对 `PACKAGE_NAME` 环境变量的依赖性，可以为此包指定定制名称。
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}


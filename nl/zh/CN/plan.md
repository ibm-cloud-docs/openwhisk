---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# 规划无服务器应用程序
{: #plan}

开始创建函数之前，请先了解在此过程中必须进行的决策。
{: shortdesc}

## 查看运行时支持
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} 提供了标准[运行时](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)来执行代码。但是，由于 {{site.data.keyword.openwhisk_short}} 是无服务器的，因此对您可以使用的运行时没有限制。您可以通过创建自己的[定制 Docker 映像](/docs/openwhisk?topic=cloud-functions-actions#actions-docker)来打包代码，从而创建定制运行时。
{: shortdesc}



## 构造代码
{: #plan_architect}

您可能需要对现有代码进行一些调整，使其能在 {{site.data.keyword.openwhisk}} 中运行。如果您尚未有代码，请在编写代码时牢记以下事项。
{: shortdesc}

1. 对代码进行组件化。

    如果您已有一个应用程序并且要使其成为无服务器的，请考虑可能需要如何将该应用程序分解成更小的部分。每个函数都包含一组针对您要运行的代码的触发器。 例如，如果在 GitHub 上创建了问题，请运行此 JavaScript 代码。如果应用程序包含其中多个事件或操作，请考虑将其分隔成不同的函数。

2. 使用 {{site.data.keyword.cloud_notm}} 服务，而不使用框架。

    可以不使用框架来使功能在运行时在您的函数中可用，而改为使用 {{site.data.keyword.cloud}} 服务。框架可帮助您完成的许多常见任务在 {{site.data.keyword.cloud}} 上作为服务提供。
    {: shortdesc}

    例如，不使用框架进行认证，而改为尝试 {{site.data.keyword.appid_full}}。如果需要外部文件存储器，请尝试 {{site.data.keyword.cos_full}}。

    如果要合并的功能未作为 {{site.data.keyword.cloud}} 上的服务提供，那么还可以始终将该功能与订阅源和 API 集成，而无需框架。

3. [验证代码是否符合系统限制](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)。

    代码必须符合系统限制才能运行。例如，如果打包的代码大于 48 MB，那么可能需要继续将其分解成更小的部分，或将其打包为 Docker 映像。

    包含大量第三方模块、本机库或外部工具的应用程序可能会达到此限制。如果您创建了大于 48 MB 的 .zip 或 .jar 包操作，那么必须使用依赖项来扩展运行时映像。然后，使用单个源文件或小于 48 MB 的归档。例如，通过构建包含必要共享库的定制 Docker 运行时，无需在归档文件中提供这些依赖项。仍可在归档中捆绑专用源文件，并在运行时注入这些文件。

4. 确定必须插入到代码中的参数。

    在无服务器操作中，通过向操作添加参数来提供数据。参数会声明为 main 无服务器函数的自变量。
通常，这些参数是服务的凭证，但也可能是使代码可复用于不同触发器的任何内容。

5. [验证在函数中使用代码的结构需求](/docs/openwhisk?topic=cloud-functions-prep)。

    无论您是已有应用程序还是计划开发脚本来使用，代码都可能需要一些调整，使其可供 {{site.data.keyword.openwhisk}} 使用。代码本身必须满足一些结构需求，例如输入参数和输出结果。代码还可能需要打包成单个文件，该文件中包含其所有依赖项。







## 确定事件源
{: #plan_source}

考虑函数中与您希望发生以触发代码运行的事件相关的方面。您可能希望在每次落实到 GitHub 存储库时运行代码。或者，您可能希望每次在 Cloudant 数据库中进行更新时运行代码。
{: shortdesc}

选择用于触发函数的事件后，请查看已可供使用的包。您或许能够使用其中一个包来简化函数开发。如果不能，您可以为事件源创建自己的包。

您可能需要返回到代码，并根据事件选择进行一些修订。


## 组织不同环境中的部署
{: #plan_environments}

决定如何在不同环境中部署函数，例如在开发环境、编译打包环境和生产环境中。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 是一个基于 Cloud Foundry 的服务，因此可以在 {{site.data.keyword.cloud_notm}} 中提供的 Cloud Foundry 组织和空间中管理函数部署。要在这些环境中组织函数，可选择为每个函数创建一个组织。然后，为所需的每个环境创建一个空间。您还可以改为每个环境拥有一个组织，并为每个函数创建一个空间。不管如何安排组织和空间，都请选择可以用于有效管理函数实体的结构。

您还可以使用[名称空间](/docs/openwhisk?topic=cloud-functions-namespaces)来隔离资源。缺省情况下，每个 {{site.data.keyword.cloud_notm}} 空间都包含一个 Open Whisk 名称空间。可以对实体（例如，名称空间中的操作或触发器）进行分组，然后创建 Identity and Access (IAM) 策略来管理该组的用户许可权。





---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, serverless, functions

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


# 常见用例
{: #use_cases}

{{site.data.keyword.openwhisk_short}} 提供的执行模型支持各种用例。以下各部分包含典型的示例。有关无服务器体系结构、示例用例、利弊讨论和实施最佳实践的更详细讨论，请阅读 [Martin Fowler 博客中 Mike Roberts 所写的精彩文章](https://martinfowler.com/articles/serverless.html){: external}。
{: shortdesc}

## 微服务
{: #use_cases_microservices}

尽管基于微服务的解决方案具有优势，但要使用主流云技术来构建这些解决方案仍然很困难，这通常需要控制复杂的工具链，以及单独的构建和运行管道。小型敏捷团队会浪费太多时间来处理复杂的基础架构和操作事项，例如容错、负载均衡、自动缩放和日志记录。这些团队希望有一种方法，能让他们使用熟知惯用且最适合解决特定问题的编程语言来开发精简、增值的代码。

{{site.data.keyword.openwhisk_short}} 具有模块化和固有可缩放性质，因此非常适用于在操作中实现细颗粒度的逻辑片段。{{site.data.keyword.openwhisk_short}} 操作彼此独立，可以使用 {{site.data.keyword.openwhisk_short}} 支持的各种不同语言来实现，并可访问各种后端系统。每个操作都可以独立部署和管理，并独立于其他操作进行缩放。操作之间的互连由 {{site.data.keyword.openwhisk_short}} 以规则、序列和命名约定的形式提供。此类型环境对于基于微服务的应用程序来说是件好事。

支持 {{site.data.keyword.openwhisk_short}} 的另一重要理由是灾难恢复配置中系统的成本。对使用 PaaS 或 CaaS 的微服务与使用 {{site.data.keyword.openwhisk_short}} 的微服务进行比较；假定您有 10 个微服务，它们使用的是容器或 Cloud Foundry 运行时。此比较相当于在单个可用性专区中有 10 个持续运行的计费进程，跨 2 个可用性专区运行时进程有 20 个，而跨 2 个区域（每个区域 2 个专区）运行时进程有 40 个。使用 {{site.data.keyword.openwhisk_short}} 时要达到同样目标，可以根据需要跨任意数量的可用性专区或区域来运行微服务，而不会产生丝毫的递增成本。

## Web 应用程序
{: #use_cases_webapps}

鉴于 {{site.data.keyword.openwhisk_short}} 的事件驱动型性质，它对于面向用户的应用程序有若干优点，而来自用户浏览器的 HTTP 请求会充当事件。{{site.data.keyword.openwhisk_short}} 应用程序仅在处理用户请求时才使用计算能力并计费。不存在空闲备用或等待方式。此功能使 {{site.data.keyword.openwhisk_short}} 相比传统容器或 Cloud Foundry 应用程序要便宜得多。后两种工具会有空闲时间，用于等待入站用户请求，并且即使针对所有这些“睡眠”时间也照样要计费。

完整的 Web 应用程序可以使用 {{site.data.keyword.openwhisk_short}} 来构建并运行。将无服务器 API 与站点资源（例如，HTML、JavaScript 和 CSS）的静态文件托管相组合，意味着您可以构建完全无服务器的 Web 应用程序。运行托管的 {{site.data.keyword.openwhisk_short}} 环境十分简单，因为完全不必执行任何操作。由于 {{site.data.keyword.openwhisk_short}} 是在 {{site.data.keyword.cloud_notm}} 上托管的，因此与维持并运行 Node.js Express 或其他传统服务器运行时相比，这是一个非常大的优势。

## IoT
{: #use_cases_iot}

Internet of Things 场景通常具备由传感器驱动的固有性质。例如，如果需要对超过特定温度的传感器做出反应，那么可能会触发 {{site.data.keyword.openwhisk_short}} 中的操作。IoT 交互通常是无状态的，在自然灾害、严重风暴或交通堵塞等重大自发事件中可能会出现高负载的情况。这将产生对弹性系统的需求，在这种系统中，正常工作负载可能很小，但需要以可预测的响应时间快速进行缩放。因此，需要有能力处理大量同时发生的事件，而无需对系统预先发出警告。很难构建一个系统来满足使用传统服务器体系结构的这些需求。因为它们往往要么能力不足，无法处理流量高峰负载，要么过度供应，产生高昂费用。

可以实现使用传统服务器体系结构的 IoT 应用程序。但是，在许多情况下，不同服务和数据网桥的组合需要高性能、灵活的管道。范围从 IoT 设备一直到云存储和分析平台。预配置的网桥往往缺乏用于实现和微调特定解决方案体系结构的可编程性。考虑到有各种各样的管道以及数据融合在一般情况下缺乏标准化（在 IoT 中尤其如此），因此常常会看到环境中管道需要定制数据变换。这些定制数据变换应用于格式转换、过滤或扩充。{{site.data.keyword.openwhisk_short}} 是一款非常优秀的工具，能以“无服务器”方式实现此类变换，其中定制逻辑在完全受管的弹性云平台上进行托管。

请查看使用 {{site.data.keyword.openwhisk_short}}、Node-RED、Cognitive 和其他服务的以下样本 IoT 应用程序：[Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}。

![IoT 解决方案体系结构示例](images/IoT_solution_architecture_example.png)

## API 后端
{: #use_cases_backend}

通过无服务器计算平台，开发者能快速构建 API，而无需服务器。{{site.data.keyword.openwhisk_short}} 支持为操作自动生成 REST API。[{{site.data.keyword.openwhisk_short}} 功能](/docs/openwhisk?topic=cloud-functions-apigateway)可以通过 POST 之外的 HTTP 方法调用操作，也可以在没有操作的授权 API 密钥的情况下，通过 {{site.data.keyword.openwhisk_short}} API 网关调用操作。此功能不仅对于向外部使用者公开 API 非常有用，同时对于构建微服务应用程序也十分有用。

此外，{{site.data.keyword.openwhisk_short}} 操作可以连接到所选的 API 管理工具（例如，[IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} 或其他工具）。与其他用例类似，所有关于可伸缩性和其他服务质量的注意事项都适用。

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} 是一个样本应用程序，通过 REST API 来使用 {{site.data.keyword.openwhisk_short}} 操作。

请参阅以下示例，其中包含关于[将无服务器用作 API 后端](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}的讨论。

## 移动后端
{: #use_cases_mobile}

许多移动应用程序需要服务器端逻辑。但是，移动开发者通常在管理服务器端逻辑方面没有什么经验，而更倾向于关注设备上运行的应用程序。此开发目标可通过将 {{site.data.keyword.openwhisk_short}} 用作服务器端后端来轻松实现，并且是比较好的解决方案。此外，对服务器端 Swift 的内置支持也允许开发者复用自己现有的 iOS 编程技能。由于移动应用程序通常具有不可预测的负载模式，因此您希望使用 {{site.data.keyword.openwhisk_short}} 解决方案，如 {{site.data.keyword.cloud}}。此解决方案可以缩放以满足对工作负载的几乎任何需求，而无需提前供应资源。

## 数据处理
{: #use_cases_data}

针对现在可用的数据量，应用程序开发需要处理新数据并可能对其做出响应的能力。此需求包括处理结构化数据库记录以及非结构化的文档、图像或视频。{{site.data.keyword.openwhisk_short}} 可以通过系统提供的订阅源或定制订阅源来进行配置，以对数据的变化做出反应，并对入局数据订阅源自动执行操作。操作可以编程为处理更改，变换数据格式，发送和接收消息，调用其他操作以及更新各种数据存储。支持的数据存储包括基于 SQL 的关系数据库、内存中数据网格、NoSQL 数据库、文件、消息传递代理和各种其他系统。通过 {{site.data.keyword.openwhisk_short}} 规则和序列，可灵活地在处理管道时进行更改，无需编程, 只需通过配置更新即可执行更改。利用数据存储选项和很少的管理维护，基于 {{site.data.keyword.openwhisk_short}} 的系统高度敏捷，能轻松适应不断变化的需求。

## 认知
{: #use_cases_cognitive}

认知技术可以有效地与 {{site.data.keyword.openwhisk_short}} 组合使用来创建功能强大的应用程序。例如，IBM Alchemy API 和 Watson Visual Recognition 可以与 {{site.data.keyword.openwhisk_short}} 配合使用，以自动从视频中抽取有用的信息，而不必观看视频。此技术是先前讨论的[数据处理](#use_cases_data)用例的“认知”扩展。{{site.data.keyword.openwhisk_short}} 的另一个有效用途是与认知服务组合使用来实现 Bot 功能。

提供的样本应用程序 [Dark vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external} 正是执行此操作。在此应用程序中，用户使用 Dark Vision Web 应用程序上传视频或图像，该 Web 应用程序会将上传内容存储在 {{site.data.keyword.cloudant_short_notm}} 数据库中。一旦上传视频后，{{site.data.keyword.openwhisk_short}} 会通过侦听 {{site.data.keyword.cloudant_short_notm}} 更改（触发器）来检测到新视频。然后，{{site.data.keyword.openwhisk_short}} 会触发视频抽取器操作。在其执行过程中，抽取器会生成帧（图像），并将其存储在 {{site.data.keyword.cloudant_short_notm}} 中。随后，将使用 Watson Visual Recognition 对这些帧进行处理，得到的结果将存储在相同的 {{site.data.keyword.cloudant_short_notm}} 数据库中。这些结果可以使用 Dark Vision Web 应用程序或 iOS 应用程序进行查看。除了 {{site.data.keyword.cloudant_short_notm}} 外，还可以使用 {{site.data.keyword.cos_full_notm}}；其中，视频和图像元数据会存储在 {{site.data.keyword.cloudant_short_notm}} 中，而媒体文件存储在 {{site.data.keyword.cos_full_notm}} 中。

## 使用 Kafka 或 {{site.data.keyword.messagehub}} 进行事件处理
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} 非常适合与 Kafka、{{site.data.keyword.messagehub_full}}（基于 Kafka）和其他消息传递系统组合使用。这些系统的性质是事件驱动型，因而需要事件驱动型运行时来处理消息。运行时可以将业务逻辑应用于这些消息，这正是 {{site.data.keyword.openwhisk_short}} 通过其订阅源、触发器、操作等提供的功能。Kafka 和 {{site.data.keyword.messagehub}} 通常用于处理较高且不可预测的工作负载量，并且要求这些消息的使用者能随时进行扩展。这种情况也正是 {{site.data.keyword.openwhisk_short}} 最擅长处理的。{{site.data.keyword.openwhisk_short}} 具有内置功能，可使用以及发布 [Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams) 包中提供的消息。




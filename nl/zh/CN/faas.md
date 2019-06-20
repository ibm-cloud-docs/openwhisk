---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: functions compared, openwhisk, architecture, limitless

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

# 函数即服务体系结构
{: #faas}

{{site.data.keyword.openwhisk}} 在高度可缩放的无服务器环境中提供 OpenWhisk。您可以将 {{site.data.keyword.openwhisk_short}} 无服务器体系结构和经济有效的计算与其他体系结构模型进行比较。
{: shortdesc}

## 为什么要使用无服务器？

- 无基础架构开销
- 维护工作最少
- 经济有效
- 轻松缩放
- 快速！


## OpenWhisk 体系结构比较
{: #faas_architecture}

比较的 OpenWhisk 体系结构如下：

1. [{{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk) 上的**功能即服务 (FaaS)**。IBM 是唯一一家提供受管 OpenWhisk 的供应商。[Martin Fowler 的博客](https://martinfowler.com/articles/serverless.html)上详细介绍了使用 FaaS 平台的无服务器编程模型，您可以查看运行采用无服务器设计的 OpenWhisk 的[用例](/docs/openwhisk?topic=cloud-functions-use_cases)。

2. 带有 OpenWhisk 自行构建 (RYO) 的 **基础架构即服务 (IaaS)**。您可以从 Apache Incubation 项目下载 OpenWhisk，然后在 [{{site.data.keyword.Bluemix_notm}} IaaS](https://cloud.ibm.com/catalog/?category=devices) 上运行。

3. **平台即服务 (PaaS)** 作为受管应用程序运行时。由 {{site.data.keyword.Bluemix_notm}} Foundry 实现进行管理的 [Liberty for Java](https://cloud.ibm.com/catalog/starters/liberty-for-java) 运行时就是一个很好的例子。

4. **容器即服务 (CaaS)** 作为受管容器环境。[{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started#container_index) 就是一个很好的例子。

5. 带有 Java EE 运行时的 **基础架构即服务 (IaaS)**。[WebSphere Application Server VM on {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog/services/websphere-application-server) 就是一个很好的例子。

下表从创建和操作应用程序的开发者角度比较了每个体系结构的元素：


|主题|(1) FaaS on {{site.data.keyword.openwhisk_short}}|(2) 带有 OpenWhisk RYO 的 IaaS|(3) PaaS|(4) CaaS|(5) 带有 Java EE 的 IaaS|
| --- | --- | --- | --- | --- | --- |
|	应用程序单元|	单个功能（通常是 JavaScript、Swift 或 Docker 容器中的小代码块）- 可能小于 1 Kb，但也可能更大。通常不超过几 Kb。|	与列 (1) 相同|	取决于使用的运行时。EAR 或 WAR 文件或者其他特定于语言的应用程序捆绑软件，通常相对较大（Kb，在捆绑软件中有许多服务时甚至达到 Mb），但也可能与单个服务一样小。|	Docker 容器是部署单元。|	VM+应用程序服务器+ EAR 或 WAR 文件和其他依赖项 - 通常大小达到 Gb。|
|	资源占用量|	最终用户不为内存、CPU 或其他资源付费，也不用关注这些资源。虽然操作的确会占用一些资源，但用户无需为此担心|	高。最终用户必须首先供应 IaaS 环境，然后才基于该环境安装和配置 OpenWhisk|	低。最终用户为运行中的应用程序占用的内存和 CPU 付费，但不会为未运行的应用程序付费|	低到中等|	高。最终用户必须为应用程序运行时使用的磁盘存储器、内存、CPU 及可能的其他组件付费。应用程序停止运行时，只发生存储器成本|
|	安装和设置|	无需任何操作|	困难 - 所有安装和设置均由最终用户完成|	无需任何操作|	适度 - 硬件、网络、操作系统、容器管理工具由 CaaS 供应商提供，映像、连接和实例由最终用户提供|	困难 - 硬件、网络、操作系统、初始 Java EE 安装由供应商提供，其他配置、集群、缩放由最终用户提供|
|	供应时间|	毫秒|	请参阅列 (4) 和 (5)|	分钟|	分钟|	小时|
|	持续管理|	无|	困难|	无|	适度|	困难|
|	弹性缩放|	每个操作始终根据负载即时进行固有缩放。无需提前供应 VM 或其他资源|	未提供 - 最终用户必须在 IaaS 上提供计算能力并管理 VM 的缩放。一旦缩放了 VM 后，OpenWhisk 就会自动缩放操作，但资源必须已经提前供应|	自动，但缩放速度慢。在增加负载期间，用户可能要等待若干分钟以使缩放操作完成。自动缩放需要仔细调整|	自动，但缩放速度慢。在增加负载期间，用户可能要等待若干分钟以使缩放操作完成。自动缩放需要仔细调整|	未提供|
|	容量规划|	不需要。FaaS 将自动提供所需任意大小的容量|	需要提前供应足够的容量或者对其编制脚本|	需要进行一些容量规划，但提供了一些自动容量增加|	需要进行一些容量规划，但提供了一些自动容量增加|	需要静态供应足够的容量以处理峰值工作负载|
|	持续连接和状态|	有限 - 不能保持持续连接，但容器高速缓存的情况除外。通常，状态必须保留在外部资源中|	与列 (1) 相同|	支持 - 可以长时间保留打开的套接字或连接，也可以在调用之间在内存中存储状态|	支持 - 可以长时间保留打开的套接字或连接，也可以在调用之间在内存中存储状态|	支持 - 可以长时间保留打开的套接字或连接，也可以在调用之间在内存中存储状态|
|	维护|	无 - 整个堆栈由 IBM 管理。|	重要 - 根据目标环境，用户必须供应硬件、网络、操作系统、存储器、数据库，以及安装和维护 OpenWhisk 等。|	无 - 整个堆栈由供应商管理。|	重要 - 用户必须创建和维护定制映像，部署和管理容器以及容器之间的连接等。|	重要 - 用户必须分配 VM，以及分别管理和缩放 Java EE 服务器。|
|	高可用性 (HA) 和灾难恢复 (DR)|	固有/无额外成本|	自行构建 (RYO)|	可用，但有额外成本|	发生故障的容器可以自动重新启动|	可用，但有额外成本，半自动。VM 可以自动故障转移|
|	安全性|	供应商提供|	自行构建 (RYO)|	RYO 和供应商提供混合|	RYO 和供应商提供混合|	自行构建 (RYO)|
|	开发者速率|	最高|	最高|	最高|	一般|	慢|
|	资源利用率（仍需要付费的空闲资源）|	资源从不会空闲，因为资源只会在请求时进行调用。不存在工作负载时，不会发生成本或资源分配。|	由于此选项使用的是 IaaS 或 CaaS - 适用与列 (4) 和 (5) 中类似的注意事项|	某些资源可能处于空闲状态，但自动缩放有助于消除空闲资源。一定数量的运行中实例必须始终存在，并且这些实例很可能只使用了不到 50% 的容量。停止的实例不会发生任何成本|	类似于列 (3)|	某些资源可能处于空闲状态，但不支持自动缩放。一定数量的运行中实例必须始终存在，并且这些实例很可能只使用了不到 50% 的容量。停止的实例可能会发生存储器成本|
|	成熟度|	未成熟|	未成熟|	未成熟|	中等成熟|	高度成熟|
|	资源限制|	[存在某些限制](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)|	取决于分配的资源|	否|	否|	否|
|	很少使用的服务的等待时间|	很少使用的请求的最初响应时间可能长达几秒，但后续请求的响应时间将保持在毫秒范围内|	视情况而定|	短|	短|	短 - 假定系统具有充足的资源|
|	应用程序的最有效点类型|	事件处理、IoT、移动后端和微服务。决不适用于庞大的应用程序。请参阅[用例](/docs/openwhisk?topic=cloud-functions-use_cases)|	与列 (1) 相同，但适用于用户要在非 IBM 云上运行或内部部署中运行时。|	具有全天候工作负载和有状态服务的 Web 应用程序，需要使连接长时间保持打开。可用于运行微服务或庞大的应用程序|	非常适用于微服务应用程序。|	从内部部署迁移到云的传统企业应用程序。非常适用于庞大的应用程序|
|	收费粒度和记帐|	[以 100 毫秒为单位](https://cloud.ibm.com/openwhisk/learn/pricing)|	取决于实现情况 - 如果使用的是 IaaS 或 CaaS，那么类似的注意事项适用 - 请参阅列 (4) 和 (5)|	对于资源捆绑软件（CPU + 内存 + 某些磁盘空间），通常按小时收费（在极少情况下，按分钟收费）|	类似于列 (3)|	类似于列 (3)|
|	总体拥有成本 (TCO)|	对于其最有效点，应用程序很可能比替代方法少花费一个数量级。由于资源是自动缩放的，因此不会发生过量供应情况。|	对于云部署，很可能会比 OpenWhisk FaaS 昂贵，但内部部署可能比传统体系结构便宜|	相对低 - 用户无需供应或管理资源，而只需关注应用程序开发。相对于无服务器，会存在一定水平的过量供应|	适度 - 用户需要供应和管理容器与应用程序，相对于无服务器或 PaaS，都可以看到一定水平的过量供应|	相对高 - 考虑到将旧应用程序迁移到云本机模型的成本可能高昂得无法承受，对于这些应用程序来说，这可能是可行、经济的选择。|

## 成本注意事项
{: #faas_cost}

用于测试、编译打包、负载测试和其他环境的基础架构可能成本不菲。设置基础架构需要时间，并且因为基础架构通常以全天候方式运行，因此往往未得到充分利用，还会消耗大量容量。通过使用无服务器体系结构，对于任意数量的环境，将根据负载而不是定义的环境数来生成成本。
{: shortdesc}

要估算无服务器应用程序的成本，可以使用[定价计算器 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/learn/pricing)。

### 无限容量
{: #faas_capacity}

在传统体系结构中，每个服务会使用分配给它的容量，并且会针对容量消耗量对您计费。{{site.data.keyword.openwhisk_short}} 的无服务器体系结构减少了对微服务体系结构粒度的约束。

{{site.data.keyword.openwhisk_short}} 不使用时，不会产生任何成本。只要存在 HTTP 调用、数据库状态更改或触发代码执行的其他类型的事件，就会执行代码。系统将按执行时间（毫秒，向上舍入到最接近的 100 毫秒）对您计费，而不是无论该 VM 是否在执行有用的工作都按每小时 VM 利用率计费。
因为您只需在使用事件时付费，而不是根据环境数付费，所以您可以将应用程序细分成 100 个、1000 个或者甚至更多个微服务。

### 在任何区域中运行操作
{: #faas_region}

在传统体系结构中，代码必须在要执行该代码的每个区域中运行，并且还必须为该区域的基础架构付费。使用 {{site.data.keyword.openwhisk_short}} 后，可以部署操作并使其可在任何区域中运行，而没有任何额外的成本。您可以提高代码的可用性和弹性，没有传统的成本限制。

### 冗余（按设计）
{: #faas_redundancy}

在传统体系结构中，应用程序必须是冗余的。使用 {{site.data.keyword.openwhisk_short}} 后，过程不必具备高可用性 (HA)，因为无服务器应用程序根据设计就是无状态的并且是请求/事件驱动的。由于无服务器应用程序的无状态性质，无需显式创建冗余，因此可以显著降低基础架构成本。


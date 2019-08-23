---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: platform architecture, openwhisk, couchdb, kafka, functions

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


# {{site.data.keyword.openwhisk_short}} 的工作方式
{: #about}

{{site.data.keyword.openwhisk}} 是一种事件驱动型计算平台，也称为无服务器计算或功能即服务 (FaaS)，用于运行代码以响应事件或直接调用。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 技术
{: #about_technology}

了解 {{site.data.keyword.openwhisk_short}} 背后技术的基本概念：

**什么是操作？**

操作是一小段代码，可以调用或设置为自动运行以响应事件。对于其中任一情况，每次运行都会生成一个由唯一激活标识进行识别的记录。操作的输入和结果可以视为键/值对。键为字符串，值为有效的 JSON 值。可以使用所选语言来编写操作，并将其作为源代码或 Docker 映像提供给服务。操作代码在由 Cloud Functions API、CLI 或 iOS SDK 直接调用时运行。操作可以自动响应来自 IBM Cloud 或第三方服务的事件。

**为什么要使用操作？**

通过使用操作，可以限制代码运行时间量，从而降低成本。

例如，可以使用操作来检测映像中的构面，响应数据库中的更改，聚集一组 API 调用，或者甚至可发布推文。

**一次可以使用多个操作吗？**

可以！您可以使用操作来调用其他操作，也可以将操作串接在一起以[创建序列](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)。要创建序列，一个操作的输出将是另一个操作的输入，而这另一个操作提供的输出又可用于触发另一个操作，依此类推。您甚至可以捆绑所创建的操作组来构成包。使用包时，可以通过调用包来复用常用操作或序列，而不用重新配置操作或序列。

## {{site.data.keyword.openwhisk_short}} 术语

<dl>
  <dt>名称空间</dt>
    <dd>[名称空间](/docs/openwhisk?topic=cloud-functions-namespaces)包含 {{site.data.keyword.openwhisk_short}} 实体（例如，操作和触发器），并属于资源组。通过授予用户对名称空间的访问权，可以允许用户访问 {{site.data.keyword.openwhisk_short}} 实体。</dd>
  <dt>操作</dt>
    <dd>[操作](/docs/openwhisk?topic=cloud-functions-actions)是用于执行一个特定任务的一段代码。可以使用所选语言来编写操作，例如在 Docker 容器中嵌入的 JavaScript 或 Swift 代码或者定制二进制代码小型片段。您以源代码或 Docker 映像形式向 Cloud Functions 提供操作。
<br><br>使用 {{site.data.keyword.openwhisk_short}} API、CLI 或 iOS SDK 来直接调用操作时，操作将正常工作。操作还可以使用触发器自动响应来自 {{site.data.keyword.cloud_notm}} 服务和第三方服务的事件。</dd>
  <dt>序列</dt>
    <dd>可以将一组操作链接成一个[序列](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)，而不必编写任何代码。序列是按顺序调用的操作链，其中一个操作的输出作为输入传递给下一个操作。这允许您将现有操作组合在一起，以便快速、轻松地复用。然后，序列可以像操作一样，通过 REST API 或自动进行调用以响应事件。</dd>
  <dt>事件</dt>
    <dd>事件的示例包括对数据库记录进行更改、IoT 传感器读数超过特定温度、新代码落实到 GitHub 存储库，或者从 Web 或移动应用程序发起简单 HTTP 请求。来自外部和内部事件源的事件将通过触发器进行传递，并且规则允许操作对这些事件做出反应。</dd>
  <dt>触发器</dt>
    <dd>[触发器](/docs/openwhisk?topic=cloud-functions-triggers)是为某类事件指定的通道。触发器是指要对特定类型的事件做出反应的声明，事件可来自用户或来自事件源。</dd>
  <dt>规则</dt>
    <dd>[规则](/docs/openwhisk?topic=cloud-functions-rules)将触发器与操作关联在一起。每次触发器触发时，规则都会将触发器事件作为输入，并调用关联的操作。使用相应的规则集时，可以通过单个触发器事件来调用多个操作，也可以调用一个操作以响应来自多个触发器的事件。</dd>
  <dt>订阅源</dt>
    <dd>通过[订阅源](/docs/openwhisk?topic=cloud-functions-triggers#triggers_feeds)，可以方便地配置外部事件源来触发 {{site.data.keyword.openwhisk_short}} 可使用的触发器事件。例如，Git 订阅源可针对 Git 存储库中的每次落实，触发触发器事件。</dd>
  <dt>包</dt>
    <dd>可以随包添加与服务和事件提供程序的集成。[包](/docs/openwhisk?topic=cloud-functions-pkg_ov)是捆绑在一起的一组订阅源和操作。订阅源是一段代码，用于配置外部事件源以触发触发器事件。例如，使用 {{site.data.keyword.cloudant}} 更改订阅源创建的触发器可将服务配置为每次修改文档或将文档添加到 {{site.data.keyword.cloudant_short_notm}} 数据库时都触发该触发器。包中的操作表示可复用逻辑，服务提供者可提供此逻辑，以便开发者不仅能够将服务用作事件源，还能调用该服务的 API。<br><br>通过包的现有目录，能够迅速借助多个有用的功能来增强应用程序，也能访问生态系统中的外部服务。具有 {{site.data.keyword.openwhisk_short}} 包的外部服务的示例包括 {{site.data.keyword.cloudant_short_notm}}、The Weather Company、Slack 和 GitHub。</dd>
</dl>

### 接下来要做什么？
{: #quiz}
测试您的掌握情况并[进行测验](https://ibmcloud-quizzes.mybluemix.net/functions/terms_quiz/quiz.php){: external}！


## OpenWhisk 内部处理工作原理
{: #about_internal}

为了更详细地说明所有组件，下面将跟踪操作在整个 {{site.data.keyword.openwhisk_short}} 系统中的调用全过程。调用将执行用户馈入系统中的代码，并返回这一执行操作的结果。下图显示了高层次的 {{site.data.keyword.openwhisk_short}} 体系结构。


![{{site.data.keyword.openwhisk_short}} 体系结构](./images/OpenWhisk.png)

### 在 OpenWhisk 中后台发生了什么？
{: #about_scenes}

OpenWhisk 是一个开放式源代码项目，组合了多个组件（例如，NGINX、Kafka、Docker 和 CouchDB）以构成基于事件的无服务器编程服务。

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="OpenWhisk 中的后台内部处理流程" style="width:550px; border-style: none"/>

#### 1. 进入系统：NGINX
{: #about_ngnix}

首先，OpenWhisk 面向用户的 API 基于 HTTP，遵循的是 RESTful 设计。因此，通过 CLI 发送的命令是针对 OpenWhisk 系统的 HTTP 请求。该特定命令大致会转换为：
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

请注意此处的 *$userNamespace* 变量。用户有权访问至少一个名称空间。为了简单起见，假定用户拥有的是放入了 *myAction* 的名称空间。

系统的第一个入口点是 **NGINX**，这是一个“HTTP 和逆向代理服务器”。它用于 SSL 终止以及将相应的 HTTP 调用转发到下一个组件。

#### 2. 进入系统：控制器
{: #about_controller}


NGINX 将 HTTP 请求转发到**控制器**，这是通过 OpenWhisk 的路径中的下一个组件。这是对实际 REST API（基于 **Akka** 和 **Spray**）的基于 Scala 的实现，因此会用作用户可执行的所有操作的接口，包括在 OpenWhisk 中对您实体的 [create、retrieve、update 和 delete](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete){: external} 请求以及操作调用。

控制器首先会明确用户要尝试执行的操作。这基于您在 HTTP 请求中使用的 HTTP 方法。用户将对现有操作发出 POST 请求，而控制器会将其转换为**操作调用**。

鉴于控制器的核心角色（顾名思义），以下步骤在某种程度上都将涉及到控制器。

#### 3. 认证和授权：CouchDB
{: #about_auth}

现在，控制器将验证您的身份（*认证*），并验证您是否有权执行要对该实体执行的操作（*授权*）。请求中包含的凭证将根据 **CouchDB** 实例中的所谓 **subjects** 数据库进行验证。

在本例中，将检查用户在 OpenWhisk 的数据库中是否存在，以及该用户是否有权调用 *myAction* 操作（假定这是用户拥有的名称空间中的操作）。如果真是用户拥有的名称空间中的操作，用户就有权调用该操作。

一切就绪后，即可顺理成章进入下一个处理阶段。

#### 4. 获取操作：还是 CouchDB...
{: #about_couchdb}

由于控制器现在确定用户已获准进入并有权调用该操作，因此会从 CouchDB 中的 **whisks** 数据库装入此操作（在本例中为 *myAction*）。

操作记录主要包含要执行的代码和要传递到操作的缺省参数，并与实际调用请求中包含的参数合并。此外，还包含在执行中对其施加的资源限制，例如允许使用的内存。

在此特定例子中，操作并不会采用任何参数（函数的参数定义为空列表）。因此，假定没有设置任何缺省参数（包括操作的特定参数），从这个角度来说这是最简单的例子。


#### 5. 操作调用者：负载均衡器
{: #about_lb}

负载均衡器是控制器的一部分，通过持续检查系统中可用执行者的运行状态，从而拥有这些执行者的全局视图。这些执行者称为**调用者**。现在，负载均衡器已经知道有哪些调用者可用，接着会选择其中一个来调用请求的操作。

#### 6. 请排好队：Kafka
{: #about_kafka}

从现在开始，对于您发来的调用请求，可能发生的糟糕情况主要有两种：

1. 系统可能崩溃，导致丢失调用。
2. 系统可能负载过重，该调用需要等待其他调用先完成。

解决这两种情况的办法都是使用 **Kafka**，这是“一种高吞吐量、分布式的发布/预订消息传递系统”。控制器和调用者只通过由 Kafka 缓冲和持久存储的消息进行通信。Kafka 会为控制器和调用者减轻在内存中进行缓冲的负担并降低伴随的 *OutOfMemoryException* 风险，同时还可确保消息不会在系统崩溃时丢失。

为了使操作得到调用，控制器会向 Kafka 发布消息，其中包含要调用的操作以及要传递给该操作的参数（在本例中没有参数）。此消息会发给控制器在从 Consul 获取的列表中选择的调用者。

一旦 Kafka 确认自己获得了该消息，即会使用**激活标识**响应对用户的 HTTP 请求。用户后续可将其用于访问此特定调用的结果。这是异步调用模型，其中一旦系统接受调用操作的 HTTP 请求后，该请求即会终止。异步模型（称为阻塞性调用）是可用的，但不在本文讨论范围之内。

#### 7. 调用代码：调用者
{: #about_invoker}

**调用者**是 OpenWhisk 的核心。调用者的职责是调用操作。在 Scala 中也实施了调用者。但远远不止于此。为了以隔离、安全的方式执行操作，它会使用 **Docker**。

Docker 用于以快速、隔离且受控的方式为调用的每个操作设置新的自封装环境（称为*容器*）。对于每个操作调用，都会衍生一个 Docker 容器，并将操作码注入其中。接着，会使用传递给代码的参数来执行代码，获取结果，然后销毁该容器。在此阶段可以执行性能优化，以减少维护需求，并且可以缩短响应时间。

在本例中，手头是基于 *Node.js* 的操作，因此调用者将启动 Node.js 容器。接着，调用者将注入 *myAction* 中的代码，在不带参数的情况下运行该代码，抽取结果，保存日志，然后再次销毁 Node.js 容器。

#### 8. 存储结果：又是 CouchDB
{: #about_storing}

调用者获取结果后，该结果会在 **whisks** 数据库中存储为激活，位于激活标识下。**whisks** 数据库位于 **CouchDB** 中。

在此特定例子中，调用者获得从操作生成的 JSON 对象，抓取 Docker 编写的日志，将这些内容全部放入激活记录中，然后将该记录存储到数据库中。请参阅以下示例：
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

请注意该记录是如何同时包含返回的结果和编写的日志的。它还包含操作调用的开始时间和结束时间。激活记录还包含更多字段，但本示例中为了简单起见只提供了精练版本。

现在，可以再次使用 REST API（重新从步骤 1 开始）来获取您的激活，进而获得操作的结果。为此，请运行以下命令：

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### 总结
{: #about_summary}

您已经了解了简单的 **ibmcloud fn action 调用的 myAction** 是如何经历 {{site.data.keyword.openwhisk_short}} 系统的不同阶段的。该系统本身主要只包含两个定制组件：**控制器**和**调用者**。其他一切都是现成的，是开放式源代码社区中的许多人员开发的。

可以在以下主题中找到有关 {{site.data.keyword.openwhisk_short}} 的更多信息：

* [实体名称](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)
* [操作语义](/docs/openwhisk?topic=cloud-functions-limits#limits_semantics)
* [限制](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)
* [REST API 参考](/apidocs/functions)





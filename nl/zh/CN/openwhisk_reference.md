---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# 系统详细信息和限制
{: #openwhisk_reference}

以下各部分提供了有关 {{site.data.keyword.openwhisk}} 系统和限制设置的技术详细信息。
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 实体
{: #openwhisk_entities}

### 名称空间和包
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} 操作、触发器和规则属于名称空间，有时属于包。

包可以包含操作和订阅源。一个包不能包含其他包，所以不允许包嵌套。此外，实体不必包含在包中。

在 {{site.data.keyword.Bluemix_notm}} 中，组织+空间对与 {{site.data.keyword.openwhisk_short}} 名称空间相对应。例如，组织 `BobsOrg` 和空间 `dev` 将对应于 {{site.data.keyword.openwhisk_short}} 名称空间 `/BobsOrg_dev`。



可以通过[创建 Cloud Foundry 组织和空间](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#region_info)来创建基于 Cloud Foundry 的新名称空间。`/whisk.system` 名称空间保留用于使用 {{site.data.keyword.openwhisk_short}} 系统分发的实体。


### 标准名称
{: #openwhisk_entities_fullyqual}

实体的标准名称为 `/namespaceName/[packageName]/entityName`。请注意，`/` 用于对名称空间、包和实体定界。此外，名称空间必须带有前缀 `/`。

为了方便起见，如果名称空间是用户的缺省名称空间，那么可以将其省略。例如，假设用户的缺省名称空间为 `/myOrg`。以下是一些实体的标准名称及其别名的示例。



|标准名称|别名|名称空间|包|名称|
| --- | --- | --- | --- | --- |
|`/whisk.system/cloudant/read`|  |`/whisk.system`|`cloudant`|`read`|
|`/myOrg/video/transcode`|`video/transcode`|`/myOrg`|`video`|`transcode`|
|`/myOrg/filter`|`filter`|`/myOrg`|  |`filter`|

使用 {{site.data.keyword.openwhisk_short}} CLI 时，可以使用此命名方案，在其他位置也同样可使用此方案。

### 实体名称
{: #openwhisk_entities_names}

包括操作、触发器、规则、包和名称空间在内的所有实体的名称均为遵循以下格式的字符序列：

* 第一个字符必须为字母数字字符或下划线。
* 后续字符可以为字母数字、空格或以下任一值：`_`、`@`、`.` 和 `-`。
* 最后一个字符不能为空格。

更准确地说，名称必须匹配以下正则表达式（使用 Java 元字符语法表达）：`\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`。

## 操作语义
{: #openwhisk_semantics}

以下各部分描述了有关 {{site.data.keyword.openwhisk_short}} 操作的详细信息。

### 无状态
{: #openwhisk_semantics_stateless}

操作实施是无状态的，即*幂等*。由于系统不会强制实施此属性，因此不保证操作保持的任何状态在各调用中均可用。

此外，一个操作可能存在多个实例化，其中每个实例化都有其自己的状态。操作调用可能会分派给其中任一实例化。

### 调用输入和输出
{: #openwhisk_semantics_invocationio}

操作的输入和输出是键/值对的字典。键为字符串，值为有效的 JSON 值。

### 操作的调用顺序
{: #openwhisk_ordering}

操作的调用并未排序。如果用户通过命令行或 REST API 调用一个操作两次，那么第二个调用可能会先于第一个调用运行。如果操作有副作用，那么可能会以任意顺序观察到这些副作用。

此外，无法保证操作自动执行。两个操作可能并行运行，其副作用可能会交错。对于副作用，OpenWhisk 并不能确保任何特定的并行一致性模型。任何并行副作用均依赖于实施。

### 操作执行保证
{: #openwhisk_atmostonce}

收到调用请求时，系统会记录该请求，并分派激活。

系统会返回激活标识（对于非阻塞性调用），以确认收到了调用。如果存在网络故障或其他在接收 HTTP 响应之前引起干预的故障，那么 {{site.data.keyword.openwhisk_short}} 有可能会收到并处理该请求。

系统会尝试调用操作一次，并生成以下四个结果之一：
- *成功*：操作调用成功完成。
- *应用程序错误*：操作调用成功，但操作有意返回了错误值，例如由于不满足自变量上的前置条件。
- *操作开发者错误*：操作已调用，但以异常方式完成，例如操作未检测到异常或存在语法错误。
- *whisk 内部错误*：系统无法调用操作。
结果会记录在激活记录的 `status` 字段中，如以下部分中所述。

成功收到的每个调用以及可能对用户记帐的每个调用都将有激活记录。

结果为*操作开发者错误*时，操作可能已部分运行并生成外部可视的副作用。用户应负责检查是否实际发生了此类副作用并根据需要发出重试逻辑。某些 *whisk 内部错误*指示操作已开始运行，但在操作显示完成之前失败。

## 激活记录
{: #openwhisk_ref_activation}

每次操作调用和触发器触发都会生成一个激活记录。

激活记录包含以下字段：

- *activationId*：激活标识。
- *start* 和 *end*：记录激活开始时间和结束时间的时间戳记。值为 [UNIX 时间格式](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15)。
- *namespace* 和 `name`：实体的名称空间和名称。
- *logs*：字符串数组，其中包含在操作激活期间由操作生成的日志。每个数组元素对应于操作向 `stdout` 或 `stderr` 生成的一行输出，并且包含日志输出的时间和流。结构如下：`TIMESTAMP STREAM: LOG_OUTPUT`。
- *response*：用于定义 `success`、`status` 和 `result` 键的字典：
  - *status*：激活结果，可能为以下某个值：“success”、“application error”、“action developer error”和“whisk internal error”。
  - *success*：当且仅当 status 为“`success`”时，此项为 `true`
- *result*：包含激活结果的字典。如果激活成功，那么结果将包含操作返回的值。如果激活不成功，那么 `result` 会包含 `error` 键，通常还会带有失败说明。

## REST API
{: #openwhisk_ref_restapi}

有关 {{site.data.keyword.openwhisk_short}} REST API 的信息可在 [REST API 参考](https://cloud.ibm.com/apidocs/functions)中找到。

## 系统限制
{: #openwhisk_syslimits}

### 操作
{{site.data.keyword.openwhisk_short}} 存在一些系统限制，包括一个操作可以使用的内存量和每分钟允许的操作调用数。

下表列出了操作的缺省限制。

|限制|描述
|缺省值|最小值|最大值|
| ----- | ----------- | :-------: | :---: | :---: |
|[codeSize](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_codesize)|操作码的最大大小（以 MB 为单位）。|48|1|48|
|[concurrent](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_concurrent)|每个名称空间提交的正在执行或排队等待执行的激活数不得超过 N。|1000|1|1000* |
|[logs](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_logs)|不允许容器向 stdout 写入超过 N MB。|10|0|10|
|[memory](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_memory)|不允许容器分配的内存超过 N MB。|256|128|2048|
|[minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_minuterate)|每个名称空间每分钟提交的激活数不得超过 N。|5000|1|5000*|
|[openulimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_openulimit)|操作的最大打开文件数。|1024|0|1024|
|[parameters](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_parameters)|可以附加的参数的最大大小（以 MB 为单位）。|5|0|5|
|[proculimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_proculimit)|操作可用的最大进程数。|1024|0|1024|
|[result](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_result)|操作调用结果的最大大小（以 MB 为单位）。|5|0|5|
| [sequenceMaxActions](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_sequencemax)|组成给定序列的最大操作数。|50|0|50*|
|[timeout](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_timeout)|不允许容器运行时间超过 N 毫秒。|60000|100|600000|

### 增大固定限制
{: #increase_fixed_limit}

以 (*) 结尾的限制值是固定值，但如果业务案例可以证明使用更高的安全限制值是合理的，那么可以增大这些值。如果要增大限制值，请通过直接在 IBM [{{site.data.keyword.openwhisk_short}} Web 控制台](https://cloud.ibm.com/openwhisk)中开具凭单来联系 IBM 支持人员。
  1. 选择**支持**。
  2. 从下拉菜单中选择**添加凭单**。
  3. 对于凭单类型，选择**技术**。
  4. 对于“技术支持领域”，选择**函数**。

#### codeSize (MB)（固定值：48 MB）
{: #openwhisk_syslimits_codesize}
* 操作的最大代码大小为 48 MB。
* 对于 JavaScript 操作，请使用工具将所有源代码（包括依赖项）连接为单个捆绑文件。
* 此限制是固定值，不能更改。

#### concurrent（固定值：1000*）
{: #openwhisk_syslimits_concurrent}
* 针对一个名称空间正在执行或排队等待执行的激活数不能超过 1000。
* 此限制值是固定值，但如果业务案例可以证明使用更高的安全限制值是合理的，那么可以增大此值。请查看[增大固定限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit)部分，以获取有关如何增大此限制的详细指示信息。

#### logs (MB)（固定值：10 MB）
{: #openwhisk_syslimits_logs}
* 日志限制 N 在 [0 MB..10 MB] 范围内，并且逐个操作进行设置。
* 创建或更新操作时，用户可以更改操作日志限制。
* 超过所设置限制的日志会被截断，所以将忽略任何新的日志条目，并且在激活的最后输出中会添加警告，指出激活超出所设置的日志限制。

#### memory (MB)（固定值：256 MB）
{: #openwhisk_syslimits_memory}
* 内存限制 M 在 [128 MB..2048 MB] 范围内，并且逐个操作进行设置（以 MB 为单位）。
* 创建操作时，用户可以更改内存限制。
* 容器使用的内存不能超过此限制所分配的内存。

#### minuteRate（固定值：5000*）
{: #openwhisk_syslimits_minuterate}
* 速率限制 N 设置为 5000，用于限制 1 分钟时段中的操作调用数。
* 超过此限制的 CLI 或 API 调用将收到与 HTTP 状态码“`429：请求过多`”对应的错误代码。
* 此限制值是固定值，但如果业务案例可以证明使用更高的安全限制值是合理的，那么可以增大此值。请查看[增大固定限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit)部分，以获取有关如何增大此限制的详细指示信息。

#### openulimit（固定值：1024:1024）
{: #openwhisk_syslimits_openulimit}
* 操作的最大打开文件数为 1024（同时适用于硬限制和软限制）。
* 此限制是固定值，不能更改。
* 调用操作时，docker run 命令使用自变量 `--ulimit nofile=1024:1024` 来设置 `openulimit` 值。
* 有关更多信息，请参阅 [docker run](https://docs.docker.com/engine/reference/commandline/run) 命令行参考文档。

#### parameters（固定值：5 MB）
{: #openwhisk_syslimits_parameters}
* 创建或更新操作/包/触发器的参数总大小限制为 5 MB。
* 尝试创建或更新具有过大参数的实体时，会遭到拒绝。
* 此限制是固定值，不能更改。

#### proculimit（固定值：1024:1024）
{: #openwhisk_syslimits_proculimit}
* 操作容器可用的最大进程数为 1024。
* 此限制是固定值，不能更改。
* 调用操作时，docker run 命令使用自变量 `--pids-limit 1024` 来设置 `proculimit` 值。
* 有关更多信息，请参阅 [docker run](https://docs.docker.com/engine/reference/commandline/run) 命令行参考文档。

#### result（固定值：5 MB）
{: #openwhisk_syslimits_result}
* 操作调用结果的最大输出大小（以 MB 为单位）。
* 此限制是固定值，不能更改。

#### sequenceMaxActions（已修订：50*）
{: #openwhisk_syslimits_sequencemax}
* 组成给定序列的最大操作数。
* 此限制是固定值，不能更改。

#### timeout（毫秒）（缺省值：60 秒）
{: #openwhisk_syslimits_timeout}
* 超时限制 N 在 [100 毫秒..600000 毫秒] 范围内，并且逐个操作进行设置（以毫秒为单位）。
* 创建操作时，用户可以更改超时限制。
* 将终止运行时间超过 N 毫秒的容器。

### 触发器

触发器受每分钟触发率的影响，如下表中所述。

|限制|描述
|缺省值|最小值|最大值|
| ----- | ----------- | :-------: | :---: | :---: |
|[minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_tminuterate)|每个名称空间每分钟触发的触发器数不得超过 N。|5000*|5000*|5000*|

### 增大固定限制
{: #increase_fixed_tlimit}

以 (*) 结尾的限制值是固定值，但如果业务案例可以证明使用更高的安全限制值是合理的，那么可以增大这些值。如果要增大限制值，请通过直接在 IBM [{{site.data.keyword.openwhisk_short}} Web 控制台](https://cloud.ibm.com/openwhisk)中开具凭单来联系 IBM 支持人员。
  1. 选择**支持**。
  2. 从下拉菜单中选择**添加凭单**。
  3. 对于凭单类型，选择**技术**。
  4. 对于“技术支持领域”，选择**函数**。

#### minuteRate（固定值：5000*）
{: #openwhisk_syslimits_tminuterate}

* 速率限制 N 设置为 5000，用于限制用户在 1 分钟时段中可以触发的触发器数。
* 创建触发器时，用户无法更改触发器限制。
* 超过此限制的 CLI 或 API 调用将收到与 HTTP 状态码“`429：请求过多`”对应的错误代码。
* 此限制值是固定值，但如果业务案例可以证明使用更高的安全限制值是合理的，那么可以增大此值。请查看[增大固定限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_tlimit)部分，以获取有关如何增大此限制的详细指示信息。

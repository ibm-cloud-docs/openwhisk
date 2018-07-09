---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 在 IBM Cloud 查看激活日志
{: #openwhisk_logs}

可以在 [{{site.data.keyword.openwhisk}}“监视”页面](https://console.bluemix.net/openwhisk/dashboard/)中直接查看激活日志。这些日志还会转发到 [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)（在其中对这些日志建立索引），从而支持对生成的所有消息进行全文搜索，并根据特定字段（如日志级别）方便地执行查询。
{:shortdesc}

## 查询日志
{: #query-logs}

使用 [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 托管的 Kibana 时，查询日志非常简单。使用 Kibana 的查询语法可找到要查找的日志。

通过 {{site.data.keyword.openwhisk_short}} UI，可以直接浏览到 Kibana 中操作的日志和结果。**日志**链接可在 [{{site.data.keyword.openwhisk}}“监视”页面](https://console.bluemix.net/openwhisk/dashboard/)中的内部左侧导航中找到。访问特定操作的详细信息页面时，**日志**链接会将您转至该特定操作的结果（激活记录）。要显示其日志的时间范围的缺省值设置为 15 分钟。如果要显示更旧的记录，可以直接在 Kibana 右上角更改此值。

下面是有助于调试错误的若干查询示例。

### 查找所有错误日志：
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### 查找由“myAction”生成的所有错误日志：
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## 查询结果
{: #query-results}

除了日志行外，[IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 还会对 {{site.data.keyword.openwhisk_short}} 生成的结果（激活记录）建立索引。结果包含与激活相关的富元数据，例如其持续时间或结果代码（成功、错误）。所有字段都是可查询的，因此可以帮助您了解 {{site.data.keyword.openwhisk_short}} 操作的行为方式。

使用 Kibana 的查询语法可找到要查找的激活。下面是有助于调试错误的若干查询示例。

### 查找所有失败的激活：
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

与 Unix 命令一样，“`0`”指示成功退出的操作，而其他所有操作都被视为错误。

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### 查找由于特定错误而失败的所有激活：
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}

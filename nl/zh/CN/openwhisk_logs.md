---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 日志记录和监视器活动
{: #openwhisk_logs}

日志记录和监视功能在 {{site.data.keyword.openwhisk_short}} 中会自动启用，可帮助您对问题进行故障诊断，并提高操作的运行状况和性能。

## 查看日志
{: #view-logs}

可以直接在“{{site.data.keyword.openwhisk_short}} 监视”仪表板中查看激活日志。这些日志还会转发到 [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana)（在其中对这些日志建立索引），从而支持对生成的所有消息进行全文搜索，并根据特定字段方便地执行查询。
{:shortdesc}

1. 打开 [{{site.data.keyword.openwhisk_short}} 监视页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/dashboard/).

2. 可选：要仅查看特定操作的日志，请将监视摘要限制为该操作。在“过滤选项”部分中，从**限制为**下拉列表中选择操作名称。

3. 在左侧导航中，单击**日志**。这将打开 {{site.data.keyword.loganalysisshort_notm}} Kibana 页面。

4. 可选：要查看较旧的日志，请通过单击右上角的**最近 15 分钟**并选择其他时间范围来更改缺省时间范围值 15 分钟。

### 查询日志
{: #query-logs}

可以使用 Kibana 的查询语法在 [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 中查找特定激活日志。

以下示例查询可帮助您调试错误：
  * 查找所有错误日志：
      ```
type: user_logs AND stream_str: stderr
```
      {: codeblock}

  * 查找由“myAction”生成的所有错误日志：
      ```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
      {: codeblock}

### 查询结果
{: #query-results}

除了日志行外，[{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) 还会对 {{site.data.keyword.openwhisk_short}} 生成的结果或激活记录建立索引。结果包含激活元数据，例如激活持续时间或激活结果代码。查询结果字段可帮助您了解 {{site.data.keyword.openwhisk_short}} 操作的行为方式。

可以使用 Kibana 的查询语法查找特定激活日志。以下示例查询可帮助您调试错误：

* 查找所有失败的激活：
    ```
type: activation_record AND NOT status_str: 0
```
    {: codeblock}
    在结果中，`0` 指示已成功退出操作，其他所有值均指示错误。

* 查找由于特定错误而失败的所有激活：
    ```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
    {: codeblock}

## 监视活动
{: #openwhisk_monitoring}

[{{site.data.keyword.openwhisk_short}}“仪表板”](https://console.bluemix.net/openwhisk/dashboard/)提供了对您活动的图形化摘要。使用该仪表板可确定 {{site.data.keyword.openwhisk_short}} 操作的性能和运行状况。
{:shortdesc}

可以通过选择要查看的操作日志，然后选择记录的活动的时间范围来过滤日志。这些过滤器会应用于仪表板上的所有视图。
随时单击**重新装入**可使用最新的活动日志数据更新该仪表板。

### 活动摘要
{: #summary}

**活动摘要**视图提供了您的 {{site.data.keyword.openwhisk_short}} 环境的高级别摘要。使用此视图可监视启用了 {{site.data.keyword.openwhisk_short}} 的服务的总体运行状况和性能。通过此视图中的度量值，可以执行以下操作：
* 对于服务的启用了 {{site.data.keyword.openwhisk_short}} 的操作，通过查看调用这些操作的次数，确定这些操作的使用率。
* 确定所有操作中的总体失败率。如果发现错误，可以通过查看**活动直方图**视图来确定哪些服务或操作发生了错误。通过查看**活动日志**来确定错误本身。
* 通过查看与每个操作关联的平均完成时间，确定操作的执行情况。

### 活动时间线
{: #timeline}

**活动时间线**视图显示垂直条形图，用于查看过去和现在操作的活动情况。红色指示特定操作中有错误。将此视图与**活动日志**相关联，可了解有关错误的更多详细信息。

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### 活动日志
{: #log}

此**活动日志**视图显示已设置格式的激活日志版本。此视图显示每个激活的详细信息，并且一分钟会轮询一次，以确定是否有新的激活。单击某个操作可显示详细日志。

要使用 CLI 获取“活动日志”中显示的输出，请使用以下命令：
```
ibmcloud fn activation poll
```
{: pre}

---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health, functions

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


# 查看日志
{: #logs}

日志记录功能在 {{site.data.keyword.openwhisk}} 中会自动启用，可帮助您对问题进行故障诊断。您还可以使用 {{site.data.keyword.cloudaccesstraillong}} 服务来跟踪用户和应用程序如何与 {{site.data.keyword.openwhisk_short}} 服务进行交互。


## 查看生成的操作日志
{: #logs_poll}

{{site.data.keyword.openwhisk_short}} 操作可以由其他用户调用、响应各种事件或作为操作序列的组成部分。要获取有关调用操作的时间和输出内容的信息，监视操作日志会非常有用。

可以使用 {{site.data.keyword.openwhisk_short}} CLI 在调用操作时监视其输出。

1. 启动轮询循环，以持续检查从激活生成的日志。
    

    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. 切换到其他窗口，并调用操作。
    

    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    **示例输出**
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. 在轮询窗口中，可以查看激活日志。
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
      2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    您还可以在 {{site.data.keyword.openwhisk_short}} 中实时查看代表您运行的任何操作的日志。




## 查看激活详细信息
{: #activation_details}

{{site.data.keyword.openwhisk_short}} 操作可以由其他用户调用、响应各种事件或作为操作序列的组成部分。每当调用操作时，都会为该调用创建激活记录。要获取有关操作调用结果的信息，您可以获取有关激活的详细信息。

通过运行以下命令，可以获取名称空间中的所有激活记录标识。
```
ibmcloud fn activation list
```
{: pre}

通过运行以下命令，可以获取有关操作调用生成的特定激活记录的详细信息。请将 `<activation_ID>` 替换为激活的标识。 
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

**示例输出**
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
  },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "myNamespace/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
  },
        {
            "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>了解 <code>activation get</code> 命令输出</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>activation get</code> 命令输出</th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>此激活所在的名称空间。这可能与操作所在的名称空间不同。</td>
</tr>
<tr>
<td><code>name</code></td>
<td>操作的名称。</td>
</tr>
<tr>
<td><code>version</code></td>
<td>操作的语义版本。</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>激活该项的用户帐户。</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>此激活记录的标识。</td>
</tr>
<tr>
<td><code>start</code></td>
<td>激活的开始时间。</td>
</tr>
<tr>
<td><code>end</code></td>
<td>激活的完成时间。</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>完成激活所用时间（以毫秒为单位）。</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>：激活的出口状态。</li>
<li><code>statusCode</code>：状态码。如果操作生成错误，那么此值为 HTTP 错误代码。</li>
<li><code>success</code>：表示操作是否成功完成的结果。</li>
<li><code>result</code>：从激活返回的值。</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>此激活的日志。</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>有关此操作的注释。有关可能的激活注释的列表，请参阅[注释参考主题](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation)。</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>表示操作是否发布的结果。</td>
</tr>
</tbody></table>



## 在 {{site.data.keyword.loganalysisfull_notm}} 中查看日志
{: #logs_view}

{{site.data.keyword.loganalysislong_notm}} 日志不可用于基于 IAM 的名称空间。
{: note}

可以直接在“{{site.data.keyword.openwhisk_short}} 监视”仪表板中查看激活日志。这些日志还会转发到 [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana)（在其中对这些日志建立索引），从而支持对生成的所有消息进行全文搜索，并根据特定字段方便地执行查询。
{:shortdesc}

对于美国东部区域，日志记录不可用。
{: important}

1. 打开 [{{site.data.keyword.openwhisk_short}} 监视页面](https://cloud.ibm.com/openwhisk/dashboard){: external}。

2. 可选：要仅查看特定操作的日志，请将监视摘要限制为该操作。在“过滤选项”部分中，从**限制为**下拉列表中选择操作名称。

3. 在左侧导航中，单击**日志**。这将打开 {{site.data.keyword.loganalysisshort_notm}} Kibana 页面。

4. 可选：要查看较旧的日志，请通过单击**最近 15 分钟**并选择其他时间范围来更改缺省时间范围值 15 分钟。

### 查询日志
{: #logs_query}

可以使用 Kibana 的查询语法在 [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) 中查找特定激活日志。

以下示例查询可帮助您调试错误。
  * 查找所有错误日志。
      ```
type: user_logs AND stream_str: stderr
```
      {: codeblock}

  * 查找由 `myAction` 生成的所有错误日志。
      ```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
      {: codeblock}

### 查询结果
{: #logs_query_results}

除了日志行外，[{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) 还会对 {{site.data.keyword.openwhisk_short}} 生成的结果或激活记录建立索引。结果包含激活元数据，例如激活持续时间或激活结果代码。查询结果字段可帮助您了解 {{site.data.keyword.openwhisk_short}} 操作的行为方式。

可以使用 Kibana 的查询语法查找特定激活日志。以下示例查询可帮助您调试错误：

* 查找所有失败的激活。
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    在结果中，`0` 指示操作已成功退出。其他所有值均指示错误。

* 查找由于特定错误而失败的所有激活。
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}




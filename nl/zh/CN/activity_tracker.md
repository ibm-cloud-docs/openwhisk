---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications

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


# 查看 {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #activity_tracker}

您可以使用 {{site.data.keyword.cloudaccesstrailshort}} 服务来查看、管理和审计在 {{site.data.keyword.openwhisk}} 服务实例中执行的用户启动的活动。
{: shortdesc}


有关服务如何运作的更多信息，请参阅 [{{site.data.keyword.cloudaccesstrailshort}} 文档](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)。


## 查看事件的位置
{: #view}

事件在 {{site.data.keyword.openwhisk_short}} 名称空间资源可用的 {{site.data.keyword.Bluemix_notm}} 区域中可用的 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**中可用。有关更多信息，请参阅[查看帐户事件](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events)。

1. 登录到您的 {{site.data.keyword.Bluemix_notm}} 帐户。
2. 在目录中，在 {{site.data.keyword.openwhisk}} 实例所在的帐户中供应 {{site.data.keyword.cloudaccesstrailshort}} 服务的实例。
3. 在 {{site.data.keyword.cloudaccesstrailshort}} 仪表板的**管理**选项卡上，单击**在 Kibana 中查看**。
4. 设置要查看其日志的时间范围。缺省值为 15 分钟。
5. 在**可用字段**列表中，单击**类型**。单击 **Activity Tracker** 的“放大镜”图标，以将日志限制为仅显示由服务跟踪的日志。
6. 可以使用其他可用字段来缩小搜索范围。

要使除帐户所有者以外的其他用户能够查看日志，您必须使用高端套餐。要允许其他用户查看事件，请参阅[授予查看帐户事件的许可权](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)。
{: tip}


## 事件列表
{: #events}

请查看下表，以了解发送到 {{site.data.keyword.cloudaccesstrailshort}} 的事件的列表。
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>操作</th>
      <th>描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>创建基于 IAM 的名称空间资源</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>迁移基于 Cloud Foundry 的名称空间，使其成为启用 IAM 的名称空间</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>更新基于 IAM 的名称空间资源属性，例如显示名称或描述</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>删除名称空间资源</td>
    </tr>
  </tbody>
</table>

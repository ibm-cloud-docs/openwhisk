---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# 故障诊断
{: #troubleshooting}

在使用 {{site.data.keyword.openwhisk}} 时，如果遇到问题，可以考虑使用下列方法进行故障诊断和获取帮助。
{: shortdesc}



## 操作失败
{: #ts_action_fails}

{: tsSymptoms}
操作失败。

{: tsCauses}
对于应用程序失败的一个解释是，使用的是不推荐的运行时。操作无法成功完成，直到将相应运行时更新为支持的运行时为止。

{: tsResolve}
要查看哪个运行时用于操作，请运行 `ibmcloud fn action get ACTION_NAME`，并在查询响应中检查是否 `deprecated=true`。如果这是不推荐使用的运行时，请[更新运行时](/docs/openwhisk?topic=cloud-functions-actions#actions_update)。



## 已达到系统限制
{: #ts_limit_reached}

{: tsSymptoms}
函数未正常运行，可能会生成类似于 `{"error":"signal: killed"}` 的消息。

{: tsCauses}
所有函数都具有最小和最大[系统限制](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)，例如最大内存使用量或超时。

{: tsResolve}
如果业务用例可以证明使用更高安全限制值是合理的，那么可以增大某些限制。要增大限制值，请通过直接在 IBM Cloud Functions Web 控制台中开具凭单来联系 IBM 支持人员。

1. 选择**支持**。
2. 从下拉菜单中选择**添加凭单**。
3. 对于凭单类型，选择**技术**。
4. 对于“技术支持领域”，选择**函数**。



## 获取帮助与支持
{: #gettinghelp}

函数仍然有问题？
{: shortdesc}

-   要查看 {{site.data.keyword.cloud_notm}} 是否可用，请[检查 {{site.data.keyword.cloud_notm}} 状态页面](https://cloud.ibm.com/status?selected=status){: external}。
-   复查论坛，以查看是否有其他用户遇到相同的问题。使用论坛来提问时，请标记您的问题，以便 {{site.data.keyword.cloud_notm}} 开发团队能看到您的问题。
    -   如果您有关于使用 {{site.data.keyword.openwhisk}} 开发函数的技术问题，请在 [Stack Overflow](https://stackoverflow.com/search?q=ibm-cloud-functions){: external} 上发布问题，并使用 `ibm-cloud-functions` 标记您的问题。
    -   有关服务的问题和入门指示信息，请使用 [IBM Developer Answers](https://developer.ibm.com/answers/topics/functions){: external} 论坛。请包含 `ibm-cloud` 和 `functions` 标记。
    有关使用论坛的更多详细信息，请参阅[获取帮助](/docs/get-support?topic=get-support-getting-customer-support#using-avatar)。
-   通过打开一个案例，与 IBM 支持人员联系。要了解有关开具 IBM 支持案例或有关支持级别和案例严重性的信息，请参阅[联系支持人员](/docs/get-support?topic=get-support-getting-customer-support)。报告问题时，请包含激活标识。要获取激活标识，请运行 `ibmcloud fn activation list`。
{: tip}


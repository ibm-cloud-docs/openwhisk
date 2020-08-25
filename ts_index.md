---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-25"

keywords: troubleshooting actions, functions, help, support,

subcollection: openwhisk

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

# Troubleshooting common issues
{: #troubleshooting}

If you have problems while you're working with {{site.data.keyword.openwhisk}}, consider these techniques for troubleshooting and getting help. 
{: shortdesc}

## Action is failing
{: #ts_action_fails}

{: tsSymptoms}
The action is failing.

{: tsCauses}
One explanation for a failing app is that a deprecated runtime is being used. An action cannot be completed successfully until the runtime is updated to a supported one.

{: tsResolve}
To see which runtime is used for the action, run `ibmcloud fn action get ACTION_NAME` and check for `deprecated=true` in the query response. If the runtime is deprecated, [update the runtime](/docs/openwhisk?topic=openwhisk-actions#actions_update). For more information, see [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).

## Service configuration properties are incorrect
{: #ts_property}

{: tsSymptoms}
Your properties are not being set correctly. For example, the incorrect namespace is targeted.

{: tsCauses}
When a property is set, it is retained on your workstation at `<home_dir>/.bluemix/plugins/cloud-functions/config.json`.

{: tsResolve}
You might review your properties by using the `ibmcloud fn property get --all` command. You might need to run `ibmcloud fn property unset --<property>` to unset a specific property, then reset it. Or you might unset all properties by deleting the `config.json` file. This config file is re-created when you work with the service, for example, when you run `ibmcloud fn property get --all`.


## System limits were reached
{: #ts_limit_reached}

{: tsSymptoms}
Your function is not running properly and might result in a message like `{"error":"signal: killed"}`.

{: tsCauses}
All functions have minimum and maximum [system limits](/docs/openwhisk?topic=openwhisk-limits), such as memory usage maximum or a timeout.

{: tsResolve}
Some limits can be increased if a business case can justify higher safety limit values. To increase the limit value, contact IBM support by opening a ticket directly from the IBM Cloud Functions web console.

1. Select **Support**.
2. Select **Add Ticket** from the drop-down menu.
3. Select **Technical** for the ticket type.
4. Select **Functions** for Technical area of support.

## Can't access private endpoint from action
{: #ts_private_endpoint}

{: tsSymptoms}
You cannot connect to a private endpoint from your action.

{: tsCauses}
The use of private endpoints from within your action code is currently not supported by {{site.data.keyword.openwhisk}}.

{: tsResolve}
With {{site.data.keyword.cloud_notm}} service endpoints, you can connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network. However, the only integration that supports private endpoints with {{site.data.keyword.openwhisk}} is {{site.data.keyword.cos_full_notm}}. For more information, see [Object storage[(/docs/openwhisk?topic=openwhisk-pkg_obstorage). 

For all other actions, you cannot set private endpoints from within your action code. To connect to services such as a database service, you must use public endpoints.

## Getting help and support
{: #gettinghelp}

Still having issues with your function?
{: shortdesc}

-   To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page](https://cloud.ibm.com/status?selected=status){: external}.
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    -   If you have technical questions about developing functions with {{site.data.keyword.openwhisk}}, post your question on [Stack Overflow ](https://stackoverflow.com/search?q=ibm-cloud-functions){: external} and tag your question with `ibm-cloud-functions`.
-   See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
When you report an issue, include your activation ID. To get an activation ID, run `ibmcloud fn activation list`.
{: tip}


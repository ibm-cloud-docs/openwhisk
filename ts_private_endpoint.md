---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, endpoints

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Can't access private endpoint from action
{: #ts_private_endpoint}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

You cannot connect to a private endpoint from your action.
{: tsSymptoms}

The use of private endpoints from within your action code is not supported by {{site.data.keyword.openwhisk}}.
{: tsCauses}

With {{site.data.keyword.cloud_notm}} service endpoints, you can connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network. However, the only integration that supports private endpoints with {{site.data.keyword.openwhisk}} is {{site.data.keyword.cos_full_notm}}. For more information, see [Object storage](/docs/openwhisk?topic=openwhisk-pkg_obstorage). 
{: tsResolve}

For all other actions, you cannot set private endpoints from within your action code. To connect to services such as a database service, you must use public endpoints.



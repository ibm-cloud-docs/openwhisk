---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# System limits were reached
{: #ts_limit_reached}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Your function is not running properly and might result in a message like `{"error":"signal: killed"}`.
{: tsSymptoms}

All functions have minimum and maximum [system limits](/docs/openwhisk?topic=openwhisk-limits), such as memory usage maximum or a timeout.
{: tsCauses}

Some limits can be increased if a business case can justify higher safety limit values. To increase the limit value, contact IBM support by opening a ticket directly from the IBM Cloud Functions web console.
{: tsResolve}

1. Select **Support**.
2. Select **Add Ticket** from the drop-down menu.
3. Select **Technical** for the ticket type.
4. Select **Functions** for Technical area of support.



---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration, api gateway

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.apigw_short}} fails with HTTP status code 504
{: #ts_api_gateway_504}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

When invoking an action from the {{site.data.keyword.apigw_short}}, the action returns an HTTP 504 Gateway
timeout error.
{: tsSymptoms}

The {{site.data.keyword.apigw_short}} calls actions by using the `blocking=true` option. This option requires that the action completes within 60 seconds, even if the timeout of the action is longer. If you want to use {{site.data.keyword.apigw_short}} to call your actions, then your actions must complete within the 60 second timeframe. If your action does not complete within 60 seconds, then {{site.data.keyword.apigw_short}} returns an HTTP 504 Gateway timeout error.
{: tsCauses}

Make sure that your action completes within 60 seconds.
{: tsResolve}

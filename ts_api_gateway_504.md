---

copyright:
  years: 2017, 2021
lastupdated: "2021-10-12"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration, api gateway

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.apigw_short}} fails with HTTP status code 504
{: #ts_api_gateway_504}

When invoking an action from the {{site.data.keyword.apigw_short}}, the action returns an HTTP 504 Gateway
timeout error.
{: tsSymptoms}

The {{site.data.keyword.apigw_short}} calls actions by using the `blocking=true` option. This option requires that the action completes within 60 seconds, even if the timeout of the action is longer. If you want to use {{site.data.keyword.apigw_short}} to call your actions, then your actions must complete within the 60 second timeframe. If your action does not complete within 60 seconds, then {{site.data.keyword.apigw_short}} returns an HTTP 504 Gateway timeout error.
{: tsCauses}

Make sure that your action completes within 60 seconds.
{: tsResolve}

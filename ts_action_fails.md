---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Action is failing
{: #ts_action_fails}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

The action is failing.
{: tsSymptoms}

One explanation for a failing app is that a deprecated runtime is being used. An action cannot be completed successfully until the runtime is updated to a supported one.
{: tsCauses}

To see which runtime is used for the action, run `ibmcloud fn action get ACTION_NAME` and check for `deprecated=true` in the query response. If the runtime is deprecated, [update the runtime](/docs/openwhisk?topic=openwhisk-actions#actions_update). For more information, see [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).
{: tsResolve}



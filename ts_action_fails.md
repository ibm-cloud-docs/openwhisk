---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-03"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration, api gateway

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Action is failing
{: #ts_action_fails}

The action is failing.
{: tsSymptoms}

One explanation for a failing app is that a deprecated runtime is being used. An action cannot be completed successfully until the runtime is updated to a supported one.
{: tsCauses}

To see which runtime is used for the action, run `ibmcloud fn action get ACTION_NAME` and check for `deprecated=true` in the query response. If the runtime is deprecated, [update the runtime](/docs/openwhisk?topic=openwhisk-actions#actions_update). For more information, see [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).
{: tsResolve}



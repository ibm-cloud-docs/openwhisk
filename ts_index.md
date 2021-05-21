---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-21"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration, api gateway

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


## Action terminates after one minute
{: #ts_action_terminated}

{: tsSymptoms}
You are invoking an action that returns after one min with an `http code 202` and the result is only showing the activation ID.

{: tsCauses}
When invoking an action, there are two modes possible: blocking or non-blocking. The default for regular action invocations is `non-blocking` and for web actions, it is `blocking`. Blocking invocations use a request-response style and wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's timeout limit. At the end of the wait period (for example, after 60 sec), all invocations switch to non-blocking and instead of the result, these actions return the activation ID.

**Example output**

```
{
  "activationId": "27eca80056d54f93aca80056d5cf93b9"
}
```
{: codeblock}

If an invocation of a web action reaches the end of wait period, the response shows both the activation ID and the transaction ID as well as an indication that the request is returned, but the action continues to run.

**Example output**

```
{
  "activationId": "d13cfd3ce4b14f7cbcfd3ce4b11f7cce",
  "code": "42c15dc7f450df1e9a01104de158d489",
  "error": "Response not yet ready."
}
```
{: codeblock}

{: tsResolve}
With the activation ID, you can poll for the completion of the action and the result. For more information, see [CLI](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_activation) or [REST API](https://cloud.ibm.com/apidocs/functions#getactivations) documentation.

For more information about blocking actions, see [Testing blocking actions](/docs/openwhisk?topic=openwhisk-test#test-block).

## {{site.data.keyword.apigw_short}} fails with HTTP status code 504
{: #ts_api_gateway_504}

{: tsSymptoms}
When invoking an action from the {{site.data.keyword.apigw_short}}, the action returns an HTTP 504 Gateway
Timeout error.

{: tsCauses}
The {{site.data.keyword.apigw_short}} calls actions by using the `blocking=true` option. This option requires that the action completes within 60 seconds, even if the timeout of the action is longer. If you want to use {{site.data.keyword.apigw_short}} to call your actions, then your actions must complete within the 60 second time frame. If your action does not complete within 60 seconds, then {{site.data.keyword.apigw_short}} returns an HTTP 504 Gateway Timeout error.

{: tsResolve}
Make sure that your action completes within 60 seconds.

## Can't access private endpoint from action
{: #ts_private_endpoint}

{: tsSymptoms}
You cannot connect to a private endpoint from your action.

{: tsCauses}
The use of private endpoints from within your action code is currently not supported by {{site.data.keyword.openwhisk}}.

{: tsResolve}
With {{site.data.keyword.cloud_notm}} service endpoints, you can connect to {{site.data.keyword.cloud_notm}} services over the {{site.data.keyword.cloud_notm}} private network. However, the only integration that supports private endpoints with {{site.data.keyword.openwhisk}} is {{site.data.keyword.cos_full_notm}}. For more information, see [Object storage](/docs/openwhisk?topic=openwhisk-pkg_obstorage). 

For all other actions, you cannot set private endpoints from within your action code. To connect to services such as a database service, you must use public endpoints.

---

copyright:
  years: 2021, 2023
lastupdated: "2023-09-19"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Action terminates after one minute
{: #ts_action_terminated}

You are invoking an action that returns after one min with an `http code 202` and the result is only showing the activation ID.
{: tsSymptoms}

When invoking an action, there are two modes possible: blocking or non-blocking. The default for regular action invocations is `non-blocking` and for web actions, it is `blocking`. Blocking invocations use a request-response style and wait for the activation result to be available. The wait period is the lesser of 60 seconds or the action's timeout limit. At the end of the wait period (for example, after 60 sec), all invocations switch to non-blocking and instead of the result, these actions return the activation ID.
{: tsCauses}

The following example shows possible output.

```sh
{
    "activationId": "27eca80056d54f93aca80056d5cf93b9"
}
```
{: codeblock}

If an invocation of a web action reaches the end of wait period, the response shows both the activation ID and the transaction ID as well as an indication that the request is returned, but the action continues to run.

The following example shows possible output.

```sh
{
    "activationId": "d13cfd3ce4b14f7cbcfd3ce4b11f7cce",
    "code": "42c15dc7f450df1e9a01104de158d489",
    "error": "Response not yet ready."
}
```
{: codeblock}

With the activation ID, you can poll for the completion of the action and the result. For more information, see [CLI](/docs/openwhisk?topic=openwhisk-functions-cli#cli_activation).
{: tsResolve}

For more information about blocking actions, see [Testing blocking actions](/docs/openwhisk?topic=openwhisk-test#test-block).



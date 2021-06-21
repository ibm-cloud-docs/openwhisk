---

copyright:
  years: 2021
lastupdated: "2021-06-18"

keywords: troubleshooting actions, functions, help, support, action, troubleshoot, system limits, configuration, api gateway

subcollection: openwhisk

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Action terminates after one minute
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

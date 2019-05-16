---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Troubleshooting
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
To see which runtime is used for the action, run `ibmcloud fn action get ACTION_NAME` and check for `deprecated=true` in the query response. If the runtime is deprecated, [update the runtime](/docs/openwhisk?topic=cloud-functions-actions#actions_update).


<br />


## System limits were reached
{: #ts_limit_reached}

{: tsSymptoms}
Your function is not running properly because a limit has been reached, such as memory usage maximum or a timeout.

{: tsCauses}
All functions have minimum and maximum [system limits](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits).

{: tsResolve}
Some limits can be increased if a business case can justify higher safety limit values. To increase the limit value, contact IBM support by opening a ticket directly from the IBM Cloud Functions web console.

1. Select **Support**.
2. Select **Add Ticket** from the drop down menu.
3. Select **Technical** for the ticket type.
4. Select **Functions** for Technical area of support.


<br />


## Getting help and support
{: #gettinghelp}

Still having issues with your function?
{: shortdesc}

-   To see whether {{site.data.keyword.Bluemix_notm}} is available, [check the {{site.data.keyword.Bluemix_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?selected=status).
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.
    -   If you have technical questions about developing functions with {{site.data.keyword.openwhisk}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/search?q=ibm-cloud-functions) and tag your question with with `ibm-cloud-functions`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix) forum. Include the `ibm-cloud` and `functions` tags.
    See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
When you report an issue, include your activation ID. To get an activation ID, run `ibmcloud fn activation list`.
{: tip}

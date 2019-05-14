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
To see which runtime is used for the action, run `ibmcloud fn action get ACTION_NAME` and check for `deprecated=true` in the query response. If the runtime is deprecated, [update the runtime](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)



## System limits were reached
{: #ts_limit_reached}

{: tsSymptoms}
Your function is not running properly because a limit has been reached, such as memory usage maximum or a timeout.

{: tsCauses}
All functions have minimum and maximum [system limits](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits).

{: tsResolve}
Some limits can be increased if a business case can justify higher safety limit values. To increase the limit value, contact IBM support by opening a ticket directly from the IBM Cloud Functions web console.

1. Select **Support**.
2. Select **Add Ticket** from the drop down menu.
3. Select **Technical** for the ticket type.
4. Select **Functions** for Technical area of support.



## Getting help and support
{: #gettinghelp}

You can get help by searching for information or by asking questions through a forum. You can also open a support ticket. When you are using the forums to ask a question, be sure to tag your question so that it is seen by the {{site.data.keyword.Bluemix_notm}} development teams.

  * If you have technical questions about {{site.data.keyword.openwhisk_short}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/search?q=ibm-cloud-functions) and tag your question with "ibm-cloud-functions".

For more information about getting support, see [How do I get the support that I need?](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).

---

copyright:
  years: 2023
lastupdated: "2023-11-01"

keywords: troubleshoot, functions, get help, support, status, issues

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting overview
{: #troubleshooting}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Review some general help for troubleshooting issues with {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## General ways to resolve issues
{: #help-general}

* Make sure that your command-line tools are up to date.
    * In the command-line, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use all available commands and flags.
    * Update the `ibmcloud fn` CLI plug-in whenever an update is available. For more information, see [Updating the {{site.data.keyword.openwhisk_short}} CLI plug-in](/docs/openwhisk?topic=openwhisk-cli_install#cli_update)
* Keep the your runtime up-to-date. For more information, see [Runtimes](/docs/openwhisk?topic=openwhisk-runtimes).
* Review the other troubleshooting issues for {{site.data.keyword.openwhisk_short}}.
* Enable and review [logging](/docs/openwhisk?topic=openwhisk-logs) and [monitoring](/docs/openwhisk?topic=openwhisk-monitor-functions) details to troubleshoot your {{site.data.keyword.openwhisk}} components.

## Reviewing cloud issues and status
{: #help-cloud-status}

1. To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page](https://cloud.ibm.com/status?selected=status){: external}.
2. Filter for the **Functions** component and review any cloud status issue.
3. Review the [System details and limits](/docs/openwhisk?topic=openwhisk-limits).
4. For issues in open source projects that are used by {{site.data.keyword.cloud_notm}}, see the [IBM open source and third-party policy](https://www.ibm.com/support/pages/node/737271){: external}.

## Getting help
{: #help-functions}

If you still cannot resolve your issue, see [Getting support](/docs/openwhisk?topic=openwhisk-gettinghelp). For any general questions or feedback, post in Slack.



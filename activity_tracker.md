---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications, functions

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



# Viewing {{site.data.keyword.cloudaccesstrailshort}} events
{: #activity_tracker}

You can view, manage, and audit user-initiated activities made in your {{site.data.keyword.openwhisk}} service instance by using the {{site.data.keyword.cloudaccesstrailshort}} service.
{: shortdesc}


For more information about how the service works, see the [{{site.data.keyword.cloudaccesstrailshort}} docs](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started).


## Where to view events
{: #view}

Events are available in the {{site.data.keyword.cloudaccesstrailshort}} **account domain** that is available in the {{site.data.keyword.cloud_notm}} region where the {{site.data.keyword.openwhisk_short}} namespace resource is available. For more information, see [Viewing account events](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events).

1. Log in to your {{site.data.keyword.cloud_notm}} account.
2. From the catalog, provision an instance of the {{site.data.keyword.cloudaccesstrailshort}} service in the same account as your instance of {{site.data.keyword.openwhisk}}.
3. On the **Manage** tab of the {{site.data.keyword.cloudaccesstrailshort}} dashboard, click the **View in Kibana**.
4. Set the time frame that you want to view logs for. The default is 15 min.
5. In the **Available Fields** list, click **type**. Click the magnifying glass icon for **Activity Tracker** to limit the logs to only those tracked by the service.
6. You can use the other available fields to narrow your search.

For users other than the account owner to view logs, you must use the premium plan. To let other users view events, see [Granting permissions to see account events](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}


## List of events
{: #events}

Check out the following table for a list of events that are sent to {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Action</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>Create a IAM-based namespace resource</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Migrate a Cloud Foundry-based namespace to become IAM-enabled</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>Update IAM-based namespace resource properties, such as the display name or description</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>Delete a namespace resource</td>
    </tr>
  </tbody>
</table>




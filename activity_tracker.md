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
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Viewing {{site.data.keyword.cloudaccesstrailshort}} events
{: #activity_tracker}

You can view, manage, and audit user-initiated activities made in your {{site.data.keyword.openwhisk}} service instance by using the {{site.data.keyword.at_full_notm}} service.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [Getting Started Tutorial for {{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).


For more information about how the service works, see the [{{site.data.keyword.at_full_notm}} docs](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started).


## List of events
{: #events}

The following list of {{site.data.keyword.openwhisk}} events are sent to {{site.data.keyword.at_full_notm}}.
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
      <td><code>functions.namespace.create</code></td>
      <td>Create a IAM-based namespace resource.</td>
    </tr>
    <tr>
      <td><code>functions.namespace.migrate</code></td>
      <td>Migrate a Cloud Foundry-based namespace to become IAM-enabled.</td>
    </tr>
    <tr>
      <td><code>functions.namespace.update</code></td>
      <td>Update IAM-based namespace resource properties, such as the display name or description.</td>
    </tr>
    <tr>
      <td><code>functions.namespace.delete</code></td>
      <td>Delete a namespace resource.</td>
    </tr>
  </tbody>
</table>


## Where to view events
{: #view}

{{site.data.keyword.openwhisk_short}} sends audit logs to the {{site.data.keyword.at_full_notm}} with LogDNA service of the same region as the {{site.data.keyword.openwhisk_short}} namespace. This means that audit logs of a {{site.data.keyword.openwhisk_short}} namespace in `us-south` are sent to a LogDNA instance in `us-south`.

Audit logs for {{site.data.keyword.openwhisk_short}} namespaces in `us-east` are sent to a LogDNA instance in `us-south`.
{: note} 



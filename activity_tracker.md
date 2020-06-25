---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-23"

keywords: events, serverless, functions, activity tracker

subcollection: openwhisk

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

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to follow regulatory audit requirements. You can also be alerted about actions as they happen. The events that are collected follow the Cloud Auditing Data Federation (CADF) standard. For more information, see the [Getting Started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started).


## List of events
{: #events}

The following list of {{site.data.keyword.openwhisk}} events is sent to {{site.data.keyword.at_full_notm}}.
{: shortdesc}

<table>
  	<col style="width:40%">
	<col style="width:60%">
  <thead>
    <tr>
      <th>Action</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>functions.action.create</code></td>
            <td>Create or update an action.</td>
    </tr>
        <tr>
      <td><code>functions.action.get</code></td>
            <td>Get action information.</td>
    </tr>
        <tr>
      <td><code>functions.action.delete</code></td>
            <td>Delete an action.</td>
    </tr>
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
        <tr>
      <td><code>functions.package.create</code></td>
            <td>Create or update a package.</td>
    </tr>
        <tr>
      <td><code>functions.package.get</code></td>
            <td>Get package information.</td>
    </tr>
            <tr>
      <td><code>functions.package.delete</code></td>
            <td>Delete a package.</td>
    </tr>
            <tr>
      <td><code>functions.trigger.create</code></td>
            <td>Create or update a trigger.</td>
    </tr>
            <tr>
      <td><code>functions.trigger.get</code></td>
            <td>Get trigger information.</td>
    </tr>
                <tr>
      <td><code>functions.trigger.delete</code></td>
            <td>Delete a trigger.</td>
    </tr>
                <tr>
      <td><code>functions.rule.create</code></td>
            <td>Create or update a rule.</td>
    </tr>
                <tr>
      <td><code>functions.rule.get</code></td>
            <td>Get rule information.</td>
    </tr>            <tr>
      <td><code>functions.rule.delete</code></td>
            <td>Delete a rule.</td>
    </tr>
  </tbody>
</table>


## Where to view events
{: #view}

{{site.data.keyword.openwhisk_short}} sends audit logs to the {{site.data.keyword.at_full_notm}} service of the same region as the {{site.data.keyword.openwhisk_short}} namespace. For example, audit logs of a {{site.data.keyword.openwhisk_short}} namespace in `us-south` are sent to a LogDNA instance in `us-south`.

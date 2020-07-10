---

copyright:
  years: 2017, 2020
lastupdated: "2020-07-09"

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
            <td>Create an action.</td>
    </tr>
        <tr>
      <td><code>functions.action.read</code></td>
            <td>Get action information.</td>
    </tr>
	  <tr>
      <td><code>functions.action.update</code></td>
            <td>Update an action. Note that the event does not include the changes between the initial value and the new value. An update of an action might also replace the code that is associated with the request. Consider using a version control system to maintain your code changes and to find the delta between versions.</td>
    </tr>
        <tr>
      <td><code>functions.action.delete</code></td>
            <td>Delete an action.</td>
    </tr>
    <tr>
      <td><code>functions.namespace.create</code></td>
      <td>Create an IAM-based namespace resource.</td>
    </tr>
<tr>
      <td><code>functions.namespace.read</code></td>
      <td>Get IAM-based namespace resource information.</td>
    </tr>
    <tr>
      <td><code>functions.namespace.migrate</code></td>
      <td>Migrate a Cloud Foundry-based namespace to become an IAM-enabled namespace.</td>
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
            <td>Create a package.</td>
    </tr>
        <tr>
      <td><code>functions.package.read</code></td>
            <td>Get package information.</td>
    </tr>
<tr>
      <td><code>functions.package.update</code></td>
            <td>Update a package. Note that the event does not include the changes between the initial value and the new value.</td>
    </tr>
            <tr>
      <td><code>functions.package.delete</code></td>
            <td>Delete a package.</td>
    </tr>
            <tr>
      <td><code>functions.trigger.create</code></td>
            <td>Create a trigger.</td>
    </tr>
            <tr>
      <td><code>functions.trigger.read</code></td>
            <td>Get trigger information.</td>
    </tr>            
<tr>
      <td><code>functions.trigger.update</code></td>
            <td>Update a trigger. Note that the event does not include the changes between the initial value and the new value.</td>
    </tr>
                <tr>
      <td><code>functions.trigger.delete</code></td>
            <td>Delete a trigger.</td>
    </tr>
                <tr>
      <td><code>functions.rule.create</code></td>
            <td>Create a rule.</td>
    </tr>
<tr>
      <td><code>functions.rule.enable</code></td>
            <td>Enable a rule.</td>
    </tr>
<tr>
      <td><code>functions.rule.disable</code></td>
            <td>Disable a rule.</td>
    </tr>
     <tr>
      <td><code>functions.rule.read</code></td>
            <td>Get rule information.</td>
    </tr>
	   <tr>
      <td><code>functions.rule.update</code></td>
            <td>Update a rule. Note that the event does not include the changes between the initial value and the new value.</td>
    </tr>
<tr>
      <td><code>functions.rule.delete</code></td>
            <td>Delete a rule.</td>
    </tr>
  </tbody>
</table>


## Viewing events
{: #view}

{{site.data.keyword.openwhisk_short}} sends audit logs to the [{{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started) service of the same region as the {{site.data.keyword.openwhisk_short}} namespace. For example, audit logs of a {{site.data.keyword.openwhisk_short}} namespace in `us-south` are sent to a LogDNA instance in `us-south`. For more information about setting up {{site.data.keyword.at_full_notm}}, see [Provisioning an instance](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-provision).

## Analyzing events
{: #at_events_analyze}

After viewing events that are captured by {{site.data.keyword.at_full_notm}}, you can then analyze the events.
{: shortdesc}

**Identifying the {{site.data.keyword.openwhisk_short}} namespace that generates the event**

To identify the namespace for which the event was generated, look at the `target.id` field. You can use this field to filter events in LogDNA, for example, showing events for only a specific namespace. 

You can use the CLI to find details about your [namespaces](/docs/openwhisk?topic=openwhisk-namespaces#how-do-i-see-a-list-of-my-cloud-functions-namespaces-).

**Getting the unique ID of a request**

Each action that you perform on an {{site.data.keyword.openwhisk_short}} namespace resource has a unique ID.

To find the unique ID of a request, look at the `requestData.requestId` value that is set in the `requestData` field.

**Getting request details**

The `requestData` field contains the name of the namespace entity such as action, package, trigger, or rule for which the event is generated. This is named according to the entity, for example, the name `requestData.actionName` is used for actions. 

Additionally, the `requestData` field contains information about the `userAgent`. If the action was initiated through the CLI, this field starts with `CloudFunctions-Plugin`, if it is initiated through the Cloud Console (UI), the value is `IBM Cloud Functions UI`.   

**Getting information for failures**

All events that are issued for failed actions display `failure` in the `outcome` field, and in addition provide more details as part of the `reason` field. Note that the `reason.reasonForFailure` field might be especially helpful, as it contains details of the failure. 

**Custom views**

For more information about generating custom views by using event fields, see [Creating custom views in LogDNA](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-views).

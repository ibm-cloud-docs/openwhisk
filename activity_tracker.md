---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-12"

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

# Auditing events for {{site.data.keyword.openwhisk_short}}
{: #at_events}

You can view, manage, and audit user-initiated activities made in your {{site.data.keyword.openwhisk_short}} service instance by using the {{site.data.keyword.at_full_notm}} service.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to follow regulatory audit requirements. You can also be alerted about actions as they happen. The events that are collected follow the Cloud Auditing Data Federation (CADF) standard. For more information, see the [Getting Started tutorial for {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

## List of events
{: #events}

The following list of {{site.data.keyword.openwhisk_short}} events is sent to {{site.data.keyword.at_full_notm}}.
{: shortdesc}

### Action events
{: #action-events}

The following activities generate action events.

| Action          | Description      | 
|-----------------|------------------|
| `functions.action.create` | Create an action. | 
| `functions.action.read` | Get action information. |
| `functions.action.activate` | Invoke an action. Note that this event is logged only when errors occur. |
| `functions.action.update` | Update an action. Note that the event does not include the changes between the initial value and the new value. An update of an action might also replace the code that is associated with the request. Consider setting up a version control system to maintain your code changes and to find the delta between versions. |
| `functions.action.delete` | Delete an action. |
{: caption="Table 1. Activities that generate action events" caption-side="top"}

### Namespace events
{: #namespace-events}

The following activities generate namespace events.

| Action          | Description      | 
|-----------------|------------------|
| `functions.namespace.create` | Create an IAM-based namespace resource. | 
| `functions.namespace.read` | Get IAM-based namespace resource information. |
| `functions.namespace.migrate` | Migrate a Cloud Foundry-based namespace to become an IAM-enabled namespace. |
| `functions.namespace.update` | Update IAM-based namespace resource properties, such as the display name or description. |
| `functions.namespace.delete` | Delete a namespace resource. |
{: caption="Table 2. Activities that generate namespace events" caption-side="top"}

### Package events
{: #package-events}

The following activities generate package events.

| Action          | Description      | 
|-----------------|------------------|
| `functions.package.create` | Create a package. | 
| `functions.package.read` | Get package information. |
| `functions.package.update` | Update a package. Note that the event does not include the changes between the initial value and the new value. |
| `functions.package.delete` | Delete a package. |
{: caption="Table 3. Activities that generate package events" caption-side="top"}

### Trigger events
{: #trigger-events}

The following activities generate trigger events.

| Action          | Description      | 
|-----------------|------------------|
| `functions.trigger.create` | Create a trigger. | 
| `functions.trigger.read` | Get trigger information. |
| `functions.trigger.activate` | Fire a trigger. Note that this event is logged only when errors occur. |
| `functions.trigger.update` | Update a trigger. Note that the event does not include the changes between the initial value and the new value. |
| `functions.trigger.delete` | Delete a trigger. |
{: caption="Table 4. Activities that generate trigger events" caption-side="top"}

### Rule events
{: #rule-events}

The following activities generate rule events.

| Action          | Description      | 
|-----------------|------------------|
| `functions.rule.create` | Create a rule. | 
| `functions.rule.enable` | Enable a rule. |
| `functions.rule.disable` | Disable a rule. |
| `functions.rule.read` | Get rule information. |
| `functions.rule.update` | Update a rule. Note that the event does not include the changes between the initial value and the new value. |
| `functions.rule.delete` | Delete a rule. |
{: caption="Table 5. Activities that generate rule events" caption-side="top"}

## Viewing events
{: #view}

{{site.data.keyword.openwhisk_short}} sends audit logs to the [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started) service of the same region as the {{site.data.keyword.openwhisk_short}} namespace. For example, audit logs of a {{site.data.keyword.openwhisk_short}} namespace in `us-south` are sent to a logging instance in `us-south`. For more information about setting up {{site.data.keyword.at_full_notm}}, see [Provisioning an instance](/docs/activity-tracker?topic=activity-tracker-provision).

## Analyzing events
{: #at_events_analyze}

After viewing events that are captured by {{site.data.keyword.at_full_notm}}, you can then analyze the events.
{: shortdesc}

**Identifying the {{site.data.keyword.openwhisk_short}} namespace that generates the event**.

To identify the namespace for which the event was generated, look at the `target.id` field. You can use this field to filter events, for example, showing events for only a specific namespace. 

You can use the CLI to find details about your [namespaces](/docs/openwhisk?topic=openwhisk-namespaces#how_list_namespace).

**Getting the unique ID of a request**

Each action that you perform on a {{site.data.keyword.openwhisk_short}} namespace resource has a unique ID.

To find the unique ID of a request, look at the `requestData.requestId` value that is set in the `requestData` field.

**Getting request details**

The `requestData` field contains the name of the namespace entity such as action, package, trigger, or rule for which the event is generated. This field is named according to the entity; for example, the name `requestData.actionName` is used for actions. 

Additionally, the `initiator.host.agent` field contains information about the user agent that sent the request. If the action was initiated through the CLI, this field starts with `CloudFunctions-Plugin`, if it is initiated through the Cloud Console (UI), the value is `IBM Cloud Functions UI`.   

**Getting information for failures**

All events that are issued for failed actions display `failure` in the `outcome` field, and in addition provide more details as part of the `reason` field. Note that the `reason.reasonForFailure` field might be especially helpful, as it contains details of the failure. 

**Custom views**

For more information about generating custom views by using event fields, see [Creating custom views](/docs/activity-tracker?topic=activity-tracker-views).

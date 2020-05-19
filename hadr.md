---

copyright:
  years: 2019, 2020
lastupdated: "2020-05-15"

keywords: cli, functions, high availability

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
{:external: target="_blank" .external}

# Understanding high availability and disaster recovery for {{site.data.keyword.openwhisk_short}}
{: #ha_dr}

Understand the high availability features of the {{site.data.keyword.openwhisk}} and design your resources to meet the availability requirements that your business and customers need.
{:shortdesc}

High availability is a core discipline in an IT infrastructure to keep your resources healthy and your app workloads up and running, even after a partial or a full site failure. The main purpose of high availability is to eliminate potential points of failure in an IT infrastructure.

Your global strategy is important, and you are responsible for understanding your configuration, customization, and usage of the service to best leverage the capabilities of {{site.data.keyword.cloud_notm}}. For more information, see [How does {{site.data.keyword.cloud_notm}} ensure zero downtime?](/docs/overview?topic=overview-zero-downtime#zero-downtime){: external}.

**Who is responsible to set up high availability for my resources?**

While {{site.data.keyword.openwhisk_short}} is responsible to ensure that your namespace information is available, backed up, and replicated across multiple regions so that information can be recovered after a failure, {{site.data.keyword.openwhisk_short}} does not set up high availability for any {{site.data.keyword.cloud_notm}} resources that are used by {{site.data.keyword.openwhisk_short}}. Instead, you must understand the high availability options that each resource offering provides to implement the right level of availability for your needs.

## High availability

{{site.data.keyword.openwhisk_short}} is highly available within multiple [{{site.data.keyword.cloud_notm}} regions](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions). {{site.data.keyword.openwhisk_short}} supports high availability to avoid a single point of failure. This service achieves high availability automatically and transparently through the multi-zone region (MZR) feature provided by {{site.data.keyword.cloud_notm}}.

## Disaster recovery

Customer data, such as action code, is automatically synchronized between database instances in different zones. In addition, {{site.data.keyword.cloud_notm}} backs up your data by taking snapshots for use in data restoration in a disaster situation. In the case of a disaster, {{site.data.keyword.openwhisk_short}} switches over to use these snapshot backups automatically.

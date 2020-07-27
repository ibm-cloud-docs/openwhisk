---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-23"

keywords: cli, functions, high availability

subcollection: openwhisk

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

# High availability and disaster recovery
{: #ha_dr}

This chapter helps you to understand the high availability features of the {{site.data.keyword.openwhisk}} and design your resources to meet the availability requirements that your business and customers need.
{:shortdesc}

High availability is a core discipline in an IT infrastructure to keep your resources healthy and your app workloads up and running, even after a partial or a full site failure. The main purpose of high availability is to eliminate potential points of failure in an IT infrastructure.

**Regions and Service Level**

All IBM Cloud® general availability (GA) services have a Service Level Agreement of 99.99% availability. IBM® Cloud Functions is a GA service that is offered in five regions: US South, US East, Germany, Tokyo, and the United Kingdom. Each location has three different data centers for redundancy. The data for each location is kept in the three data centers near that location. If all the data centers in a location fail, the Cloud Functions service for that location becomes unavailable.

Cloud Functions is available only on the public cloud, but the tools that are used with Cloud Functions can run in dedicated or public cloud environments and in on-premises environments.

**Who is responsible to set up high availability for my resources?**

Your global strategy is important, and you are responsible for understanding your configuration, customization, and usage of the service to best leverage the capabilities of {{site.data.keyword.cloud_notm}}. For more information, see [How does {{site.data.keyword.cloud_notm}} ensure zero downtime?](/docs/overview?topic=overview-zero-downtime#zero-downtime){: external}.

While {{site.data.keyword.openwhisk_short}} is responsible to ensure that your namespace information is available, backed up, and replicated across multiple regions so that information can be recovered after a failure, {{site.data.keyword.openwhisk_short}} does not set up high availability for any {{site.data.keyword.cloud_notm}} resources that are used by {{site.data.keyword.openwhisk_short}}. Instead, you must understand the high availability options that each resource offering provides to implement the right level of availability for your needs.

See [ensure zero downtime](/docs/overview?topic=overview-zero-downtime#zero-downtime) to learn more about the high availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}. You can also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).

The following chapters have information about high availability, disaster recovery for {{site.data.keyword.openwhisk_short}} as well as HIPAA® related considerations. 

## High availability

{{site.data.keyword.openwhisk_short}} is highly available within multiple [{{site.data.keyword.cloud_notm}} regions](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions). {{site.data.keyword.openwhisk_short}} supports high availability to avoid a single point of failure. This service achieves high availability automatically and transparently through the multi-zone region (MZR) feature provided by {{site.data.keyword.cloud_notm}}.

## Disaster recovery

Customer data, such as action code, is automatically synchronized between database instances in different zones. In addition, {{site.data.keyword.cloud_notm}} backs up your data by taking snapshots for use in data restoration in a disaster situation. In the case of a disaster, {{site.data.keyword.openwhisk_short}} switches over to use these snapshot backups automatically.

{{site.data.keyword.openwhisk_short}} is a Function as a Service offering, and as such it offers compute services only. It does not backup the data that your workload (actions) processes. In order to process data, you will use separate cloud based datastores with separate disaster recovery options, dependent on the storage service's SLAs.

## HIPAA®

{{site.data.keyword.openwhisk_short}} is a Function as a Service offering, and as such it offers compute services only. It does not offer storage options nor management of your data. Ensuring the HIPAA® compliant processing of your data is subject to your storage service's operation and SLAs.

---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-28"

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

Learn about the high availability features of the {{site.data.keyword.openwhisk}} and design your resources to meet the availability requirements that your business and customers need.
{:shortdesc}

High availability is a core discipline in an IT infrastructure to keep your resources healthy and your app workloads up and running, even after a partial or a full site failure. The main purpose of high availability is to eliminate potential points of failure in an IT infrastructure.

**Regions and service level**

All {{site.data.keyword.cloud_notm}} general availability (GA) services have a Service Level Agreement of 99.99% availability. {{site.data.keyword.openwhisk_short}} is a GA service that is offered in five regions: US South, US East, EU Central, Tokyo, and UK South. Each location uses different availability zones for redundancy. If all the availability zones in a location fail, the {{site.data.keyword.openwhisk_short}} service for that location becomes unavailable.

{{site.data.keyword.openwhisk_short}} is available only on the public cloud.

**Who is responsible to set up high availability for my resources?**

Your global strategy is important, and you are responsible for understanding your configuration, customization, and usage of the service to best leverage the capabilities of {{site.data.keyword.cloud_notm}}. For more information, see the topic [How does {{site.data.keyword.cloud_notm}} ensure zero downtime?](/docs/overview?topic=overview-zero-downtime#zero-downtime){: external}

While {{site.data.keyword.openwhisk_short}} is responsible to ensure that your namespace information is available, backed up, and replicated across multiple regions so that information can be recovered after a failure, {{site.data.keyword.openwhisk_short}} does not set up high availability for any {{site.data.keyword.cloud_notm}} resources that are used by {{site.data.keyword.openwhisk_short}}. Instead, you must understand the high availability options that each resource offering provides to implement the right level of availability for your needs.

See [ensure zero downtime](/docs/overview?topic=overview-zero-downtime#zero-downtime) to learn more about the high availability and disaster recovery standards in {{site.data.keyword.cloud_notm}}. You can also find information about [Service Level Agreements](/docs/overview?topic=overview-slas).

The following sections contain information about high availability, disaster recovery for {{site.data.keyword.openwhisk_short}} as well as HIPAA-related considerations. 

## High availability

{{site.data.keyword.openwhisk_short}} is highly available within multiple [{{site.data.keyword.cloud_notm}} regions](/docs/openwhisk?topic=openwhisk-cloudfunctions_regions). {{site.data.keyword.openwhisk_short}} supports high availability to avoid a single point of failure. This service achieves high availability automatically and transparently through the multi-zone region (MZR) feature provided by {{site.data.keyword.cloud_notm}}.

## Disaster recovery

Customer data, such as action code, is automatically synchronized between database instances in different zones. In addition, {{site.data.keyword.cloud_notm}} backs up your data by taking snapshots for use in data restoration in a disaster situation. If a disaster occurs, {{site.data.keyword.openwhisk_short}} switches over to use these snapshot backups automatically.

The likelihood of a region-wide disaster is low. However, if a disaster happens, {{site.data.keyword.IBM_notm}} executes disaster-recovery plans to restore the service into another region. {{site.data.keyword.openwhisk}} then uses the regional backup of your data and the compute resources of another region to process your requests.

| Location | Failover location | 
| --------- | -------- |
| US South | US East |
| US East | US South |
| Tokyo | EU Central |
| UK South | EU Central |
| EU Central | Amsterdam |
{: caption="{{site.data.keyword.openwhisk}} regional failover locations" caption-side="top"}

Because there are no active compute resources in Amsterdam for {{site.data.keyword.openwhisk}}, the failover period in the EU Central region might be longer than in other regions.

{{site.data.keyword.openwhisk_short}} is a Function-as-a-Service offering, and as such it offers only compute services. It does not back up the data that your workload (actions) processes. In order to store data, you can use separate cloud-based datastores with separate disaster recovery options, which depends on the storage service's SLAs.

## HIPAA

{{site.data.keyword.openwhisk_short}} is a Function-as-a-Service offering, and as such it offers only compute services. It does not offer storage options nor management of your data. Ensuring the HIPAA-compliant processing of your data is subject to your storage service's operation and SLAs.

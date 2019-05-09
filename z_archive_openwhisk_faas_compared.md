---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: functions compared, openwhisk, architecture, limitless

subcollection: cloud-functions

---






{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Function as a Service architecture
{: #openwhisk_faas_compared}

{{site.data.keyword.openwhisk}} delivers OpenWhisk in a highly scalable, serverless environment. You can compare {{site.data.keyword.openwhisk_short}}'s serverless architecture and cost-effective computing with other architecture models.
{: shortdesc}

## Comparison of OpenWhisk architectures
{: #architecture_comparison}

The following OpenWhisk architectures are compared:

1. **Function as a Service (FaaS)** on [{{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk). IBM is the only vendor to offer managed OpenWhisk. A good introduction to the serverless programming model by using a FaaS platform is available on [Martin Fowler's blog](https://martinfowler.com/articles/serverless.html), and you can see [uses cases](/docs/openwhisk?topic=cloud-functions-openwhisk_common_use_cases) for running OpenWhisk with a serverless design.

2. **Infrastructure as a Service (IaaS)** with OpenWhisk Roll Your Own (RYO). You can download OpenWhisk from Apache Incubation Project and run it on [{{site.data.keyword.Bluemix_notm}} IaaS](https://cloud.ibm.com/catalog/?category=devices).

3. **Platform as a Service (PaaS)** as a managed application runtime. A good example is the [Liberty for Java](https://cloud.ibm.com/catalog/starters/liberty-for-java) runtime that is managed by the {{site.data.keyword.Bluemix_notm}} Foundry implementation.

4. **Container as a Service (CaaS)** as a managed container environment. A good example is the [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started#container_index).

5. **Infrastructure as a Service (IaaS)** with Java EE runtime. A good example is the [WebSphere Application Server VM on {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog/services/websphere-application-server).

The following table compares elements of each architecture from the perspective of a developer that is creating and operating applications:


| Topic | (1) FaaS on {{site.data.keyword.openwhisk_short}} | (2) IaaS with OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS with Java EE |
| --- | --- | --- | --- | --- | --- |
|	Application unit	|	Single function (usually small block of code in JavaScript, Swift, or Docker container) - can be less than one Kb, but can be larger. Usually not more than few Kb.	|	Same as column (1)	|	Depends on the runtime used. An EAR or WAR file, or other language-specific application bundle, usually relatively large - Kb or even Mb with many services in a bundle, but can be as small as a single service.	|	Docker container is the unit of deployment.	|	VM with App Server with EAR or WAR file and other dependencies - usually sized at Gb.	|
|	Resource footprint	|	End user does not pay or care about memory, CPU, or other resources. Although the action does have some footprint, the user does not need to worry about it	|	High. The end user must first provision IaaS environment and only then install and configure OpenWhisk on top of it	|	Small. End user pays for memory and CPU for running apps, but nothing for apps that are not running	|	Small to Medium	|	High. End user has to pay for disk storage, memory, CPUs, and possibly other components when the app is running. When it is stopped only the storage costs occur	|
|	Installation and setup	|	None required	|	Hard - all done by the end user	|	None required	|	Moderate - Hardware, networking, OS, container mgmt tools provided by CaaSs vendor, images, connectivity, and instances by end user	|	Hard - Hardware, networking, OS, initial Java EE installation provided by vendor, additional configuration, clustering, scaling by end user	|
|	Provisioning time	|	Milliseconds	|	See columns (4) and (5)	|	Minutes	|	Minutes	|	Hours	|
|	Ongoing administration	|	None	|	Hard	|	None	|	Moderate	|	Hard	|
|	Elastic scaling	|	Each action is always instantly and inherently scaled depending on the load. No need to provision VMs or other resources in advance	|	Not provided - end user must provide compute capacity on IaaS and manage scaling of VMs. Once VMs are scaled, OpenWhisk scales actions automatically, but the resources must already be provisioned in advance	|	Automatic, but slow scaling. During increased load, for several minutes users might wait for scale action to complete. Auto scaling requires careful tuning	|	Automatic, but slow scaling. During increased load, for several minutes users might wait for scale action to complete. Auto scaling requires careful tuning	|	Not provided	|
|	Capacity planning	|	Not needed. FaaS automatically provides as much capacity as needed	|	Need to provision enough capacity in advance or script it	|	Some capacity planning is needed, but some automatic capacity increase is provided	|	Some capacity planning is needed, but some automatic capacity increase is provided	|	Need to statically provision enough capacity to handle peak workload	|
|	Persistent connections and state	|	Limited - cannot keep a persistent connection, except in cases of container caching. Generally state must be kept in external resource	|	Same as column (1)	|	Supported - can keep an open socket or connection for long times, can store state in memory between calls	|	Supported - can keep an open socket or connection for long times, can store state in memory between calls	|	Supported - can keep an open socket or connection for long times, can store state in memory between calls	|
|	Maintenance	|	None - The entire stack is managed by IBM.	|	Significant - Depends on the target environment, User must provision hardware, networking, OS, storage, DB, install and maintain OpenWhisk, and so on.	|	None - The entire stack is managed by vendor.	|	Significant - User must create and maintain custom images, deploy and manage containers, connections between containers, and so on.	|	Significant - User must allocate VMs, manage, and scale Java EE servers individually.	|
|	High Availability (HA) and Disaster Recovery (DR)	|	Inherent / no extra costs	|	Roll your own (RYO) 	|	Available at extra cost	|	Failed containers can be automatically restarted	|	Available at extra cost, semi-automatic. VMs can be automatically failed over	|
|	Security	|	Vendor provided	|	Roll your own (RYO)	|	Mix of RYO and vendor provided	|	Mix of RYO and vendor provided	|	Roll your own (RYO)	|
|	Developer velocity	|	Highest	|	Highest	|	Highest	|	Average	|	Slow	|
|	Resource utilization (idle resources that still need to be paid for)	|	Resources are never idle as they are invoked only upon request. When workload is absent, no cost or resource allocation occurs.	|	Because this option is using IaaS or CaaS - similar considerations apply as in columns (4) and (5)	|	Some resources can be idle, and autoscaling helps to eliminate idle resources. A number of running instances must always be present, and are likely to be used at less than 50% of their capacity. Stopped instances do not cost anything	|	Similar to column (3)	|	Some resources can be idle, but autoscaling is not supported. Some number of running instances must always be present, and are likely to be used at less than 50% of their capacity. Stopped instances can incur the cost of storage	|
|	Maturity	|	Early maturity	|	Early maturity	|	Early maturity	|	Moderate maturity	|	Highly mature	|
|	Resource limits	|	[Some limits exist](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)	|	Depends on allocated resources	|	No	|	No	|	No	|
|	Latency for rarely used services	|	Rare requests can initially see several seconds response time, but remain in ms range for subsequent requests	|	Depends	|	Low	|	Low	|	Low - assuming the system has enough resources	|
|	Sweet spot type of application	|	Event processing, IoT, Mobile backend, microservices. Definitely not for monolithic applications. See [use cases](/docs/openwhisk?topic=cloud-functions-openwhisk_common_use_cases)	|	Same as column (1), but when user wants to run on non-IBM cloud or run on-prem.	|	Web applications with 24x7 workload load, stateful services that need to keep the connection open for long periods of time. Can be used to run microservices or monolithic applications	|	Ideal for microservices applications.	|	Traditional enterprise applications that are migrated from on-prem to the cloud. Ideal for monolithic applications	|
|	Charging granularity and billing	|	[Per blocks of 100 milliseconds](https://cloud.ibm.com/openwhisk/learn/pricing)	|	Depends on implementation - If IaaS or CaaS are used, then similar considerations apply - See columns (4) and (5)	|	Usually charged per hour (rarely per minute) for bundle of resources (CPU + memory + some disk space)	|	Similar to column (3)	|	Similar to column (3)	|
|	Total Cost of Ownership (TCO)	|	For its sweet spot, applications are likely to cost an order of magnitude less than alternatives. Because resources are automatically scaled, over provisioning does not occur.	|	For cloud deployments, it is likely to be more expensive than OpenWhisk FaaS, but on-prem deployment can be cheaper than traditional architectures	|	Relatively low - The user does not need to provision or manage resources, and can focus on application development. Some level of over provisioning compared to serverless	|	Moderate - The user needs to provision and manage containers and application, and could see some level of over provisioning compared to serverless or PaaS	|	Relatively high - Consider that migration of legacy applications into the cloud native model could be prohibitively expensive, this can be a viable and economical choice for those apps.	|

## Cost considerations
{: #cost_considerations}

The infrastructure for your testing, staging, load testing, and other environments can be costly. It takes time to set them up, and because they usually operate 24x7, they are often underutilized and consume large amounts of capacity. By using a serverless architecture, costs for any number of environments are generated based on load instead of the number of environments defined.
{: shortdesc}

To estimate costs for a serverless application, you can use the [pricing calculator ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/learn/pricing).

### Limitless capacity
{: #limitless_capacity}

In traditional architectures, each service consumes the amount of capacity allocated to them, and you are billed for the capacity consumption. {{site.data.keyword.openwhisk_short}}'s serverless architecture reduces the constraint on the granularity of your microservices architecture.

When not in use, {{site.data.keyword.openwhisk_short}} costs nothing. Your code executes when there is an HTTP call, database state change, or other type of event that triggers the execution of your code. You get billed by millisecond of execution time rounded up to the nearest 100ms, not per hour of VM utilization regardless of whether that VM was doing useful work. Because you only pay when events are consumed and not based on the number of environments, you can break down your app into 100, 1000, or even more microservices.

### Run actions in any region
{: #actions_region}

In traditional architectures, code must be running in each region to be executed there and the infrastructure for that region must also be paid for. With {{site.data.keyword.openwhisk_short}}, actions can be deployed and made available to run in any region at no extra cost. You can increase the availability and resiliency of your code without the traditional cost restrictions.

### Redundancy by design
{: #redundancy_design}

In traditional architectures, apps must be redundant. With {{site.data.keyword.openwhisk_short}}, processes don't need to be highly available (HA) because serverless apps are stateless and request-event driven by design. By eliminating the need for explicitly creating redundancy, the stateless nature of serverless apps can significantly reduce infrastructure costs.


---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-19"

keywords: functions compared, architecture, limitless, functions, openwhisk, actions, redundancy

subcollection: openwhisk

---


{{site.data.keyword.attribute-definition-list}}


# Function as a Service architecture
{: #faas}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

{{site.data.keyword.openwhisk}} delivers OpenWhisk in a highly scalable, serverless environment. You can compare {{site.data.keyword.openwhisk_short}} serverless architecture and cost-effective computing with other architecture models.
{: shortdesc}

Why go serverless?
:    - No infrastructure management required
     - Minimal maintenance
     - Cost efficient
     - Scales easily
     - Fast!


## Comparison of OpenWhisk architectures
{: #faas_architecture}

The following OpenWhisk architectures are compared:

1. **Function as a Service (FaaS)** on [{{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/functions/){: external}. IBM is the only vendor to offer managed OpenWhisk. A good introduction to the serverless programming model by using a FaaS platform is available on [Martin Fowler's blog](https://martinfowler.com/articles/serverless.html){: external}, and you can see [uses cases](/docs/openwhisk?topic=openwhisk-use_cases) for running OpenWhisk with a serverless design.

2. **Infrastructure as a Service (IaaS)** with OpenWhisk Roll Your Own (RYO). You can download OpenWhisk from Apache Incubation Project and run it on [{{site.data.keyword.cloud_notm}} IaaS](https://cloud.ibm.com/catalog?category=compute){: external}.

3. **Platform as a Service (PaaS)** as a managed application runtime. A good example is the [Liberty for Java](https://cloud.ibm.com/catalog/starters/liberty-for-java){: external} runtime that is managed by the {{site.data.keyword.cloud_notm}} Foundry implementation.

4. **Container as a Service (CaaS)** as a managed container environment. A good example is the [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started).

5. **Infrastructure as a Service (IaaS)** with Java EE runtime. A good example is the [WebSphere Application Server VM on {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog/services/websphere-application-server){: external}.

The following table compares elements of each architecture from the perspective of a developer that is creating and operating applications:


| Topic | (1) FaaS on {{site.data.keyword.openwhisk_short}} | (2) IaaS with OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS with Java EE |
| --- | --- | --- | --- | --- | --- |
|	Application unit	|	Single function (usually small block of code in JavaScript, Swift, or Docker container) - can be less than one Kb, but can be larger. Usually not more than few Kb.	|	Same as column (1)	|	Depends on the runtime used. An EAR or WAR file, or other language-specific application bundle, usually relatively large - Kb or even Mb with many services in a bundle, but can be as small as a single service.	|	Docker container is the unit of deployment.	|	VM with App Server with EAR or WAR file and other dependencies - usually sized at GB.	|
|	Resource footprint	|	Small. The user does not pay or care about memory, CPU, or other resources. Although the action does have some footprint, the user does not need to worry about it.	|	High. The user must first provision IaaS environment and only then install and configure OpenWhisk on top of it.	|	Small. The user pays for memory and CPU for running apps, but nothing for apps that are not running.	|	Small to Medium	|	High. User has to pay for disk storage, memory, CPUs, and possibly other components when the app is running. When it is stopped, only the storage costs occur.	|
|	Installation and setup	|	None required	|	Hard. All installation is done by the user.	|	None required	|	Moderate. Hardware, networking, OS, and container management tools provided by CaaS vendor. Images, connectivity, and instances provided by the user.	|	Hard. Hardware, networking, OS, and initial Java EE installation provided by vendor, with additional configuration, clustering, and scaling by user.	|
|	Provisioning time	|	Milliseconds	|	See columns (4) and (5)	|	Minutes	|	Minutes	|	Hours	|
|	Ongoing administration	|	None	|	Hard	|	None	|	Moderate	|	Hard	|
|	Elastic scaling	|	Each action is always instantly and inherently scaled depending on the load. No need to provision VMs or other resources in advance.	|	Not provided. The user must provide compute capacity on IaaS and scale the VMs. After VMs are scaled, OpenWhisk scales actions automatically, but the resources must already be provisioned in advance.	|	Automatic, but slow to scale. During increased load, for several minutes users might wait for scale action to complete. Auto scaling requires careful tuning.	|	Automatic, but slow to scale. During increased load, for several minutes users might wait for scale action to complete. Auto scaling requires careful tuning.	|	Not provided	|
|	Capacity planning	|	Not needed. FaaS automatically provides as much capacity as needed.	|	Need to provision enough capacity in advance or script it.	|	Some capacity planning is needed, but some automatic capacity increase is provided.	|	Some capacity planning is needed, but some automatic capacity increase is provided.	|	Need to statically provision enough capacity to handle peak workload.	|
|	Persistent connections and state	|	Limited. You cannot keep a persistent connection, except in cases of container caching. Generally, state must be kept in external resource.	|	Same as column (1)	|	Supported. You can keep an open socket or connection for long times and can store state in memory between calls.	|	Supported. You can keep an open socket or connection for long times and can store state in memory between calls.	|	Supported. You can keep an open socket or connection for long times and can store state in memory between calls.	|
|	Maintenance	|	None. The entire stack is managed by IBM.	|	Significant, depending on the target environment, User must provision hardware, networking, OS, storage, DB, install and maintain OpenWhisk, and so on.	|	None. The entire stack is managed by vendor.	|	Significant. User must create and maintain custom images, deploy and manage containers, connections between containers, and so on.	|	Significant. User must allocate VMs, manage, and scale Java EE servers individually.	|
|	High Availability (HA) and Disaster Recovery (DR)	|	Inherent or no extra costs	|	Roll your own (RYO) 	|	Available at extra cost.	|	Failed containers can be automatically restarted.	|	Available at extra cost, semi-automatic. VMs can be automatically failed over.	|
|	Security	|	Vendor provided	|	Roll your own (RYO)	|	Mix of RYO and vendor provided	|	Mix of RYO and vendor provided	|	Roll your own (RYO)	|
|	Developer velocity	|	Highest	|	Highest	|	Highest	|	Average	|	Slow	|
|	Resource utilization (idle resources that still need to be paid for)	|	Resources are never idle as they are invoked only upon request. When workload is absent, no cost or resource allocation occurs.	|	Because this option is using IaaS or CaaS - similar considerations apply as in columns (4) and (5).	|	Some resources can be idle, and autoscaling helps to eliminate idle resources. A number of running instances must always be present, and are likely to be used at less than 50% of their capacity. Stopped instances do not cost anything.	|	Similar to column (3)	|	Some resources can be idle, but autoscaling is not supported. Some number of running instances must always be present, and are likely to be used at less than 50% of their capacity. Stopped instances can incur the cost of storage.	|
|	Maturity	|	Early maturity	|	Early maturity	|	Early maturity	|	Moderate maturity	|	Highly mature	|
|	Resource limits	|	[Some limits exist](/docs/openwhisk?topic=openwhisk-limits)	|	Depends on allocated resources	|	No	|	No	|	No	|
|	Latency for rarely used services	|	Rare requests can initially see several seconds response time, but remain in MS range for subsequent requests.	|	Depends	|	Low	|	Low	|	Low - assuming the system has enough resources.	|
|	Best type of application	|	Event processing, IoT, Mobile backend, microservices. Definitely not for monolithic applications. See [use cases](/docs/openwhisk?topic=openwhisk-use_cases).	|	Same as column (1), but when user wants to run on non-IBM Cloud or run on premises.	|	Web applications with 24x7 workload load, stateful services that need to keep the connection open for long periods of time. Can be used to run microservices or monolithic applications.	|	Ideal for microservices applications.	|	Traditional enterprise applications that are migrated from on premises to the cloud. Ideal for monolithic applications.	|
|	Charging granularity and billing	|	[Per blocks of 100 milliseconds](https://cloud.ibm.com/functions/learn/pricing){: external}	|	Depends on implementation - If IaaS or CaaS are used, then similar considerations apply - See columns (4) and (5).	|	Usually charged per hour (rarely per minute) for bundle of resources (CPU + memory + some disk space).	|	Similar to column (3).	|	Similar to column (3).	|
|	Total cost of ownership	|	Low. For optimal usage, applications are likely to cost an order of magnitude less than alternatives. Because resources are automatically scaled, over provisioning does not occur.	|	Depends. For cloud deployments, it is likely to be more expensive than OpenWhisk FaaS, but on premises deployment can be cheaper than traditional architectures.	|	Relatively low. The user does not need to provision or manage resources, and can focus on application development. Some level of over provisioning compared to serverless	|	Moderate. The user needs to provision and manage containers and application, and could see some level of over provisioning compared to serverless or PaaS.	|	Relatively high. Consider that the migration of legacy applications into the cloud native model could be prohibitively expensive, this option can be a viable and economical choice for those apps.	|
{: caption="Table 1. Architecture comparison" caption-side="bottom"}

## Cost considerations for {{site.data.keyword.openwhisk_short}}
{: #faas_cost}

The infrastructure for your testing, staging, load testing, and other environments can be costly. It takes time to set them up, and because they usually operate 24x7, they are often underutilized and consume large amounts of capacity. By using a serverless architecture, costs for any number of environments are generated based on load instead of the number of environments defined.
{: shortdesc}

To estimate costs for a serverless application, you can use the [pricing calculator](https://cloud.ibm.com/functions/learn/pricing){: external}.

### Limitless capacity
{: #faas_capacity}

In traditional architectures, each service consumes the amount of capacity that is allocated to them and you are billed for the capacity consumption. {{site.data.keyword.openwhisk_short}} serverless architecture reduces the constraint on the granularity of your microservices architecture.

When not in use, {{site.data.keyword.openwhisk_short}} costs nothing. Your code runs only when a specific action occurs, such as an HTTP call, database state change, or other type of event that triggers the execution of your code. You get billed by millisecond of execution time rounded up to the nearest 100 MS, not per hour of VM utilization regardless of whether that VM was doing useful work. Because you pay only when events are consumed and not based on the number of environments, you can break down your app into 100, 1000, or even more microservices.

### Run actions in any region
{: #faas_region}

In traditional architectures, code must be running in each region to be executed there and the infrastructure for that region must also be paid for. With {{site.data.keyword.openwhisk_short}}, actions can be deployed and made available to run in any region at no extra cost. You can increase the availability and resiliency of your code without the traditional cost restrictions.

### Redundancy by design
{: #faas_redundancy}

In traditional architectures, apps must be redundant. With {{site.data.keyword.openwhisk_short}}, processes don't need to be highly available (HA) because serverless apps are stateless and request-event that is driven by design. By eliminating the need for explicitly creating redundancy, the stateless nature of serverless apps can significantly reduce infrastructure costs.



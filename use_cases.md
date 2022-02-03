---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-03"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, functions, api, data processing, event processing

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Common use cases
{: #use_cases}

The execution model that is offered by {{site.data.keyword.openwhisk}} supports various use cases. The following sections include typical examples. For a more detailed discussion of Serverless architecture, example use cases, pros and cons discussion, and implementation best practices, read the excellent [Mike Roberts article on Martin Fowler's blog](https://martinfowler.com/articles/serverless.html){: external}.
{: shortdesc}

## Microservices
{: #use_cases_microservices}

Despite their benefit, microservice-based solutions remain difficult to build by using mainstream cloud technologies, often requiring control of a complex toolchain, and separate build and operations pipelines. Small and agile teams waste too much time with infrastructural and operational complexities such as fault-tolerance, load balancing, auto-scaling, and logging. These teams want a way to develop streamlined, value-added code with programming languages they already know, love, and that are best suited to solve particular problems.

The modular and inherently scalable nature of {{site.data.keyword.openwhisk_short}} makes it ideal for implementing granular pieces of logic in actions. {{site.data.keyword.openwhisk_short}} actions are independent of each other and can be implemented by using various different languages that are supported by {{site.data.keyword.openwhisk_short}} and access various backend systems. Each action can be independently deployed and managed, is scaled independently of other actions. Interconnectivity between actions is provided by {{site.data.keyword.openwhisk_short}} in the form of rules, sequences, and naming conventions. This type of environment bodes well for microservices based applications.

Another important argument in favor of {{site.data.keyword.openwhisk_short}} is the cost of a system in a disaster recovery configuration. Compare microservices with PaaS or CaaS verses using {{site.data.keyword.openwhisk_short}} by assuming that you have 10 microservices, which use containers or Cloud Foundry runtimes. This comparison equates to 10 continuously running and billable processes in a single availability zone, 20 when run across 2 availability zones, and 40 when run across two regions with two zones each. To achieve the same goal with {{site.data.keyword.openwhisk_short}}, you can run them across as many availability zones or regions as you like, without having to pay a penny of incremental costs.

## Web apps
{: #use_cases_webapps}

Given {{site.data.keyword.openwhisk_short}}’s event-driven nature, it offers several benefits for user-facing applications, whereas the HTTP requests coming from the user’s browser serve as the events. {{site.data.keyword.openwhisk_short}} applications use compute capacity and billed only when they are serving user requests. Idle standby or waiting mode is nonexistent. This feature makes {{site.data.keyword.openwhisk_short}} considerably less expensive when compared to traditional containers or Cloud Foundry applications. Both of those tools have idle time, waiting for inbound user requests, and you are billed for all that “sleeping” time.

Full web application can be built and run with {{site.data.keyword.openwhisk_short}}. Combining serverless APIs with static file hosting for site resources such as HTML, JavaScript, and CSS, means that you can build entirely serverless web applications. The simplicity of operating a hosted {{site.data.keyword.openwhisk_short}} environment is not having to operate anything at all. Since {{site.data.keyword.openwhisk_short}} is hosted on {{site.data.keyword.cloud_notm}}, it is a great benefit when compared to standing up, and operating a Node.js Express or other traditional server runtime.

## IoT
{: #use_cases_iot}

Internet of Things scenarios are often inherently sensor-driven. For example, an action in {{site.data.keyword.openwhisk_short}} might be triggered if a need to react to a sensor that exceeds a specific temperature. IoT interactions are usually stateless with the potential of high load in major spontaneous events such as natural disasters, significant weather storms, or traffic jams. A need is created for an elastic system where normal workload might be small, but needs to scale quickly with predictable response time. So the ability to handle many simultaneous events with no prior warning to the system is desirable. It is difficult to build a system to meet these requirements that use traditional server architectures. As they tend to either be underpowered, and unable to handle peak load in traffic, or be over-provisioned and highly expensive.

It is possible to implement IoT applications that use traditional server architectures. However, in many cases, the combination of different services and data bridges requires high performance and flexible pipelines. Spanning from IoT devices up to cloud storage, and an analytics platform. Often pre-configured bridges lack the programmability to implement and fine-tune a particular solution architecture. Given the variety of pipelines, and the lack of standardization around data fusion in general (IoT in particular), it is common to see environments where the pipeline requires custom data transformation. These custom data transformations apply to format conversion, filtering, or augmentation. {{site.data.keyword.openwhisk_short}} is an excellent tool to implement such a transformation, in a serverless manner, where the custom logic is hosted on a fully managed and elastic cloud platform.

Look at the following sample IoT application that uses {{site.data.keyword.openwhisk_short}}, Node-RED, Cognitive, and other services: [Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}.

## API backend
{: #use_cases_backend}

Serverless computing platforms give developers a rapid way to build APIs without servers. {{site.data.keyword.openwhisk_short}} supports automatic generation of REST API for actions. The [{{site.data.keyword.openwhisk_short}} feature](/docs/openwhisk?topic=openwhisk-apigateway) can invoke an action with HTTP methods other than POST and without the action's authorization API key through the {{site.data.keyword.openwhisk_short}} API Gateway. This capability is helpful not only for exposing APIs to external consumers, but also for building microservices applications.

Additionally, {{site.data.keyword.openwhisk_short}} actions can be connected to an API Management tool of choice (such as [IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} or other). Similar to other use cases, all considerations for scalability, and other Qualities of Services apply.

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} is a sample app that uses {{site.data.keyword.openwhisk_short}} actions through a REST API.

See the following example that includes a discussion of [using Serverless as an API backend](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external}.

## Mobile back end
{: #use_cases_mobile}

Many mobile applications require server-side logic. However, mobile developers usually don’t have experience in managing server-side logic, and would rather focus on the app that is running on the device. This development goal is easily obtained by using {{site.data.keyword.openwhisk_short}} as the server-side back end, and is a good solution. In addition, the built-in support for server-side Swift allows developers to reuse their existing iOS programming skills. Since mobile applications often have unpredictable load patterns, you want to use a {{site.data.keyword.openwhisk_short}} solution like {{site.data.keyword.cloud}}. This solution can scale to meet practically any demand in workload without the need to provision resources ahead of time.

## Data processing
{: #use_cases_data}

With the amount of data now available, application development requires the ability to process new data, and potentially react to it. This requirement includes processing both structured database records as well as unstructured documents, images, or videos. {{site.data.keyword.openwhisk_short}} can be configured by system-provided or custom feeds to react to changes in data, and automatically execute actions on the incoming feeds of data. Actions can be programmed to process changes, transform data formats, send and receive messages, invoke other actions, and update various data stores. Supported data stores include SQL based relational databases, in-memory data grids, NoSQL database, files, messaging brokers, and various other systems. {{site.data.keyword.openwhisk_short}} rules and sequences provide flexibility to change the processing pipeline without programming, and is performed through simple configuration updates. The data store options and low administrative maintenance make a {{site.data.keyword.openwhisk_short}} based system highly agile, and easily adaptable to changing requirements.

## Cognitive
{: #use_cases_cognitive}

Cognitive technologies can be effectively combined with {{site.data.keyword.openwhisk_short}} to create powerful applications. For example, Watson Visual Recognition can be used with {{site.data.keyword.openwhisk_short}} to automatically extract useful information from videos without having to watch them. This technology is the “cognitive” extension of the [Data Processing](#use_cases_data) use case that was discussed earlier. Another good use for {{site.data.keyword.openwhisk_short}} is to implement Bot function that is combined with cognitive services.

A sample application, [Dark vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external}, is provided and does just that. In this application, the user uploads a video or image by using the Dark Vision web application, which stores it in an {{site.data.keyword.cloudant_short_notm}} DB. Once the video is uploaded, {{site.data.keyword.openwhisk_short}} detect the new video by listening to {{site.data.keyword.cloudant_short_notm}} changes (trigger). {{site.data.keyword.openwhisk_short}} then triggers the video extractor action. During its execution, the extractor produces frames (images) and stores them in {{site.data.keyword.cloudant_short_notm}}. The frames are then processed with Watson Visual Recognition, and the results are stored in the same {{site.data.keyword.cloudant_short_notm}} DB. The results can be viewed by using the Dark Vision web application or an iOS application. {{site.data.keyword.cos_full_notm}} can be used in addition to {{site.data.keyword.cloudant_short_notm}}, where video and image metadata are stored in {{site.data.keyword.cloudant_short_notm}}, and the media files are stored in {{site.data.keyword.cos_full_notm}}.

## Event processing with Kafka or {{site.data.keyword.messagehub}}
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} is ideally to be used in combination with Kafka, {{site.data.keyword.messagehub_full}} (Kafka based), and other messaging systems. The event driven nature of those systems requires an event driven runtime to process messages. The runtime can apply business logic to those messages, which is exactly what {{site.data.keyword.openwhisk_short}} provides, with its feeds, triggers, and actions. Kafka and {{site.data.keyword.messagehub}} are often used for high and unpredictable workload volumes, and require that consumers of those messages need to be scalable on a moment's notice. This situation is, once again, ideal for {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} has built-in capability to consume messages as well as publish messages that are provided in the [Event Streams](/docs/openwhisk?topic=openwhisk-pkg_event_streams) package.



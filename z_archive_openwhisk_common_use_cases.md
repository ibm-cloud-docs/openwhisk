---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-13"

keywords: use cases, microservices, web apps, iot, serverless, cognitive

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Common use cases
{: #openwhisk_common_use_cases}

The execution model that is offered by {{site.data.keyword.openwhisk_short}} supports various use cases. The following sections include typical examples. For a more detailed discussion of Serverless architecture, example use cases, pros and cons discussion, and implementation best practices, read the excellent [Mike Roberts article on Martin Fowler's blog](https://martinfowler.com/articles/serverless.html).
{: shortdesc}

## Microservices
{: #openwhisk_common_use_cases_microservices}

Despite their benefit, microservice-based solutions remain difficult to build by using mainstream cloud technologies, often requiring control of a complex toolchain, and separate build and operations pipelines. Small and agile teams spend too much time with infrastructural and operational complexities such as fault-tolerance, load balancing, auto-scaling, and logging. These teams want a way to develop streamlined, value-added code with programming languages they already know, love, and that are best suited to solve particular problems.

The modular and inherently scalable nature of {{site.data.keyword.openwhisk_short}} makes it ideal for implementing granular pieces of logic in actions. {{site.data.keyword.openwhisk_short}} actions are independent of each other and can be implemented by using various different languages that are supported by {{site.data.keyword.openwhisk_short}} and access various backend systems. Each action can be independently deployed and managed, is scaled independently of other actions. Interconnectivity between actions is provided by {{site.data.keyword.openwhisk_short}} in the form of rules, sequences, and naming conventions. This type of environment bodes well for microservices based applications.

Another important argument in favor of {{site.data.keyword.openwhisk_short}} is the cost of a system in a disaster recovery configuration. Compare microservices with PaaS or CaaS vs. using {{site.data.keyword.openwhisk_short}} by assuming that you have 10 microservices, which use containers or CloudFoundry runtimes. This comparison equates to 10 continuously running and billable processes in a single availability zone, 20 when run across 2 AZs, and 40 when run across two regions with two zones each. To achieve the same goal with {{site.data.keyword.openwhisk_short}}, you can run them across as many AZs or regions as you like, without having to pay a penny of incremental costs.

[Logistics Wizard](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/) is an enterprise-grade sample application that leverages {{site.data.keyword.openwhisk_short}} and CloudFoundry to build 12-factor style application. It is a smart supply chain management solution that aims to simulate an environment that runs an ERP system. It augments this ERP system with applications to improve the visibility and agility of supply chain managers.

## Web apps
{: #openwhisk_common_use_cases_webapps}

Given {{site.data.keyword.openwhisk_short}}’s event-driven nature, it offers several benefits for user-facing applications, whereas the HTTP requests coming from the user’s browser serve as the events. {{site.data.keyword.openwhisk_short}} applications use compute capacity and billed only when they are serving user requests. Idle standby or waiting mode is nonexistent. This feature makes {{site.data.keyword.openwhisk_short}} considerably less expensive when compared to traditional containers or CloudFoundry applications. Both of which can spend most of their time idle, waiting for inbound user requests, and being billed for all that “sleeping” time.

Full web application can be built and run with {{site.data.keyword.openwhisk_short}}. Combining serverless APIs with static file hosting for site resources such as HTML, JavaScript, and CSS, means that you can build entirely serverless web applications. The simplicity of operating a hosted {{site.data.keyword.openwhisk_short}} environment is not having to operate anything at all. Since {{site.data.keyword.openwhisk_short}} is hosted on {{site.data.keyword.Bluemix_notm}}, it is a great benefit when compared to standing up, and operating a Node.js Express or other traditional server runtime.

See the following examples on how to use {{site.data.keyword.openwhisk_short}} to build a web app:
- [Web actions: Serverless Web Apps with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [Build a user-facing {{site.data.keyword.openwhisk_short}} application with {{site.data.keyword.Bluemix_notm}} and Node.js](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [Serverless HTTP handlers with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT
{: #openwhisk_common_use_cases_iot}

Internet of Things scenarios are often inherently sensor-driven. For example, an action in {{site.data.keyword.openwhisk_short}} might be triggered if a need to react to a sensor that is exceeds a particular temperature. IoT interactions are usually stateless with the potential of high load in major spontaneous events such as natural disasters, significant weather storms, or traffic jams. A need is created for an elastic system where normal workload might be small, but needs to scale quickly with predictable response time. So the ability to handle many simultaneous events with no prior warning to the system is desirable. It is difficult to build a system to meet these requirements that use traditional server architectures. As they tend to either be underpowered, and unable to handle peak load in traffic, or be overprovisioned and highly expensive.

It is possible to implement IoT applications that use traditional server architectures. However, in many cases, the combination of different services and data bridges requires high performance and flexible pipelines. Spanning from IoT devices up to cloud storage, and an analytics platform. Often pre-configured bridges lack the programmability that is desired to implement and fine-tune a particular solution architecture. Given the variety of pipelines, and the lack of standardization around data fusion in general (IoT in particular), it is common to see environments where the pipeline requires custom data transformation. These custom data transformations apply to format conversion, filtering, or augmentation. {{site.data.keyword.openwhisk_short}} is an excellent tool to implement such a transformation, in a ‘serverless’ manner, where the custom logic is hosted on a fully managed and elastic cloud platform.

Look at the following sample IoT application that uses {{site.data.keyword.openwhisk_short}}, NodeRed, Cognitive, and other services: [Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c).

![IoT solution architecture example](images/IoT_solution_architecture_example.png)

## API backend
{: #openwhisk_api_backend}

Serverless computing platforms give developers a rapid way to build APIs without servers. {{site.data.keyword.openwhisk_short}} supports automatic generation of REST API for actions. The [{{site.data.keyword.openwhisk_short}} feature](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) can invoke an action with HTTP methods other than POST and without the action's authorization API key through the {{site.data.keyword.openwhisk_short}} API Gateway. This capability is helpful not only for exposing APIs to external consumers, but also for building microservices applications.

Additionally, {{site.data.keyword.openwhisk_short}} actions can be connected to an API Management tool of choice (such as [IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) or other). Similar to other use cases, all considerations for scalability, and other Qualities of Services (QoS) apply.

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting) is a sample app that uses {{site.data.keyword.openwhisk_short}} actions through a REST API.

See the following example that includes a discussion of [using Serverless as an API backend](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples).

## Mobile back end
{: #openwhisk_common_use_cases_mobile}

Many mobile applications require server-side logic. However, mobile developers usually don’t have experience in managing server-side logic, and would rather focus on the app that is running on the device. This development goal is easily obtained by using {{site.data.keyword.openwhisk_short}} as the server-side back end, and is a good solution. In addition, the built-in support for server-side Swift allows developers to reuse their existing iOS programming skills. Since mobile applications often have unpredictable load patterns, you want to use a {{site.data.keyword.openwhisk_short}} solution like {{site.data.keyword.Bluemix}}. This solution can scale to meet practically any demand in workload without the need to provision resources ahead of time.

[Skylink](https://github.com/IBM-Bluemix/skylink) is a sample application that connects a drone aircraft by using an iPad, to the IBM Cloud with near realtime image analysis that leverages {{site.data.keyword.openwhisk_short}}, IBM {{site.data.keyword.cloudant_short_notm}}, IBM Watson, and Alchemy Vision.

[BluePic](https://github.com/IBM-Swift/BluePic) is a photo and image sharing application that can be used to take photos, and share them with other BluePic users. This application demonstrates how to leverage, in a mobile iOS 10 application, a Kitura-based server application that is written in Swift, uses {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.cos_full_notm}} for image data. AlchemyAPI is also used in the {{site.data.keyword.openwhisk_short}} sequence to analyze the image, extract text tags based on the content of the image, and finally send a push notification to the user.

## Data processing
{: #data-processing}

With the amount of data now available, application development requires the ability to process new data, and potentially react to it. This requirement includes processing both structured database records as well as unstructured documents, images, or videos. {{site.data.keyword.openwhisk_short}} can be configured by system provided or custom feeds to react to changes in data, and automatically execute actions on the incoming feeds of data. Actions can be programmed to process changes, transform data formats, send and receive messages, invoke other actions, and update various data stores. Supported data stores include SQL based relational databases, in-memory data grids, NoSQL database, files, messaging brokers, and various other systems. {{site.data.keyword.openwhisk_short}} rules and sequences provide flexibility to make changes in the processing pipeline without programming, and is performed through simple configuration updates. The data store options and low admin overhead make a {{site.data.keyword.openwhisk_short}} based system highly agile, and easily adaptable to changing requirements.

[OpenChecks](https://github.com/krook/openchecks) project is a proof of concept that shows how {{site.data.keyword.openwhisk_short}} can be used for processing the deposit of checks to a bank account by using optical character recognition. It is built on the public {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.openwhisk_short}} service and relies on {{site.data.keyword.cloudant}} and {{site.data.keyword.cos_full_notm}}. For on premise, it could use CouchDB, and OpenStack Swift. Other storage services could include FileNet or Cleversafe. Tesseract provides the OCR library.

## Cognitive
{: #openwhisk_common_use_cases_cognitive}

Cognitive technologies can be effectively combined with {{site.data.keyword.openwhisk_short}} to create powerful applications. For example, IBM Alchemy API and Watson Visual Recognition can be used with {{site.data.keyword.openwhisk_short}} to automatically extract useful information from videos without having to watch them. This technology is the “cognitive” extension of the [Data Processing](#data-processing) use case that was discussed earlier. Another good use for {{site.data.keyword.openwhisk_short}} is to implement Bot function that is combined with cognitive services.

A sample application, [Dark vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp), is provided and does just that. In this application, the user uploads a video or image by using the Dark Vision web application, which stores it in an {{site.data.keyword.cloudant_short_notm}} DB. Once the video is uploaded, {{site.data.keyword.openwhisk_short}} detect the new video by listening to {{site.data.keyword.cloudant_short_notm}} changes (trigger). {{site.data.keyword.openwhisk_short}} then triggers the video extractor action. During its execution, the extractor produces frames (images) and stores them in {{site.data.keyword.cloudant_short_notm}}. The frames are then processed with Watson Visual Recognition, and the results are stored in the same {{site.data.keyword.cloudant_short_notm}} DB. The results can be viewed by using the Dark Vision web application or an iOS application. {{site.data.keyword.cos_full_notm}} can be used in addition to {{site.data.keyword.cloudant_short_notm}}, where video and image metadata are stored in {{site.data.keyword.cloudant_short_notm}}, and the media files are stored in {{site.data.keyword.cos_full_notm}}.

An [example iOS Swift application](https://github.com/gconan/BluemixMobileServicesDemoApp) is available that uses {{site.data.keyword.openwhisk_short}}, IBM Mobile Analytics, and Watson to analyze tone and post to a Slack channel.

## Event processing with Kafka or {{site.data.keyword.messagehub}}
{: #openwhisk_event_processing}

{{site.data.keyword.openwhisk_short}} is ideally to be used in combination with Kafka, {{site.data.keyword.messagehub_full}} (Kafka based), and other messaging systems. The event driven nature of those systems requires an event driven runtime to process messages. The runtime can apply business logic to those messages, which is exactly what {{site.data.keyword.openwhisk_short}} provides, with its feeds, triggers, and actions. Kafka and {{site.data.keyword.messagehub}} are often used for high and unpredictable workload volumes, and require that consumers of those messages need to be scalable on a moment's notice. This situation is, once again, a sweet spot for {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} has built-in capability to consume messages as well as publish messages that are provided in the [openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka) package.

An [example application that implements event processing scenario](https://github.com/IBM/openwhisk-data-processing-message-hub) is provided with {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.messagehub}}, and Kafka.


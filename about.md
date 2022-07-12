---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-07"

keywords: platform architecture, openwhisk, couchdb, kafka, functions, terminology, nginx, controller, load balancer, invoker

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# How {{site.data.keyword.openwhisk_short}} works 
{: #about}

{{site.data.keyword.openwhisk}} service is an event-driven compute platform, also referred to as Serverless computing, or as Function as a Service (FaaS), that runs code in response to events or direct invocations. 
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} terminology
{: #functions-terminology}

Learn the basic concepts of the technology behind {{site.data.keyword.openwhisk_short}}. Then, test your knowledge and [take a quiz!](https://ibmcloud-quizzes.mybluemix.net/functions/terms_quiz/quiz.php){: external}

| Term | Description |
| --------- | ------------------- |
| Namespace | [Namespaces](/docs/openwhisk?topic=openwhisk-namespaces) contain {{site.data.keyword.openwhisk_short}} entities, such as actions and triggers, and belong to a resource group. You can let users access your {{site.data.keyword.openwhisk_short}} entities by granting them access to the namespace. |
| Action | An [action](/docs/openwhisk?topic=openwhisk-actions) is a piece of code that performs one specific task. An action can be written in the language of your choice, such as small snippets of JavaScript or Swift code or custom binary code embedded in a Docker container. You provide your action to {{site.data.keyword.openwhisk_short}} either as source code or a Docker image. An action performs work when it is directly invoked by using the {{site.data.keyword.openwhisk_short}} API, CLI, or iOS SDK. An action can also automatically respond to events from {{site.data.keyword.cloud_notm}} services and third-party services by using a trigger. | 
| Sequence | A set of actions can be chained together into a [sequence](/docs/openwhisk?topic=openwhisk-sequences) without having to write any code. A sequence is a chain of actions, invoked in order, where the output of one action is passed as input to the next action. By creating a sequence, you can combine existing actions together for quick and easy reuse. A sequence can then be invoked just like an action, through a REST API or automatically in response to events. | 
| Event | Examples of events include changes to database records, IoT sensor readings that exceed a certain temperature, new code commits to a GitHub repository, or simple HTTP requests from web or mobile apps. Events from external and internal event sources are channeled through a trigger, and rules allow actions to react to these events. |
| Trigger | [Triggers](/docs/openwhisk?topic=openwhisk-triggers) are a named channel for a class of events. A trigger is a declaration that you want to react to a certain type of event, whether from a user or by an event source. | 
| Rule | A [rule](/docs/openwhisk?topic=openwhisk-rules) associates a trigger with an action. Every time the trigger fires, the rule uses the trigger event as input and invokes the associated action. With the appropriate set of rules, it's possible for a single trigger event to invoke multiple actions, or for a single action to be invoked as a response to events from multiple triggers. | 
| Feed | A [feed](/docs/openwhisk?topic=openwhisk-triggers#triggers_feeds) is a convenient way to configure an external event source to fire trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. For example, a Git feed might fire a trigger event for every commit to a Git repository. | 
| Package | Integrations with services and event providers can be added with packages. A [package](/docs/openwhisk?topic=openwhisk-pkg_ov) is a bundle of feeds and actions. An existing catalog of packages offers a quick way to enhance applications with useful capabilities, and to access external services in the ecosystem. Examples of external services that have {{site.data.keyword.openwhisk_short}} packages include {{site.data.keyword.cloudant_short_notm}}, Slack, and GitHub. | 
{: caption="Table 1. {{site.data.keyword.openwhisk_short}} Terms" caption-side="bottom"}

## What happens behind the scenes in {{site.data.keyword.openwhisk_short}}?
{: #about_scenes}

{{site.data.keyword.openwhisk_short}} is based on OpenWhisk, an open source project that combines components such as NGINX, Kafka, Docker, and CouchDB to form a serverless event-based programming service.

![The internal flow of processing behind the scenes in OpenWhisk.](images/OpenWhisk_flow_of_processing.png){: caption="Figure 1. The internal flow of processing behind the scenes in OpenWhisk" caption-side="bottom"}

### 1. Entering the system: NGINX
{: #about_ngnix}

The first entry point into the system is through **NGINX**, an HTTP and reverse proxy server. NGINX is used for SSL termination and forwarding appropriate HTTP calls to the next component.

The OpenWhisk user-facing API is HTTP-based and follows a RESTful design. As a consequence, the command that is sent through the CLI is an HTTP request against the OpenWhisk system. The specific command translates roughly to the following syntax:

```bash
POST /api/v1/namespaces/<userNamespace>/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Note the `<userNamespace>` variable. A user has access to at least one namespace. For simplicity with this example, assume that you own the namespace where `myAction` is located.

### 2. Entering the system: Controller
{: #about_controller}

NGINX forwards the HTTP request to the **Controller**, the next component on the path through OpenWhisk. The controller is a Scala-based implementation of the actual REST API (based on **Akka** and **Spray**). As such, the controller serves as the interface for everything that you want to do, including create, retrieve, update, and delete requests for your entities in OpenWhisk and the invocation of actions.

When the HTTP request is forwarded, the controller first determines what action that you are trying to take, based on the HTTP method that you used in your HTTP request. In this case, you are issuing a POST request to an existing action, which the controller translates to an **invocation of an action**.

Given the central role of the controller (hence the name), the following steps all involve the controller, to a certain extent.

### 3. Authentication and Authorization: CouchDB
{: #about_auth}

Now the controller verifies who you are (*Authentication*) and whether you have the required privileges to do what you want to do with that entity (*Authorization*). The credentials that are included in the request are verified against the so-called **subjects** database in a **CouchDB** instance.

In this case, the controller verifies that you exist in the Functions database and determines whether you have the necessary privilege to invoke the action `myAction`, which is assumed to be an action in a namespace that you own. Thus, you have the authorization to invoke the action.

As everything is sound, the gate opens for the next stage of processing.

### 4. Getting the action: CouchDB… again
{: #about_couchdb}

After the controller determines that you are authenticated and authorized to invoke the action, it loads the action (in this case `myAction`) from the **whisks** database in CouchDB.

The record of the action contains mainly the code to run and any default parameters that you want to pass to your action, merged with the parameters that you included in the actual invoke request. The record also contains the resource restrictions that are imposed on it in execution, such as the amount of memory that the action is allowed to consume.

In this particular case, the action doesn’t take any parameters (the function parameter definition is an empty list). Thus, it is assumed that default parameters are not set, including specific parameters for the action.

### 5. Who’s there to invoke the action: Load Balancer
{: #about_lb}

The Load Balancer, which is part of the controller, has a global view of the executors that are available in the system by checking their health status continuously. Those executors are called **Invokers**. The Load Balancer, knowing which Invokers are available, chooses one of them to invoke the action that you requested.

### 6. Please form a line: Kafka
{: #about_kafka}

The controller and the Invoker solely communicate through messages that are buffered and persisted by Kafka. Kafka lifts the burden of buffering in memory, which risks an *`OutOfMemoryException`*, from both the controller and the Invoker, while also making sure that messages are not lost if a system crashes. 

To get the action invoked, the controller publishes a message to **Kafka**, a high-throughput, distributed, publish-subscribe messaging system. The message contains the action to invoke and the parameters to pass to that action (in this case none). This message is addressed to the Invoker.

After Kafka confirms that the message is received, it responds to the HTTP request with an **Activation ID**. You can use this ID to get access to the results of this specific invocation. This model is an asynchronous invocation model, where the HTTP request terminates once the system accepts the request to invoke an action. A synchronous model (called blocking invocation) is available, but not covered here.

### 7. Invoking the code: Invoker
{: #about_invoker}

The **Invoker** is the heart of {{site.data.keyword.openwhisk_short}} because the Invoker implements the action. To run actions in an isolated and safe way, **Docker** is used to set up a self-encapsulated environment (called a *container*) for each action that is invoked. After the container is created, the code is injected and then run with the parameters that were passed to it. When the results are returned, the container is destroyed. Performance optimizations can be done at this stage to reduce maintenance requirements, and make low response times possible.

In this case, with a *Node.js* based action at hand, the Invoker starts a Node.js container. Then, it injects the code from `myAction`, runs it with no parameters, extracts the result, saves the logs, and destroys the Node.js container.

### 8. Storing the results: CouchDB again
{: #about_storing}

After the result is obtained by the Invoker, it is stored into the **whisks** database as an activation under the assigned activation ID. The **whisks** database lives in **CouchDB**.

In this specific case, the Invoker gets the resulting JSON object back from the action, grabs the log that was written by Docker, includes them all in the activation record, and stores the record in the database. Refer to the following example:

```json
{
    "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
    "response": {
        "statusCode": 0,
        "result": {
            "hello": "world"
        }
    },
    "end": 1474459415621,
    "logs": [
        "2016-09-21T12:03:35.619234386Z stdout: Hello World"
    ],
    "start": 1474459415595,
}
```
{: codeblock}

Note how the record contains both the returned result and the logs that were written. The record also contains the start and end time of the invocation of the action. Activation records contain more fields, but this example is stripped down for simplicity.

Now you can use the REST API (start from step 1 again) to obtain your activation and thus the result of your action. To do so, run this command:

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

In summary, you can see how a simple `ibmcloud fn action invoke myAction` command passes through different stages of the {{site.data.keyword.openwhisk_short}} system. The system itself mainly consists of only two custom components: the **Controller** and the **Invoker**. Everything else is already there, developed by many people in the open source community. 



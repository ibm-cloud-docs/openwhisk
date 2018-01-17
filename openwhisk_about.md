---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# About {{site.data.keyword.openwhisk_short}}

{{site.data.keyword.openwhisk}} is an event-driven compute platform, also referred to as Serverless computing, or as Function as a Service (FaaS), that runs code in response to events or direct invocations. The following figure shows the high-level {{site.data.keyword.openwhisk}} architecture.
{: shortdesc}

![{{site.data.keyword.openwhisk_short}} architecture](./images/OpenWhisk.png)

Examples of events include changes to database records, IoT sensor readings that exceed a certain temperature, new code commits to a GitHub repository, or simple HTTP requests from web or mobile apps. Events from external and internal event sources are channeled through a Trigger, and Rules allow Actions to react to these events.

Actions can be small snippets of JavaScript or Swift code, or custom binary code embedded in a Docker container. Actions in {{site.data.keyword.openwhisk_short}} are instantly deployed and executed whenever a Trigger fires. The more Triggers that fire, the more Actions get invoked. If no Trigger fires, no Action code is running, the cost remains zero.

In addition to associating Actions with Triggers, it is possible to directly invoke an Action by using the {{site.data.keyword.openwhisk_short}} API, CLI, or iOS SDK. A set of Actions can also be chained without having to write any code. Each Action in the chain is invoked in sequence with the output of one Action that is passed as input to the next in the sequence.

With traditional long-running virtual machines or containers, it is common practice to deploy multiple VMs or containers to be resilient against outages of a single instance. However, {{site.data.keyword.openwhisk_short}} offers an alternative model with no resiliency-related cost overhead. The on-demand execution of Actions provides inherent scalability and optimal utilization as the number of running Actions always matches the Trigger rate. Additionally, the developer can now focus on code, and does not worry about monitoring, patching, and securing the underlying server, storage, network, and operating system infrastructure.

Integrations with services and event providers can be added with packages. A package is a bundle of feeds and Actions. A feed is a piece of code that configures an external event source to fire Trigger events. For example, a Trigger that is created with a Cloudant change feed configures a service to fire the Trigger every time a document is modified or added to a Cloudant database. Actions in packages represent reusable logic that a service provider can make available so developers can use the service as an event source, and invoke APIs of that service.

An existing catalog of packages offers a quick way to enhance applications with useful capabilities, and to access external services in the ecosystem. Examples of external services that are {{site.data.keyword.openwhisk_short}} enabled include Cloudant, The Weather Company, Slack, and GitHub.


## How {{site.data.keyword.openwhisk_short}} works
{: #openwhisk_how}

Being an open-source project, OpenWhisk stands on the shoulders of giants, including Nginx, Kafka, Consul, Docker, CouchDB. All of these components come together to form a “serverless event-based programming service”. To explain all the components in more detail, lets trace an invocation of an Action through the system as it happens. An invocation in OpenWhisk is the core thing that a serverless-engine does: Execute the code the user fed into the system, and return the results of that execution.

### Creating the Action

To give the explanation some context, we can create an Action in the system first. Then, use that Action to explain the concepts while tracing through the system. The following commands assume that the [OpenWhisk CLI is set up properly](https://github.com/openwhisk/openwhisk/tree/master/docs#setting-up-the-openwhisk-cli).

First, create a file *action.js* containing the following code, which prints “Hello World” to stdout, and returns a JSON object containing “world” under the key “hello”.
```javascript
function main() {
    console.log('Hello World');
    return { hello: 'world' };
}
```
{: codeblock}

Create the Action by running the following command:
```
wsk action create myAction action.js
```
{: pre}

Now, run the following command to invoke that Action:
```
wsk action invoke myAction --result
```
{: pre}

## The internal flow of processing
What happens behind the scenes in OpenWhisk?

![OpenWhisk flow of processing](images/OpenWhisk_flow_of_processing.png)

### Entering the system: nginx

First, OpenWhisk’s user-facing API is completely HTTP-based and follows a RESTful design. As a consequence, the command that is sent via the wsk-CLI is essentially an HTTP request against the OpenWhisk system. The specific command translates roughly to:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Note the *$userNamespace* variable here. A user has access to at least one namespace. For simplicity, assume that the user owns the namespace where *myAction* is put into.

The first entry point into the system is through **nginx**, “an HTTP and reverse proxy server”. It is used for SSL termination and forwarding appropriate HTTP calls to the next component.

### Entering the system: Controller

Nginx forwards the HTTP request to the **Controller**, the next component on the path through OpenWhisk. It is a Scala-based implementation of the actual REST API (based on **Akka** and **Spray**), and thus serves as the interface for everything a user can do. Including [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) requests for your entities in OpenWhisk, and invocation of Actions.

The Controller first disambiguates what the user is trying to do. It does so based on the HTTP method you use in your HTTP request. As per translation above, the user is issuing a POST request to an existing Action, which the Controller translates to an **invocation of an Action**.

Given the central role of the Controller (hence the name), the following steps will all involve it to a certain extent.

### Authentication and Authorization: CouchDB

Now the Controller verifies who you are (*Authentication*) and if you have the privilege to do what you want to do with that entity (*Authorization*). The credentials included in the request are verified against the so-called **subjects** database in a **CouchDB** instance.

In this case, it is checked that the user exists in OpenWhisk’s database, and that it has the privilege to invoke the Action *myAction*, which is assumed to be an Action in a namespace the user owns. The latter effectively gives the user the privilege to invoke the Action.

As everything is sound, the gate opens for the next stage of processing.

### Getting the Action: CouchDB… again

As the Controller is now sure that the user is allowed in, and has the privileges to invoke the Action, it loads this Action (in this case *myAction*) from the **whisks** database in CouchDB.

The record of the Action contains mainly the code to execute, and default parameters that you want to pass to your Action, merged with the parameters you included in the actual invoke request. It also contains the resource restrictions that are imposed on it in execution, such as the memory it is allowed to consume.

In this particular case, the Action doesn’t take any parameters (the function’s parameter definition is an empty list). Thus, it is assumed that default parameters are not set, including specific parameters for the Action, making for the most trivial case from this point-of-view.

### Who’s there to invoke the Action: Consul

The Controller (or more specifically the load balancing part of it) has everything in place now to run your code, however, it needs to know who’s available to do so. **Consul**, a service discovery, is used to monitor available executors in the system by checking their health status continuously. Those executors are called **Invokers**.

The Controller, now knowing which Invokers are available, chooses one of them to invoke the Action requested.

Let’s assume for this case, that the system has three Invokers available, Invokers 0 - 2, and that the Controller chose *Invoker 2* to invoke the Action at hand.

### Please form a line: Kafka

From now on, mainly two bad things can happen to the invocation request you sent in:

1. The system can crash, losing your invocation.
2. The system can be under such a heavy load, that the invocation needs to wait for other invocations to finish first.

The answer to both is **Kafka**, “a high-throughput, distributed, publish-subscribe messaging system”. Controller and Invoker solely communicate through messages that are buffered and persisted by Kafka. Kafka lifts the burden of buffering in memory, risking an *OutOfMemoryException*, off of both the Controller and the Invoker, while also making sure that messages are not lost in case the system crashes.

To get the Action invoked then, the Controller publishes a message to Kafka, which contains the Action to invoke and the parameters to pass to that Action (in this case none). This message is addressed to the Invoker, which the Controller chose from the list it got from Consul.

Once Kafka confirms that it got the message, the HTTP request to the user is responded to with an **ActivationId**. The user can use that later on, to get access to the results of this specific invocation. This is an asynchronous invocation model, where the HTTP request terminates once the system accepts the request to invoke an Action. A synchronous model (called blocking invocation) is available, but not covered here.

### Invoking the code: Invoker

The **Invoker** is the heart of OpenWhisk. The Invoker’s duty is to invoke an Action. It is also implemented in Scala. But there’s much more to it. To execute Actions in an isolated and safe way it uses **Docker**.

Docker is used to setup a new self-encapsulated environment (called *container*) for each Action that we invoke in a fast, isolated, and controlled way. For each Action invocation, a Docker container is spawned, and the Action code gets injected. The code is then executed by using the parameters that are passed to it, the result is obtained, and the container gets destroyed. Performance optimizations can be done at this stage to reduce overhead, and make low response times possible. 

In this case, having a *Node.js* based Action at hand, the Invoker starts a Node.js container. Then, injects the code from *myAction*, runs it with no parameters, extracts the result, saves the logs, and destroys the Node.js container again.

### Storing the results: CouchDB again

As the result is obtained by the Invoker, it is stored into the **whisks** database as an activation under the ActivationId. The **whisks** database lives in **CouchDB**.

In this specific case, the Invoker gets the resulting JSON object back from the Action, grabs the log written by Docker, puts them all into the activation record, and stores it into the database. Refer to the following example:

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

Note how the record contains both the returned result and the logs written. It also contains the start and end time of the invocation of the Action. Activation records contain more fields, but are stripped down in this example for simplicity.

Now you can use the REST API again (start from step 1 again) to obtain your activation and thus the result of your Action. To do so,run this command:

```bash
wsk activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre} 

### Summary

You can see how a simple **wsk action invoked myAction** passes through different stages of the {{site.data.keyword.openwhisk_short}} system. The system itself mainly consists of only two custom components, the **Controller** and the **Invoker**. Everything else is already there, developed by many people in the open-source community.

You can find additional information about {{site.data.keyword.openwhisk_short}} in the following topics:

* [Entity names](./openwhisk_reference.html#openwhisk_entities)
* [Action semantics](./openwhisk_reference.html#openwhisk_semantics)
* [Limits](./openwhisk_reference.html#openwhisk_syslimits)
* [REST API](./openwhisk_reference.html#openwhisk_ref_restapi)

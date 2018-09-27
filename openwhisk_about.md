---

copyright:
  years: 2016, 2018
lastupdated: "2018-09-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Platform architecture
{: #openwhisk_about}

{{site.data.keyword.openwhisk}} is an event-driven compute platform, also referred to as Serverless computing, or as Function as a Service (FaaS), that runs code in response to events or direct invocations.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} technology
{: #technology}

Learn about some basic concepts of the technology behind {{site.data.keyword.openwhisk_short}}:

<dl>
<dt>Action</dt>
<dd>An [action](openwhisk_actions.html) is a piece of code that performs one specific task. An action can be written in the language of your choice, such as small snippets of JavaScript or Swift code or custom binary code embedded in a Docker container. You provide your action to Cloud Functions either as source code or a Docker image.
<br><br>An action performs work when it is directly invoked by using the {{site.data.keyword.openwhisk_short}} API, CLI, or iOS SDK. An action can also automatically respond to events from {{site.data.keyword.Bluemix_notm}} services and third party services using a trigger.</dd>
<dt>Sequence</dt>
<dd>A set of actions can be chained together into a [sequence](openwhisk_actions.html#openwhisk_create_action_sequence) without having to write any code. A sequence is a chain of actions, invoked in order, where the output of one action is passed as input to the next action. This allows you to combine existing actions together for quick and easy re-use. A sequence can then be invoked just like an action, through a REST API or automatically in response to events.
</dd>
<dt>Event</dt>
<dd>Examples of events include changes to database records, IoT sensor readings that exceed a certain temperature, new code commits to a GitHub repository, or simple HTTP requests from web or mobile apps. Events from external and internal event sources are channeled through a trigger, and rules allow actions to react to these events.</dd>
<dt>Trigger</dt>
<dd>[Triggers](openwhisk_triggers_rules.html#openwhisk_triggers_create) are a named channel for a class of events. A trigger is a declaration that you want to react to a certain type of event, whether from a user or by an event source.</dd>
<dt>Rule</dt>
<dd>A [rule](openwhisk_triggers_rules.html#openwhisk_rules_use) associates a trigger with an action. Every time the trigger fires, the rule uses the trigger event as input and invokes the associated action. With the appropriate set of rules, it's possible for a single trigger event to invoke multiple actions, or for an action to be invoked as a response to events from multiple triggers.</dd>
<dt>Feed</dt>
<dd>A [feed](openwhisk_feeds.html#openwhisk_feeds) is a convenient way to configure an external event source to fire trigger events that can be consumed by {{site.data.keyword.openwhisk_short}}. For example, a Git feed might fire a trigger event for every commit to a Git repository.</dd>
<dt>Package</dt>
<dd>Integrations with services and event providers can be added with packages. A [package](openwhisk_packages.html) is a bundle of feeds and actions. A feed is a piece of code that configures an external event source to fire trigger events. For example, a trigger that is created with an {{site.data.keyword.cloudant}} change feed configures a service to fire the trigger every time a document is modified or added to an {{site.data.keyword.cloudant_short_notm}} database. Actions in packages represent reusable logic that a service provider can make available so developers can use the service as an event source, and invoke APIs of that service.
<br><br>An existing catalog of packages offers a quick way to enhance applications with useful capabilities, and to access external services in the ecosystem. Examples of external services that have {{site.data.keyword.openwhisk_short}} packages include {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack, and GitHub.</dd>
</dl>

## How {{site.data.keyword.openwhisk_short}} works
{: #openwhisk_how}

To explain all the components in more detail, let's trace an invocation of an action through the {{site.data.keyword.openwhisk_short}} system. An invocation executes the code the user feeds into the system, and returns the results of that execution. The following figure shows the high-level {{site.data.keyword.openwhisk_short}} architecture.

![{{site.data.keyword.openwhisk_short}} architecture](./images/OpenWhisk.png)


## How OpenWhisk internal processing works
{: #openwhisk_internal}

What happens behind the scenes in OpenWhisk?

OpenWhisk is an open-source project that combines components including Nginx, Kafka, Docker, and CouchDB to form a serverless event-based programming service.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="The internal flow of processing behind the scenes in OpenWhisk" style="width:550px; border-style: none"/>

### Entering the system: nginx

First, OpenWhisk’s user-facing API is completely HTTP-based and follows a RESTful design. As a consequence, the command that is sent through the CLI is an HTTP request against the OpenWhisk system. The specific command translates roughly to:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Note the *$userNamespace* variable here. A user has access to at least one namespace. For simplicity, assume that the user owns the namespace where *myAction* is put into.

The first entry point into the system is through **nginx**, “an HTTP and reverse proxy server”. It is used for SSL termination and forwarding appropriate HTTP calls to the next component.

### Entering the system: Controller

Nginx forwards the HTTP request to the **Controller**, the next component on the path through OpenWhisk. It is a Scala-based implementation of the actual REST API (based on **Akka** and **Spray**), and thus serves as the interface for everything a user can do. Including [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) requests for your entities in OpenWhisk, and invocation of actions.

The Controller first disambiguates what the user is trying to do. It does so based on the HTTP method you use in your HTTP request. As per translation above, the user is issuing a POST request to an existing action, which the Controller translates to an **invocation of an action**.

Given the central role of the Controller (hence the name), the following steps will all involve it to a certain extent.

### Authentication and Authorization: CouchDB

Now the Controller verifies who you are (*Authentication*) and if you have the privilege to do what you want to do with that entity (*Authorization*). The credentials included in the request are verified against the so-called **subjects** database in a **CouchDB** instance.

In this case, it is checked that the user exists in OpenWhisk’s database, and that it has the privilege to invoke the action *myAction*, which is assumed to be an action in a namespace the user owns. The latter effectively gives the user the privilege to invoke the action.

As everything is sound, the gate opens for the next stage of processing.

### Getting the action: CouchDB… again

As the Controller is now sure that the user is allowed in, and has the privileges to invoke the action, it loads this action (in this case *myAction*) from the **whisks** database in CouchDB.

The record of the action contains mainly the code to execute, and default parameters that you want to pass to your action, merged with the parameters you included in the actual invoke request. It also contains the resource restrictions that are imposed on it in execution, such as the memory it is allowed to consume.

In this particular case, the action doesn’t take any parameters (the function’s parameter definition is an empty list). Thus, it is assumed that default parameters are not set, including specific parameters for the action, making for the most trivial case from this point-of-view.


### Who’s there to invoke the action: Load Balancer

The Load Balancer, which is part of the Controller, has a global view of the executors available in the system by checking their health status continuously. Those executors are called **Invokers**. The Load Balancer, knowing which Invokers are available, chooses one of them to invoke the action requested.

### Please form a line: Kafka

From now on, mainly two bad things can happen to the invocation request you sent in:

1. The system can crash, losing your invocation.
2. The system can be under such a heavy load, that the invocation needs to wait for other invocations to finish first.

The answer to both is **Kafka**, “a high-throughput, distributed, publish-subscribe messaging system”. Controller and Invoker solely communicate through messages that are buffered and persisted by Kafka. Kafka lifts the burden of buffering in memory, risking an *OutOfMemoryException*, off of both the Controller and the Invoker, while also making sure that messages are not lost in case the system crashes.

To get the action invoked then, the Controller publishes a message to Kafka, which contains the action to invoke and the parameters to pass to that action (in this case none). This message is addressed to the Invoker, which the Controller chose from the list it got from Consul.

Once Kafka confirms that it got the message, the HTTP request to the user is responded to with an **ActivationId**. The user can use that later on, to get access to the results of this specific invocation. This is an asynchronous invocation model, where the HTTP request terminates once the system accepts the request to invoke an action. A synchronous model (called blocking invocation) is available, but not covered here.

### Invoking the code: Invoker

The **Invoker** is the heart of OpenWhisk. The Invoker’s duty is to invoke an action. It is also implemented in Scala. But there’s much more to it. To execute actions in an isolated and safe way it uses **Docker**.

Docker is used to setup a new self-encapsulated environment (called *container*) for each action that we invoke in a fast, isolated, and controlled way. For each action invocation, a Docker container is spawned, and the action code gets injected. The code is then executed by using the parameters that are passed to it, the result is obtained, and the container gets destroyed. Performance optimizations can be done at this stage to reduce overhead, and make low response times possible.

In this case, having a *Node.js* based action at hand, the Invoker starts a Node.js container. Then, injects the code from *myAction*, runs it with no parameters, extracts the result, saves the logs, and destroys the Node.js container again.

### Storing the results: CouchDB again

As the result is obtained by the Invoker, it is stored into the **whisks** database as an activation under the ActivationId. The **whisks** database lives in **CouchDB**.

In this specific case, the Invoker gets the resulting JSON object back from the action, grabs the log written by Docker, puts them all into the activation record, and stores it into the database. Refer to the following example:
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

Note how the record contains both the returned result and the logs written. It also contains the start and end time of the invocation of the action. Activation records contain more fields, but are stripped down in this example for simplicity.

Now you can use the REST API again (start from step 1 again) to obtain your activation and thus the result of your action. To do so, run this command:

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Summary

You can see how a simple **ibmcloud fn action invoked myAction** passes through different stages of the {{site.data.keyword.openwhisk_short}} system. The system itself mainly consists of only two custom components, the **Controller** and the **Invoker**. Everything else is already there, developed by many people in the open-source community.

You can find additional information about {{site.data.keyword.openwhisk_short}} in the following topics:

* [Entity names](./openwhisk_reference.html#openwhisk_entities)
* [Action semantics](./openwhisk_reference.html#openwhisk_semantics)
* [Limits](./openwhisk_reference.html#openwhisk_syslimits)
* [REST API reference](https://console.bluemix.net/apidocs/functions)

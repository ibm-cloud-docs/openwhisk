---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-21"

keywords: actions, functions, serverless, javascript, node, node.js

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Action overview
{: #actions_over}

Learn more about actions, web actions, and sequences.
{: shortdesc}

**What is an action?**

An action is a small piece of code that can be invoked or set to automatically run in response to an event. In either case, each run results in a record that is identified by a unique activation ID. The input and the result of an action can be seen as key-value pairs. The key is a string and the value is a valid JSON value. An action can be written in the language of your choice and provided to the service as either source code or a Docker image. The action code runs when it is directly invoked by the Cloud Functions API, CLI, or iOS SDK. An action can automatically respond to events from IBM Cloud or third-party services. For more information about actions, see [Creating actions](/docs/openwhisk?topic=cloud-functions-actions).

**Why would I use an action?**

By using actions, you control the amount of time that your code is running, which lowers your costs.

For example, you can use actions to detect faces in an image, respond to changes in a database, aggregate a set of API calls, or even post a tweet.

**Can I use more than one action at a time?**

Yes! You can use actions to call other actions, or you can string actions together to [create sequences](/docs/openwhisk?topic=cloud-functions-sequences). To make this work, the output of one action is the input for another action, which would provide an output that can be used to trigger another action and so on. You can even bundle the group of actions that you create to form a package. With a package, you can reuse common actions or sequences by calling the package instead of configuring the action or sequence again.

**What is a sequence?**

A set of actions can be chained together into a sequence without having to write any code. A sequence is a chain of actions, invoked in order, where the output of one action is passed as input to the next action. By creating a sequence, you can combine existing actions together for quick and easy reuse. A sequence can then be invoked just like an action, through a REST API or automatically in response to events. For more information about sequences, see [Creating sequences]((/docs/openwhisk?topic=cloud-functions-sequences).

**What is a web action?**

A web action is an action that can be invoked without authentication and can be used to implement HTTP handlers that respond with headers, statusCode, and body content of different types. For more information, see [Creating a web action](/docs/openwhisk?topic=cloud-functions-actions_web).

**Can I use APIs to manage my web actions?**

Yes! You can use the API Gateway as a proxy to your web actions. API Gateway provides HTTP method routing, client ID and secrets, rate limits, CORS, viewing API usage, viewing response logs, and API sharing policies. For more information, see [Creating serverless REST APIs](/docs/openwhisk?topic=cloud-functions-apigateway).

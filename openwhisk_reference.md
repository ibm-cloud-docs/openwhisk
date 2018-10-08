---

copyright:
  years: 2016, 2018
lastupdated: "2018-10-05"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# System details and limits
{: #openwhisk_reference}

The following sections provide technical details about the {{site.data.keyword.openwhisk}} system and limit settings.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} entities
{: #openwhisk_entities}

### Namespaces and packages
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} actions, triggers, and rules belong in a namespace, and sometimes a package.

Packages can contain actions and feeds. A package cannot contain another package, so package nesting is not allowed. Also, entities do not have to be contained in a package.

In {{site.data.keyword.Bluemix_notm}}, an organization+space pair corresponds to a {{site.data.keyword.openwhisk_short}} namespace. For example, the organization `BobsOrg` and space `dev` would correspond to the {{site.data.keyword.openwhisk_short}} namespace `/BobsOrg_dev`.

You can create new Cloud Foundry-based namespaces by [creating Cloud Foundry orgs and spaces](bluemix_cli.html#region_info). The `/whisk.system` namespace is reserved for entities that are distributed with the {{site.data.keyword.openwhisk_short}} system.


### Fully qualified names
{: #openwhisk_entities_fullyqual}

The fully qualified name of an entity is
`/namespaceName/[packageName]/entityName`. Notice that `/` is used to delimit namespaces, packages, and entities. Also, namespaces must be prefixed with a `/`.

For convenience, the Namespace can be left off if it is the user's default namespace. For example, consider a user whose default Namespace is `/myOrg`. Following are examples of the fully qualified names of a number of entities and their aliases.

| Fully qualified name | Alias | Namespace | Package | Name |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

You can use this naming scheme when you use the {{site.data.keyword.openwhisk_short}} CLI, among other places.

### Entity names
{: #openwhisk_entities_names}

The names of all entities, including actions, triggers, rules, packages, and namespaces, are a sequence of characters that follow the following format:

* The first character must be an alphanumeric character, or an underscore.
* The subsequent characters can be alphanumeric, spaces, or any of the following values: `_`, `@`, `.`, `-`.
* The last character can't be a space.

More precisely, a name must match the following regular expression (expressed with Java metacharacter syntax): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Action semantics
{: #openwhisk_semantics}

The following sections describe details about {{site.data.keyword.openwhisk_short}} actions.

### Statelessness
{: #openwhisk_semantics_stateless}

Action implementations are stateless, or *idempotent*. While the system does not enforce this property, it is not guaranteed that any state maintained by an action is available across invocations.

Moreover, multiple instantiations of an action might exist, with each instantiation with its own state. An action invocation might be dispatched to any of these instantiations.

### Invocation input and output
{: #openwhisk_semantics_invocationio}

The input to and output from an action is a dictionary of key-value pairs. The key is a string, and the value a valid JSON value.

### Invocation ordering of actions
{: #openwhisk_ordering}

Invocations of an action are not ordered. If the user invokes an action twice from the command line or the REST API, the second invocation might run before the first. If the actions have side effects, they might be observed in any order.

Additionally, it is not guaranteed that actions execute automatically. Two actions can run concurrently and their side effects can be interleaved. OpenWhisk does not ensure any particular concurrent consistency model for side effects. Any concurrency side effects are implementation-dependent.

### Action execution guarantees
{: #openwhisk_atmostonce}

When an invocation request is received, the system records the request and dispatches an activation.

The system returns an activation ID (with a nonblocking invocation) that confirms that it is received.
If a network failure or other failure that intervenes before you receive an HTTP response, it is possible that {{site.data.keyword.openwhisk_short}} received and processed the request.

The system attempts to invoke the action once, resulting in one of the following four outcomes:
- *success*: The action invocation completed successfully.
- *application error*: The action invocation was successful, but the action returned an error value on purpose, for instance because a precondition on the arguments was not met.
- *action developer error*: The action was invoked, but it completed abnormally, for instance the action did not detect an exception, or a syntax error existed.
- *whisk internal error*: The system was unable to invoke the action.
The outcome is recorded in the `status` field of the activation record, as document in a following section.

For every invocation that is successfully received, and that the user might be billed for, has an activation record.

When the outcome is *action developer error*, the action might partially run, and generate external visible side effects. It is the user's responsibility to check whether such side effects happened, and issue retry logic if desired. Certain *whisk internal errors* indicate that an action starts to run, but fails before the action registers completion.

## Activation record
{: #openwhisk_ref_activation}

Each action invocation and trigger firing results in an activation record.

An activation record contains the following fields:

- *activationId*: The activation ID.
- *start* and *end*: Timestamps recording the start and end of the activation. The values are in [UNIX time format](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* and `name`: The namespace and name of the entity.
- *logs*: An array of strings with the logs that are produced by the action during its activation. Each array element corresponds to a line output to `stdout` or `stderr` by the action, and includes the time and stream of the log output. The structure is as follows: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: A dictionary that defines the keys `success`, `status`, and `result`:
  - *status*: The activation result, which might be one of the following values: "success", "application error", "action developer error", "whisk internal error".
  - *success*: Is `true` if and only if the status is `"success"`
- *result*: A dictionary that contains the activation result. If the activation was successful, the result contains the value that is returned by the action. If the activation was unsuccessful, `result` contains the `error` key, generally with an explanation of the failure.

## JavaScript actions
{: #openwhisk_ref_javascript}

### Function prototype
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript actions run in a Node.js runtime.

actions that are written in JavaScript must be confined to a single file. The file can contain multiple functions, but by convention, a function that is called `main` must exist, and is the one called when the action is invoked. For example, the following example shows an action with multiple functions.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

The action input parameters are passed as a JSON object as a parameter to the `main` function. The result of a successful activation is also a JSON object but is returned differently depending on whether the action is synchronous or asynchronous as described in the following section.

### Synchronous and asynchronous behavior
{: #openwhisk_ref_javascript_synchasynch}

It is common for JavaScript functions to continue execution in a callback function even after a return. To accommodate this behavior, an activation of a JavaScript action can be *synchronous* or *asynchronous*.

A JavaScript action's activation is **synchronous** if the main function exits under one of the following conditions:

- The main function exits without executing a `return` statement.
- The main function exits by executing a `return` statement that returns any value *except* a Promise.

See the following example of a synchronous action:

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

A JavaScript action's activation is **asynchronous** if the main function exits by returning a Promise. In this case, the system assumes that the action is still running until the Promise is fulfilled or rejected.
Start by instantiating a new Promise object and passing it a callback function. The callback takes two arguments, resolve and reject, which are both functions. All your asynchronous code goes inside that callback.

In the following example, you can see how to fulfill a Promise by calling the resolve function.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
     })
}
```
{: codeblock}

This example shows how to reject a Promise by calling the reject function.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
     })
}
```
{: codeblock}

It is possible for an action to be synchronous on some inputs and asynchronous on others as shown in the following example.
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
        return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
          }, 100);
        })
     }  else {
        // synchronous activation
        return {done: true};
     }
}
```
{: codeblock}

Regardless of whether an activation is synchronous or asynchronous, the invocation of the action can be blocking or non-blocking.

### JavaScript global whisk object removed

The global object `whisk` has been removed; migrate your nodejs actions to use alternative methods.
For the functions `whisk.invoke()` and `whisk.trigger()`, use the already installed client library [openwhisk](https://www.npmjs.com/package/openwhisk).
For the `whisk.getAuthKey()`, you can get the API key value from the environment variable `__OW_API_KEY`.
For the `whisk.error()`, you can return a rejected Promise (that is, Promise.reject).

### JavaScript runtime environments
{: #openwhisk_ref_javascript_environments}

JavaScript actions can be executed in Node.js version 6 or Node.js version 8.
Currently actions are executed by default in a Node.js version 6 environment.
### Packaging npm packages with your actions
For any `npm` packages that are not pre-installed in the Node.js environment, you can bundle them as dependencies when you create or update your action.

For more information, see [Packaging an action as a Node.js module](./openwhisk_actions.html#openwhisk_js_packaged_action) or [Packaging an action as a single bundle](./openwhisk_actions.html#openwhisk_js_webpack_action).

### Node.js version 8 environment with IBM SDKs
{: #openwhisk_ref_javascript_environments_8}
The Node.js version 8.12.0 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:8` when creating or updating an action.

The following packages are pre-installed in the Node.js version 8 environment:
  - [amqplib v0.5.2](https://www.npmjs.com/package/amqplib) - A library for making AMQP 0-9-1 clients for Node.JS.
  - [apn v2.2.0](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
  - [async v2.6.1](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
  - [bent v1.1.0](https://www.npmjs.com/package/bent) - Functional HTTP client for Node.js w/ async/await.
  - [bodyparser v1.18.3](https://www.npmjs.com/package/body-parser) - Parse incoming request bodies in a middleware before your handlers, available under the req.body property.
  - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
  - [cassandra-driver v3.5.0](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
  - [@cloudant/cloudant v2.4.0](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
  - [commander v2.18.0](https://www.npmjs.com/package/commander) - The complete solution for node.js command-line interfaces.
  - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Address translator from Compose UI or API for Scylla databases.
  - [consul v0.34.0](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object keyed by the cookie names.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - A high-level, caching, CouchDB client for Node.js.
  - [elasticsearch v15.1.1](https://www.npmjs.com/package/elasticsearch) - The official low-level Elasticsearch client for Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
  - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API.
  - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - A Node.js module for parsing form data, especially file uploads.
  - [glob v7.1.3](https://www.npmjs.com/package/glob) - Match files using the patterns the shell uses, like stars and stuff.
  - [gm v1.23.1](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
  - [ibm-cos-sdk v1.3.0](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
  - [ibm_db v2.4.1](https://www.npmjs.com/package/ibm_db) - An asynchronous/synchronous interface for node.js to IBM DB2 and IBM Informix.
  - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - The node.js client is used for simplifying the interaction with the IBM Watson Internet of Things Platform.
  - [iconv-lite v0.4.24](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
  - [jsdom v12.0.0](https://www.npmjs.com/package/jsdom) - jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG DOM and HTML Standards.
  - [jsforce v1.9.1](https://www.npmjs.com/package/jsforce)Salesforce API Library for JavaScript applications.
  - [jsonwebtoken v8.3.0](https://www.npmjs.com/package/jsonwebtoken) - An implementation of JSON Web Tokens.
  - [lodash v4.17.11](https://www.npmjs.com/package/lodash) - The Lodash library exported as Node.js modules.
  - [log4js v3.0.5](https://www.npmjs.com/package/log4js) - This is a conversion of the log4js framework to work with Node.
  - [marked v0.5.1](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, written in JavaScript. Built for speed.
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - Merge multiple objects into one, optionally creating a new cloned object.
  - [moment v2.22.2](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
  - [mongodb v3.1.6](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
  - [mysql v2.16.0](https://www.npmjs.com/package/mysql) - This is a node.js driver for mysql.
  - [mustache v3.0.0](https://www.npmjs.com/package/mustache) - mustache.js is an implementation of the mustache template system in JavaScript.
  - [nano v7.0.1](https://www.npmjs.com/package/nano) - minimalistic couchdb driver for Node.js.
  - [nodemailer v4.6.8](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
  - [oauth2-server v3.0.1](https://www.npmjs.com/package/oauth2-server) - Complete, compliant and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
  - [openwhisk v3.18.0](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
  - [path-to-regex v2.4.0](https://www.npmjs.com/package/path-to-regexp) - Turn a path string such as /user/:name into a regular expression which can then be used to match against URL paths.
  - [pg v7.4.3](https://www.npmjs.com/package/pg) - Non-blocking PostgreSQL client for node.js. Pure JavaScript and optional native libpq bindings.
  - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'); just like any other module.
  - [pug v2.0.3](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - This is a complete and feature rich Redis client for Node.js.
  - [request v2.88.0](https://www.npmjs.com/package/request) - Request is designed to be the simplest way possible to make HTTP calls.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
  - [semver v5.5.1](https://www.npmjs.com/package/semver) - Semantic Versioning for Nodejs
  - [@sendgrid/mail@6.3.1](https://www.npmjs.com/package/@sendgrid/mail) - Provides email support via the SendGrid API.
  - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
  - [socket.io v2.1.1](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
  - [socket.io-client v2.1.1](https://www.npmjs.com/package/socket.io-client) - Realtime application framework for socket.io.
  - [superagent v3.8.3](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
  - [swagger-tools v0.10.4](https://www.npmjs.com/package/swagger-tools) - Package that provides various tools for integrating and interacting with Swagger.
  - [twilio v3.21.0](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
  - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that provides support for the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
  - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - Parse URLs for path parameters more easily than from using a regex string matcher.
  - [uuid v3.3.2](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
  - [validator v10.8.0](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
  - [vcap_services v0.5.1](https://www.npmjs.com/package/vcap_services)Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides.
  - [watson-developer-cloud v3.11.1](https://www.npmjs.com/package/watson-developer-cloud) - Node.js client library to use the Watson Developer Cloud services, a collection of APIs that use cognitive computing to solve complex problems.
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
  - [winston v3.1.0](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
  - [ws v6.0.0](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
  - [yauzl v2.10.0](https://www.npmjs.com/package/yauzl) - yet another unzip library for node. For zipping.

### Node.js version 6 environment
{: #openwhisk_ref_javascript_environments_6}
The Node.js 6.14.4 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:6` when creating or updating an action.

The following packages are available to be used in the Node.js 6.14.4 environment:

- [apn v2.1.2](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
- [async v2.1.4](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
- [btoa v1.1.2](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio) - Fast, flexible, and lean implementation of core jQuery designed specifically for the server.
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant) - The official Cloudant library for Node.js.
- [commander v2.9.0](https://www.npmjs.com/package/commander) - The complete solution for Node.js command-line interfaces.
- [consul v0.27.0](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object that is keyed by the cookie names.
- [cradle v0.7.1](https://www.npmjs.com/package/cradle) - A high-level, caching, CouchDB client for Node.js.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
- [glob v7.1.1](https://www.npmjs.com/package/glob) - Match files by using patterns that the shell uses, like stars and stuff.
- [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
- [lodash v4.17.2](https://www.npmjs.com/package/lodash) - The Lodash library that is exported as Node.js modules.
- [log4js v0.6.38](https://www.npmjs.com/package/log4js) - A conversion of the log4js framework designed to work with Node.
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
- [marked v0.3.6](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, which is written in JavaScript. Built for speed.
- [merge v1.2.0](https://www.npmjs.com/package/merge) - Merge multiple objects into one, to create a new cloned object.
- [moment v2.17.0](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js is an implementation of the mustache template system in JavaScript.
- [nano v6.2.0](https://www.npmjs.com/package/nano) - Minimalistic couchdb driver for Node.js.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid) - Deprecated UUID packaged.
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server) - Complete, compliant, and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
- [openwhisk v3.18.0](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud is a standard library for Node.js that abstracts away differences among multiple cloud providers.
- [process v0.11.9](https://www.npmjs.com/package/process) - require('process'); just like any other module.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
- [redis v2.6.3](https://www.npmjs.com/package/redis) - A complete and feature-rich Redis client for Node.js.
- [request v2.79.0](https://www.npmjs.com/package/request) - Request is the simplest way possible to make HTTP calls.
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
- [semver v5.3.0](https://www.npmjs.com/package/semver) - Supports semantic versioning.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - Provides email support with the SendGrid API.
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client) - Client-side support for Socket.IO.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools) - Tools that are related to working with Swagger, a way to document APIs.
- [tmp v0.0.31](https://www.npmjs.com/package/tmp) - A simple temporary file and directory creator for node.js.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
- [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that supports the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
- [uuid v3.0.0](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
- [validator v6.1.0](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud) - Node.js client library to use the Watson Developer Cloud services, a collection of APIs that use cognitive computing to solve complex problems.
- [when v3.7.7](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
- [winston v2.3.0](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
- [ws v1.1.1](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - Another unzip library for node.


## Python runtime environments
{: #openwhisk_ref_python_environments}

OpenWhisk supports running Python actions by using two different runtime versions.

### Python 3 actions (Jessie based)
{: #openwhisk_ref_python_environments_jessie}

Python 3 actions are executed with Python 3.6.6. To use this runtime, specify the `wsk` CLI parameter `--kind python-jessie:3` when you create or update an action.
When creating python actions using virtualenv, use the docker image `ibmfunctions/action-python-v3`.
The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries.

Python version:
- [3.6.6](https://github.com/docker-library/python/blob/master/3.6/alpine3.6/Dockerfile)

Python packages:
- asn1crypto             0.24.0
- attrs                  18.1.0
- autobahn               18.8.1
- Automat                0.7.0
- beautifulsoup4         4.6.3
- botocore               1.10.81
- cassandra-driver       3.14.0
- certifi                2018.8.13
- cffi                   1.11.5
- chardet                3.0.4
- click                  6.7
- cloudant               2.9.0
- constantly             15.1.0
- cryptography           2.3.1
- cssselect              1.0.3
- docutils               0.14
- elasticsearch          5.5.3
- Flask                  1.0.2
- gevent                 1.2.2
- greenlet               0.4.14
- httplib2               0.11.3
- hyperlink              18.0.0
- ibm-cos-sdk            2.1.3
- ibm-db                 2.0.9
- ibmcloudsql            0.2.13
- idna                   2.7
- incremental            17.5.0
- itsdangerous           0.24
- Jinja2                 2.10
- jmespath               0.9.3
- kafka-python           1.4.3
- lxml                   4.2.4
- MarkupSafe             1.0
- numpy                  1.15.0
- pandas                 0.23.4
- parsel                 1.5.0
- pika                   0.12.0
- Pillow                 5.2.0
- pip                    18.0
- psycopg2               2.7.5
- pyasn1                 0.4.4
- pyasn1-modules         0.2.2
- pycparser              2.18
- PyDispatcher           2.0.5
- PyHamcrest             1.9.0
- pymongo                3.7.1
- pyOpenSSL              18.0.0
- python-dateutil        2.7.3
- pytz                   2018.5
- queuelib               1.5.0
- redis                  2.10.6
- requests               2.19.1
- scikit-learn           0.19.2
- scipy                  1.1.0
- Scrapy                 1.5.1
- service-identity       17.0.0
- setuptools             40.2.0
- simplejson             3.16.0
- six                    1.11.0
- tornado                4.5.2
- Twisted                18.7.0
- txaio                  18.7.1
- urllib3                1.23
- virtualenv             16.0.0
- w3lib                  1.19.0
- watson-developer-cloud 1.7.0
- Werkzeug               0.14.1
- wheel                  0.31.1
- zope.interface         4.5.0

### Python 3 actions (Alpine based)
{: #openwhisk_ref_python_environments_alpine}

Python 3 actions are executed with Python 3.6.6. To use this runtime, specify the `wsk` CLI parameter `--kind python:3` when you create or update an action.
When creating python actions using virtualenv, use the docker image `openwhisk/python3action`.
The following packages are available for use by Python actions, in addition to the Python 3.6 standard libraries.

Python packages:
- asn1crypto       0.24.0
- attrs            18.2.0
- Automat          0.7.0
- beautifulsoup4   4.5.3
- cffi             1.11.5
- click            6.7
- constantly       15.1.0
- cryptography     2.3.1
- cssselect        1.0.3
- Flask            0.12
- gevent           1.2.1
- greenlet         0.4.15
- httplib2         0.10.3
- idna             2.7
- incremental      17.5.0
- itsdangerous     0.24
- Jinja2           2.10
- kafka-python     1.3.4
- lxml             3.7.3
- MarkupSafe       1.0
- parsel           1.5.0
- pip              18.0
- pyasn1           0.4.4
- pyasn1-modules   0.2.2
- pycparser        2.19
- PyDispatcher     2.0.5
- pyOpenSSL        18.0.0
- python-dateutil  2.6.0
- queuelib         1.5.0
- requests         2.13.0
- Scrapy           1.3.3
- service-identity 17.0.0
- setuptools       40.4.1
- simplejson       3.10.0
- six              1.11.0
- Twisted          17.1.0
- virtualenv       15.1.0
- w3lib            1.19.0
- Werkzeug         0.14.1
- wheel            0.31.1
- zope.interface   4.5.0

### Python 2 actions

Python 2 actions are executed with Python 2.7.15, which is the default runtime for Python actions. Unless you specify the `--kind` flag when you create or update an action. To explicitly select this runtime, use `--kind python:2`.
When creating python actions using virtualenv, use the docker image `openwhisk/python2action`.
The following packages are available for use by Python 2 actions, in addition to the Python 2.7 standard library.

Python packages:
- asn1crypto       0.24.0
- attrs            18.2.0
- beautifulsoup4   4.5.1
- cffi             1.11.5
- click            6.7
- cryptography     2.3.1
- cssselect        1.0.3
- enum34           1.1.6
- Flask            0.11.1
- functools32      3.2.3.post2
- gevent           1.1.2
- greenlet         0.4.15
- httplib2         0.9.2
- idna             2.7
- ipaddress        1.0.22
- itsdangerous     0.24
- Jinja2           2.10
- kafka-python     1.3.1
- lxml             3.6.4
- MarkupSafe       1.0
- parsel           1.5.0
- pip              18.0
- pyasn1           0.4.4
- pyasn1-modules   0.2.2
- pycparser        2.19
- PyDispatcher     2.0.5
- pyOpenSSL        18.0.0
- python-dateutil  2.5.3
- queuelib         1.5.0
- requests         2.11.1
- Scrapy           1.1.2
- service-identity 17.0.0
- setuptools       40.4.1
- simplejson       3.8.2
- six              1.11.0
- Twisted          16.4.0
- virtualenv       15.1.0
- w3lib            1.19.0
- Werkzeug         0.14.1
- wheel            0.31.1
- zope.interface   4.5.0

## Swift actions
{: #swift-actions}

### Swift 3
Swift 3 actions are executed with Swift 3.1.1 `--kind swift:3.1.1`. Always specify kind `swift:3.1.1` as previous versions of Swift are unsupported.

You must migrate all Swift actions to use kind `swift:3.1.1`. As a best practice, always provide the specific kind when you create or update actions.
{: tip}

Swift 3.1.1 actions can use the following packages when using a single Swift source file:
- KituraNet version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 actions are executed using Swift 4.1 `--kind swift:4.1`.

Follow the instructions for [packaged swift actions](./openwhisk_actions.html#packaging-an-action-as-a-swift-executable) to include dependencies using a Package.swift.

Swift 4.1 actions can use the following packages when using single Swift source file:
- Watson Developer Cloud SDK version 0.34.0, https://github.com/watson-developer-cloud/swift-sdk

### Migrating Swift 3.1.1 to Swift 4.1

#### SwiftyJSON using a single source action file
If you have a `swift:3.1.1` action that is not compiled, just as a source file using the **SwiftyJSON** package, you need to pre-compile your action, and specify the version of SwiftyJSON you want to use for `swift:4.1` kind action. Take into account that starting with Swift 4.1, there is better support to manage JSON data natively.

## PHP actions
{: #openwhisk_ref_php}

PHP actions are executed with PHP 7.2.6 To use this runtime, specify the `wsk` CLI parameter `--kind php:7.2` when you create or update an action. This behavior is the default when you create an action with a file that has a `.php` extension.

The PHP 7.1 runtime is currently deprecated, is recommended that all actions get migrated to PHP 7.2, PHP 7.2 provides better cache for warm invocations.

The following PHP extensions are available in addition to the standard ones:

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### Composer packages
The following Composer packages are also available:

- guzzlehttp/guzzle       v6.3.3
- ramsey/uuid             v3.7.3

## Docker actions
{: #openwhisk_ref_docker}

Docker actions run a user-supplied binary in a Docker container. The binary runs in a Docker image based on [python:2.7.12-alpine](https://hub.docker.com/r/library/python), so the binary must be compatible with this distribution.

The Docker skeleton is a convenient way to build OpenWhisk compatible Docker images. You can install the skeleton with the `ibmcloud fn sdk install docker` CLI plug-in command.

The main binary program must be located in `/action/exec` inside the container. The executable receives the input arguments from a single command-line argument string, which can be deserialized as a `JSON` object. It must return a result by using `stdout` as a single-line string of serialized `JSON`.

You can include any compilation steps or dependencies by modifying the `Dockerfile` included in the `dockerSkeleton`.

## REST API
{: #openwhisk_ref_restapi}

Information about the {{site.data.keyword.openwhisk_short}} REST API can be found in the [REST API reference](https://console.bluemix.net/apidocs/functions).

## System limits
{: #openwhisk_syslimits}

### Actions
{{site.data.keyword.openwhisk_short}} has a few system limits, including how much memory an action can use and how many action invocations are allowed per minute.

The following table lists the default limits for actions.

| Limit | Description | Default | Min | Max |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | The maximum size of the action code in MB. | 48 | 1 | 48 |
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) | No more than N activations can be submitted per namespace either executing or queued for execution. | 1000 | 1 | 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | A container is not allowed to write more than N MB to stdout. | 10 | 0 | 10 |
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory) | A container is not allowed to allocate more than N MB of memory. | 256 | 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | No more than N activations can be submitted per namespace per minute. | 5000 | 1 | 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | The maximum number of open files for an action. | 1024 | 0 | 1024 |
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) | The maximum size of the parameters that can be attached in MB. | 1 | 0 | 1 |
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | The maximum number of processes available to an action. | 1024 | 0 | 1024 |
| [result](openwhisk_reference.html#openwhisk_syslimits_result) | The maximum size of the action invocation result in MB. | 1 | 0 | 1 |
| [sequenceMaxActions](openwhisk_reference.html#openwhisk_syslimits_sequencemax) | The maximum number of actions that comprise a given sequence. | 50 | 0 | 50* |
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout) | A container is not allowed to run longer than N milliseconds. | 60000 | 100 | 600000 |

### Increasing fixed limits
{: #increase_fixed_limit}

Limit values ending with a (*) are fixed, but can be increased if a business case can justify higher safety limit values. If you would like to increase the limit value, contact IBM support by opening a ticket directly from the IBM [{{site.data.keyword.openwhisk_short}} web console](https://console.bluemix.net/openwhisk/).
  1. Select **Support**
  2. Select **Add Ticket** from the drop down menu.
  3. Select **Technical** for the ticket type.
  4. Select **Functions** for Technical area of support.

#### codeSize (MB) (Fixed: 48 MB)
{: #openwhisk_syslimits_codesize}
* The maximum code size for the action is 48 MB.
* It is recommended for a JavaScript action to use a tool to concatenate all source code, which includes dependencies, into a single bundled file.
* This limit is fixed and cannot be changed.

#### concurrent (Fixed: 1000*)
{: #openwhisk_syslimits_concurrent}
* The number of activations that are either executing or queued for execution for a namespace cannot exceed 1000.
* This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](openwhisk_reference.html#increase_fixed_limit) for detailed instructions on how to increase this limit.

#### logs (MB) (Default: 10 MB)
{: #openwhisk_syslimits_logs}
* The log limit N is in the range [0 MB..10 MB] and is set per action.
* A user can change the action log limit when an action is created or updated.
* Logs that exceed the set limit are truncated, so any new log entries are ignored, and a warning is added as the last output of the activation to indicate that the activation exceeded the set log limit.

#### memory (MB) (Default: 256 MB)
{: #openwhisk_syslimits_memory}
* The memory limit M is in the range from [128 MB..512 MB] and is set per action in MB.
* A user can change the memory limit when an action is created.
* A container cannot use more memory than is allocated by the limit.

#### minuteRate (Fixed: 5000*)
{: #openwhisk_syslimits_minuterate}
* The rate limit N is set to 5000 and limits the number of action invocations in 1-minute windows.
* A CLI or API call that exceeds this limit receives an error code corresponding to HTTP status code `429: TOO MANY REQUESTS`.
* This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](openwhisk_reference.html#increase_fixed_limit) for detailed instructions on how to increase this limit.

#### openulimit (Fixed: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* The maximum number of open files for an action is 1024 (for both hard and soft limits).
* This limit is fixed and cannot be changed.
* When an action is invoked, the docker run command uses the argument `--ulimit nofile=1024:1024` to set the `openulimit` value.
* For more information, see the [docker run](https://docs.docker.com/engine/reference/commandline/run) command line reference documentation.

#### parameters (Fixed: 1 MB)
{: #openwhisk_syslimits_parameters}
* The size limit for the total parameters on creating or updating of an Action/Package/Trigger is 1 MB.
* An entity with too large parameters is rejected on trying to create or update it.
* This limit is fixed and cannot be changed.

#### proculimit (Fixed: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* The maximum number of processes available to the action container is 1024.
* This limit is fixed and cannot be changed.
* When an action is invoked, the docker run command uses the argument `--pids-limit 1024` to set the `proculimit` value.
* For more information, see the [docker run](https://docs.docker.com/engine/reference/commandline/run) command line reference documentation.

#### result (Fixed: 1 MB)
{: #openwhisk_syslimits_result}
* The maximum output size of an action invocation result in MB.
* This limit is fixed and cannot be changed.

#### sequenceMaxActions (Fixed: 50*)
{: #openwhisk_syslimits_sequencemax}
* The maximum number of actions that comprise a given sequence.
* This limit is fixed and cannot be changed.

#### timeout (ms) (Default: 60s)
{: #openwhisk_syslimits_timeout}
* The timeout limit N is in the range [100 ms..600000 ms], and is set per action in milliseconds.
* A user can change the timeout limit when an action is created.
* A container that runs longer than N milliseconds is terminated.

### Triggers

Triggers are subject to a firing rate per minute as documented in the following table.

| Limit | Description | Default | Min | Max |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | No more than N triggers can be fired per namespace per minute. | 5000* | 5000* | 5000* |

### Increasing fixed limits
{: #increase_fixed_tlimit}

Limit values ending with a (*) are fixed, but can be increased if a business case can justify higher safety limit values. If you would like to increase the limit value, contact IBM support by opening a ticket directly from the IBM [{{site.data.keyword.openwhisk_short}} web console](https://console.bluemix.net/openwhisk/).
  1. Select **Support**
  2. Select **Add Ticket** from the drop down menu.
  3. Select **Technical** for the ticket type.
  4. Select **Functions** for Technical area of support.

#### minuteRate (Fixed: 5000*)
{: #openwhisk_syslimits_tminuterate}

* The rate limit N is set to 5000 and limits the number of triggers that a user can fire in 1-minute windows.
* A user cannot change the trigger limit when a trigger is created.
* A CLI or API call that exceeds this limit receives an error code corresponding to HTTP status code `429: TOO MANY REQUESTS`.
* This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](openwhisk_reference.html#increase_fixed_tlimit) for detailed instructions on how to increase this limit.

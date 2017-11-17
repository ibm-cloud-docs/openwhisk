---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}} system details
{: #openwhisk_reference}

The following sections provide more details about the {{site.data.keyword.openwhisk}} system.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} entities
{: #openwhisk_entities}

### Namespaces and Packages
{: #openwhisk_entities_namespaces}

{{site.data.keyword.openwhisk_short}} Actions, Triggers, and Rules belong in a Namespace, and sometimes a Package.

Packages can contain Actions and Feeds. A package cannot contain another package, so package nesting is not allowed. Also, entities do not have to be contained in a package.

In {{site.data.keyword.Bluemix_notm}}, an organization+space pair corresponds to a {{site.data.keyword.openwhisk_short}} namespace. For example, the organization `BobsOrg` and space `dev` would correspond to the {{site.data.keyword.openwhisk_short}} namespace `/BobsOrg_dev`.

You can create your own namespaces if you're entitled to do so. The `/whisk.system` namespace is reserved for entities that are distributed with the {{site.data.keyword.openwhisk_short}} system.


### Fully qualified names
{: #openwhisk_entities_fullyqual}

The fully qualified name of an entity is
`/namespaceName[/packageName]/entityName`. Notice that `/` is used to delimit Namespaces, Packages, and entities. Also, Namespaces must be prefixed with a `/`.

For convenience, the Namespace can be left off if it is the user's *default namespace*.

For example, consider a user whose default Namespace is `/myOrg`. Following are examples of the fully qualified names of a number of entities and their aliases.

| Fully qualified name | Alias | Namespace | Package | Name |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

You can use this naming scheme when you use the {{site.data.keyword.openwhisk_short}} CLI, among other places.

### Entity names
{: #openwhisk_entities_names}

The names of all entities, including Actions, Triggers, Rules, Packages, and Namespaces, are a sequence of characters that follow the following format:

* The first character must be an alphanumeric character, or an underscore.
* The subsequent characters can be alphanumeric, spaces, or any of the following values: `_`, `@`, `.`, `-`.
* The last character can't be a space.

More precisely, a name must match the following regular expression (expressed with Java metacharacter syntax): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Action semantics
{: #openwhisk_semantics}

The following sections describe details about {{site.data.keyword.openwhisk_short}} Actions.

### Statelessness
{: #openwhisk_semantics_stateless}

Action implementations are stateless, or *idempotent*. While the system does not enforce this property, it is not guaranteed that any state maintained by an Action is available across invocations.

Moreover, multiple instantiations of an Action might exist, with each instantiation with its own state. An Action invocation might be dispatched to any of these instantiations.

### Invocation input and output
{: #openwhisk_semantics_invocationio}

The input to and output from an Action is a dictionary of key-value pairs. The key is a string, and the value a valid JSON value.

### Invocation ordering of Actions
{: #openwhisk_ordering}

Invocations of an Action are not ordered. If the user invokes an Action twice from the command line or the REST API, the second invocation might run before the first. If the Actions have side effects, they might be observed in any order.

Additionally, it is not guaranteed that Actions execute automatically. Two Actions can run concurrently and their side effects can be interleaved. OpenWhisk does not ensure any particular concurrent consistency model for side effects. Any concurrency side effects are implementation-dependent.

### Action execution guarantees
{: #openwhisk_atmostonce}

When an invocation request is received, the system records the request and dispatches an activation.

The system returns an activation ID (with a nonblocking invocation) that confirms that it is received.
If a network failure or other failure that intervenes before you receive an HTTP response, it is possible that {{site.data.keyword.openwhisk_short}} received and processed the request.

The system attempts to invoke the Action once, resulting in one of the following four outcomes:
- *success*: The Action invocation completed successfully.
- *application error*: The Action invocation was successful, but the Action returned an error value on purpose, for instance because a precondition on the arguments was not met.
- *action developer error*: The Action was invoked, but it completed abnormally, for instance the Action did not detect an exception, or a syntax error existed.
- *whisk internal error*: The system was unable to invoke the Action.
The outcome is recorded in the `status` field of the activation record, as document in a following section.

For every invocation that is successfully received, and that the user might be billed for, has an activation record.

When the outcome is *action developer error*, the Action might partially run, and generate external visible side effects. It is the user's responsibility to check whether such side effects happened, and issue retry logic if desired. Certain *whisk internal errors* indicate that an Action starts to run, but fails before the Action registers completion.

## Activation record
{: #openwhisk_ref_activation}

Each Action invocation and Trigger firing results in an activation record.

An activation record contains the following fields:

- *activationId*: The activation ID.
- *start* and *end*: Timestamps recording the start and end of the activation. The values are in [UNIX time format](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* and `name`: The namespace and name of the entity.
- *logs*: An array of strings with the logs that are produced by the Action during its activation. Each array element corresponds to a line output to `stdout` or `stderr` by the Action, and includes the time and stream of the log output. The structure is as follows: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: A dictionary that defines the keys `success`, `status`, and `result`:
  - *status*: The activation result, which might be one of the following values: "success", "application error", "action developer error", "whisk internal error".
  - *success*: Is `true` if and only if the status is `"success"`
- *result*: A dictionary that contains the activation result. If the activation was successful, the result contains the value that is returned by the Action. If the activation was unsuccessful, `result` contains the `error` key, generally with an explanation of the failure.


## JavaScript Actions
{: #openwhisk_ref_javascript}

### Function prototype
{: #openwhisk_ref_javascript_fnproto}

{{site.data.keyword.openwhisk_short}} JavaScript Actions run in a Node.js runtime.

Actions that are written in JavaScript must be confined to a single file. The file can contain multiple functions, but by convention, a function that is called `main` must exist, and is the one called when the Action is invoked. For example, the following example shows an Action with multiple functions.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

The Action input parameters are passed as a JSON object as a parameter to the `main` function. The result of a successful activation is also a JSON object but is returned differently depending on whether the Action is synchronous or asynchronous as described in the following section.


### Synchronous and asynchronous behavior
{: #openwhisk_ref_javascript_synchasynch}

It is common for JavaScript functions to continue execution in a callback function even after a return. To accommodate this behavior, an activation of a JavaScript Action can be *synchronous* or *asynchronous*.

A JavaScript Action's activation is **synchronous** if the main function exits under one of the following conditions:

- The main function exits without executing a `return` statement.
- The main function exits by executing a `return` statement that returns any value *except* a Promise.

See the following example of a synchronous Action:

```javascript
// an Action in which each path results in a synchronous activation
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

A JavaScript Action's activation is **asynchronous** if the main function exits by returning a Promise. In this case, the system assumes that the Action is still running until the Promise is fulfilled or rejected.
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

It is possible for an Action to be synchronous on some inputs and asynchronous on others as shown in the following example. 
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

Regardless of whether an activation is synchronous or asynchronous, the invocation of the Action can be blocking or non-blocking.

### JavaScript global whisk object removed

The global object `whisk` has been removed; migrate your nodejs Actions to use alternative methods.
For the functions `whisk.invoke()` and `whisk.trigger()`, use the already installed client library [openwhisk](https://www.npmjs.com/package/openwhisk).
For the `whisk.getAuthKey()`, you can get the API key value from the environment variable `__OW_API_KEY`.
For the `whisk.error()`, you can return a rejected Promise (that is, Promise.reject).

### JavaScript runtime environments
{: #openwhisk_ref_javascript_environments}

JavaScript Actions can be executed in Node.js version 6 or Node.js version 8.
Currently Actions are executed by default in a Node.js version 6.11.4 environment.  

### Node.js version 6 environment
{: #openwhisk_ref_javascript_environments_6}
The Node.js 6.11.4 environment is used if the `--kind` flag is explicitly specified with a value of 'nodejs:6' when creating or updating an Action.

The following packages are available to be used in the Node.js 6.11.4 environment:

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
- [openwhisk v3.9.0](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud) - pkgcloud is a standard library for Node.js that abstracts away differences among multiple cloud providers.
- [process v0.11.9](https://www.npmjs.com/package/process) - Require('process'); just like any other module.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
- [redis v2.6.3](https://www.npmjs.com/package/redis) - A complete and feature-rich Redis client for Node.js. 
- [request v2.79.0](https://www.npmjs.com/package/request) - Request is the simplest way possible to make HTTP calls. 
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
- [semver v5.3.0](https://www.npmjs.com/package/semver) - Supports semantic versioning.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid) - Provides email support via the SendGrid API.
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
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - Yet another unzip library for node. For zipping.

### Node.js version 8 environment
{: #openwhisk_ref_javascript_environments_8}
The Node.js version 8.9.1 environment is used if the `--kind` flag is explicitly specified with a value of 'nodejs:8' when creating or updating an Action.

The following packages are pre-installed in the Node.js version 8.9.1 environment:

  - [apn v2.1.5](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
  - [async v2.6.0](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
  - [bent v1.1.0](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
  - [btoa v1.1.2](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
  - [commander v2.11.0](https://www.npmjs.com/package/commander) - The complete solution for Node.js command-line interfaces.
  - [consul v0.30.0](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object keyed by the cookie names.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle) - A high-level, caching, CouchDB client for Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
  - [glob v7.1.2](https://www.npmjs.com/package/glob) - Match files by using patterns that the shell uses, like stars and stuff.
  - [gm v1.23.0](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
  - [ibm-cos-sdk v1.0.2](https://www.npmjs.com/package/ibm-cos-sdk) - IBM Cloud Object Storage SDK for Node.js
  - [ibm_db v2.2.1](https://www.npmjs.com/package/ibm_db) - An asynchronous/synchronous interface for node.js to IBM DB2 and IBM Informix. 
  - [lodash v4.17.4](https://www.npmjs.com/package/lodash) - The Lodash library exported as Node.js modules.
  - [log4js v2.3.11](https://www.npmjs.com/package/log4js) - A conversion of the log4js framework designed to work with Node.
  - [iconv-lite v0.4.19](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
  - [marked v0.3.6](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, which is written in JavaScript. Built for speed.
  - [merge v1.2.0](https://www.npmjs.com/package/merge) - Merge multiple objects into one, optionally creating a new cloned object.
  - [moment v2.19.1](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
  - [mongodb v2.2.33](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache) - Mustache.js is an implementation of the mustache template system in JavaScript.
  - [nano v6.4.2](https://www.npmjs.com/package/nano) - Minimalistic couchdb driver for Node.js.
  - [nodemailer v4.3.1](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server) - Complete, compliant, and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
  - [openwhisk v3.9.0](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
  - [process v0.11.10](https://www.npmjs.com/package/process) - Require('process'); just like any other module.
  - [pug v2.0.0-rc.4](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
  - [redis v2.8.0](https://www.npmjs.com/package/redis) - This is a complete and feature-rich Redis client for Node.js.
  - [request v2.83.0](https://www.npmjs.com/package/request) - Request is the simplest way possible to make HTTP calls.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
  - [semver v5.4.1](https://www.npmjs.com/package/semver) - Supports semantic versioning.
  - [@sendgrid/mail@6.1.4](https://www.npmjs.com/package/@sendgrid/mail) - Provides email support via the SendGrid API.
  - [serve-favicon v2.4.5](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
  - [socket.io v2.0.4](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
  - [socket.io-client v2.0.4](https://www.npmjs.com/package/socket.io-client) - Client-side support for Socket.IO.
  - [superagent v3.8.1](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
  - [swagger-tools v0.10.3](https://www.npmjs.com/package/swagger-tools) - Tools that are related to working with Swagger, a way to document APIs.
  - [tmp v0.0.33](https://www.npmjs.com/package/tmp) - A simple temporary file and directory creator for node.js.
  - [twilio v3.9.2](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
  - [underscore v1.8.3](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that supports the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
  - [uuid v3.1.0](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
  - [validator v9.1.1](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
  - [watson-developer-cloud v2.42.0](https://www.npmjs.com/package/watson-developer-cloud) - Node.js client library to use the Watson Developer Cloud services, a collection of APIs that use cognitive computing to solve complex problems.
  - [when v3.7.8](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
  - [winston v2.4.0](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
  - [ws v3.3.1](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl) - Yet another unzip library for node. For zipping.

### Packaging npm packages with your actions
For any `npm` packages that are not pre-installed in the Node.js environment, you can bundle them as dependencies when you create or update your action.

For more information, see [Packaging an action as a Node.js module](./openwhisk_actions.html#openwhisk_js_packaged_action) or [Packaging an action as a single bundle](./openwhisk_actions.html#openwhisk_js_webpack_action).



## Python runtime environments
{: #openwhisk_ref_python_environments}

OpenWhisk supports running Python Actions by using two different runtime versions.

### Python 3 Actions

Python 3 Actions are executed with Python 3.6.1. To use this runtime, specify the `wsk` CLI parameter `--kind python:3` when you create or update an Action.
The following packages are available for use by Python Actions, in addition to the Python 3.6 standard libraries.

- aiohttp v1.3.3
- appdirs v1.4.3
- asn1crypto v0.21.1
- async-timeout v1.2.0
- attrs v16.3.0
- beautifulsoup4 v4.5.1
- cffi v1.9.1
- chardet v2.3.0
- click v6.7
- cryptography v1.8.1
- cssselect v1.0.1
- Flask v0.12
- gevent v1.2.1
- greenlet v0.4.12
- httplib2 v0.9.2
- idna v2.5
- itsdangerous v0.24
- Jinja2 v2.9.5
- kafka-python v1.3.1
- lxml v3.6.4
- MarkupSafe v1.0
- multidict v2.1.4
- packaging v16.8
- parsel v1.1.0
- pyasn1 v0.2.3
- pyasn1-modules v0.0.8
- pycparser v2.17
- PyDispatcher v2.0.5
- pyOpenSSL v16.2.0
- pyparsing v2.2.0
- python-dateutil v2.5.3
- queuelib v1.4.2
- requests v2.11.1
- Scrapy v1.1.2
- service-identity v16.0.0
- simplejson v3.8.2
- six v1.10.0
- Twisted v16.4.0
- w3lib v1.17.0
- Werkzeug v0.12
- yarl v0.9.8
- zope.interface v4.3.3

### Python 2 Actions

Python 2 Actions are executed with Python 2.7.12, which is the default runtime for Python Actions. Unless you specify the `--kind` flag when you create or update an Action. To explicitly select this runtime, use `--kind python:2`. The following packages are available for use by Python 2 Actions, in addition to the Python 2.7 standard library.

- appdirs v1.4.3
- asn1crypto v0.21.1
- attrs v16.3.0
- beautifulsoup4 v4.5.1
- cffi v1.9.1
- click v6.7
- cryptography v1.8.1
- cssselect v1.0.1
- enum34 v1.1.6
- Flask v0.11.1
- gevent v1.1.2
- greenlet v0.4.12
- httplib2 v0.9.2
- idna v2.5
- ipaddress v1.0.18
- itsdangerous v0.24
- Jinja2 v2.9.5
- kafka-python v1.3.1
- lxml v3.6.4
- MarkupSafe v1.0
- packaging v16.8
- parsel v1.1.0
- pyasn1 v0.2.3
- pyasn1-modules v0.0.8
- pycparser v2.17
- PyDispatcher v2.0.5
- pyOpenSSL v16.2.0
- pyparsing v2.2.0
- python-dateutil v2.5.3
- queuelib v1.4.2
- requests v2.11.1
- Scrapy v1.1.2
- service-identity v16.0.0
- simplejson v3.8.2
- six v1.10.0
- Twisted v16.4.0
- virtualenv v15.1.0
- w3lib v1.17.0
- Werkzeug v0.12
- zope.interface v4.3.3

## Swift Actions
{: #openwhisk_ref_swift3}

### Swift 3
Swift 3 Actions are executed with Swift 3.0.2  `--kind swift:3` or Swift 3.1.1 `--kind swift:3.1.1`, respectively. Always specify kind `swift:3.1.1` as previous versions of Swift are unsupported.

Actions that specify the kind `swift:3` will no longer be invoked after November 30, 2017. You must migrate all Swift Actions to use kind `swift:3.1.1`. As a best practice, always provide the specific kind when you create or update Actions.
{: tip}

Swift 3.0.2 Actions can use the following packages:
- KituraNet version 1.0.1, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 14.2.0, https://github.com/IBM-Swift/SwiftyJSON
- IBM Swift Watson SDK version 0.4.1, https://github.com/IBM-Swift/swift-watson-sdk

Swift 3.1.1 Actions can use the following packages:
- KituraNet version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

## PHP Actions
{: #openwhisk_ref_php}

PHP Actions are executed with PHP 7.1. To use this runtime, specify the `wsk` CLI parameter `--kind php:7.1` when you create or update an Action. This behavior is the default when you create an Action with a file that has a `.php` extension.

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

- guzzlehttp/guzzle       v6.3.0
- ramsey/uuid             v3.6.1

## Docker Actions
{: #openwhisk_ref_docker}

Docker Actions run a user-supplied binary in a Docker container. The binary runs in a Docker image based on [python:2.7.12-alpine](https://hub.docker.com/r/library/python), so the binary must be compatible with this distribution.

The Docker skeleton is a convenient way to build OpenWhisk compatible Docker images. You can install the skeleton with the `wsk sdk install docker` CLI command.

The main binary program must be located in `/action/exec` inside the container. The executable receives the input arguments via a single command-line argument string, which can be deserialized as a `JSON` object. It must return a result via `stdout` as a single-line string of serialized `JSON`.

You can include any compilation steps or dependencies by modifying the `Dockerfile` included in the `dockerSkeleton`.

## REST API
{: #openwhisk_ref_restapi}
Information about the REST API can be found [here](openwhisk_rest_api.html)

## System limits
{: #openwhisk_syslimits}

### Actions
{{site.data.keyword.openwhisk_short}} has a few system limits, including how much memory an Action can use and how many Action invocations are allowed per minute.

The following table lists the default limits for Actions.

<!--
| Limit | Description | Configurable | Unit | Default | Min | Max | 
| ----- | ----------- | ------------ | ---- | -------: | ---: | ---: |
| timeout | A container is not allowed to run longer than N milliseconds. | Per action |  ms | 60000 | 100 | 300000 |
| memory | A container is not allowed to allocate more than N MB of memory. | Per action | MB | 256 | 128 | 512 |
| logs | A container is not allowed to write more than N MB to stdout. | Per action | MB | 10 | 0 | 10 |
| concurrent | No more than N activations can be submitted per namespace either executing or queued for execution. | Per namespace | Number | 1000 | 1 | 1000 |
| minuteRate | No more than N activations can be submitted per namespace per minute. | Per user | Number | 5000 | 1 | 5000 |
| codeSize | The maximum size of the actioncode. | Not configurable, limit per action. | MB | 48 | 1 | 48 |
| parameters | The maximum size of the parameters that can be attached. | Not configurable, limit per action, package, or trigger. | MB | 1 | 0 | 1 |
-->

| Limit | Description | Default | Min | Max | 
| ----- | ----------- | :-------: | :---: | :---: |
| timeout | A container is not allowed to run longer than N milliseconds. | 60000 | 100 | 300000 |
| memory | A container is not allowed to allocate more than N MB of memory. | 256 | 128 | 512 |
| logs | A container is not allowed to write more than N MB to stdout. | 10 | 0 | 10 |
| concurrent | No more than N activations can be submitted per namespace either executing or queued for execution. | 1000 | 1 | 1000* |
| minuteRate | No more than N activations can be submitted per namespace per minute. | 5000 | 1 | 5000* |
| codeSize | The maximum size of the actioncode in MB. | 48 | 1 | 48 |
| parameters | The maximum size of the parameters that can be attached in MB. | 1 | 0 | 1 |

### Increasing fixed limits
{: #increase_fixed_limit}

Limit values ending with a (*) are fixed, but can be increased if a business case can justify higher safety limit values. If you would like to increase the limit value, contact IBM support by opening a ticket directly from the IBM [{{site.data.keyword.openwhisk_short}} web console](https://console.bluemix.net/openwhisk/).
  1. Select **Support**
  2. Select **Add Ticket** from the drop down menu.
  3. Select **Technical** for the ticket type.
  4. Select **Functions** for Technical area of support.

### Per Action timeout (ms) (Default: 60s)
{: #openwhisk_syslimits_timeout}
* The timeout limit N is in the range [100 ms..300000 ms], and is set per Action in milliseconds.
* A user can change the timeout limit when an Action is created.
* A container that runs longer than N milliseconds is terminated.

### Per Action memory (MB) (Default: 256 MB)
{: #openwhisk_syslimits_memory}
* The memory limit M is in the range from [128 MB..512 MB] and is set per Action in MB.
* A user can change the memory limit when an Action is created.
* A container cannot use more memory than is allocated by the limit.

### Per Action logs (MB) (Default: 10 MB)
{: #openwhisk_syslimits_logs}
* The log limit N is in the range [0 MB..10 MB] and is set per Action.
* A user can change the Action log limit when an Action is created or updated.
* Logs that exceed the set limit are truncated, so any new log entries are ignored, and a warning is added as the last output of the activation to indicate that the activation exceeded the set log limit.

### Per Action artifact (MB) (Fixed: 48 MB)
{: #openwhisk_syslimits_artifact}
* The maximum code size for the Action is 48 MB.
* It is recommended for a JavaScript Action to use a tool to concatenate all source code, which includes dependencies, into a single bundled file.
* This limit is fixed and cannot be changed.

### Per activation payload size (MB) (Fixed: 1 MB)
{: #openwhisk_syslimits_activationsize}
* The maximum POST content size plus any curried parameters for an Action invocation or Trigger firing is 1 MB.
* This limit is fixed and cannot be changed.

### Per Namespace concurrent invocation (Fixed: 1000*)
{: #openwhisk_syslimits_concur}
* The number of activations that are either executing or queued for execution for a Namespace cannot exceed 1000.
* This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](openwhisk_reference.html#increase_fixed_limit) for detailed instructions on how to increase this limit.

### Invocations per minute (Fixed: 5000*)
{: #openwhisk_syslimits_invocations}
* The rate limit N is set to 5000 and limits the number of Action invocations in 1-minute windows.
* A CLI or API call that exceeds this limit receives an error code corresponding to HTTP status code `429: TOO MANY REQUESTS`.
* This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](openwhisk_reference.html#increase_fixed_limit) for detailed instructions on how to increase this limit.

### Size of the parameters (Fixed: 1 MB)
{: #openwhisk_syslimits_parameters}
* The size limit for the parameters on creating or updating of an Action/Package/Trigger is 1 MB.
* An entity with too large parameters is rejected on trying to create or update it.
* This limit is fixed and cannot be changed.

### Per Docker Action open files ulimit (Fixed: 64:64)
{: #openwhisk_syslimits_openulimit}
* The maximum number of open files is 64 (for both hard and soft limits).
* This limit is fixed and cannot be changed.
* The docker run command uses the argument `--ulimit nofile=64:64`.
* For more information, see the [docker run](https://docs.docker.com/engine/reference/commandline/run) command line reference documentation.

### Per Docker Action processes ulimit (Fixed: 512:512)
{: #openwhisk_syslimits_proculimit}
* The maximum number of processes available to a user is 512 (for both hard and soft limits).
* This limit is fixed and cannot be changed.
* The docker run command uses the argument `--ulimit nproc=512:512`.
* For more information, see the [docker run](https://docs.docker.com/engine/reference/commandline/run) command line reference documentation.

### Triggers

Triggers are subject to a firing rate per minute as documented in the following table.

| Limit | Description | Configurable | Unit | Default |
| ----- | ----------- | ------------ | -----| ------- |
| minuteRate | No more than N Triggers can be fired per namespace per minute. | Per user | Number | 5000* |

### Triggers per minute (Fixed: 5000*)
* The rate limit N is set to 5000 and limits the number of Triggers that can be fired in 1-minute windows.
* A user cannot change the Trigger limit when a Trigger is created.
* A CLI or API call that exceeds this limit receives an error code corresponding to HTTP status code `429: TOO MANY REQUESTS`.
* This limit value is fixed, but can be increased if a business case can justify higher safety limit values. Check the section [Increasing fixed limits](openwhisk_reference.html#increase_fixed_limit) for detailed instructions on how to increase this limit.

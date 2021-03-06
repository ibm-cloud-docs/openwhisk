---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-21"

keywords: runtimes, support, functions, javascript, node, node.js, java, swift, go, python, ruby, .net, PHP, docker

subcollection: openwhisk

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

# Runtimes 
{: #runtimes}

Your apps can be coded and executed in programming languages such as JavaScript or Python. Many runtimes are available by default with {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Available images and runtimes
{: #runtimes_available}

These runtimes (kinds) are available:
<table>
  <tr>
    <th>Language</th>
    <th>Kind identifier</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td><code>nodejs:12</code> (default)</td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code> (default), <code>python:3.6</code></td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (default)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (default)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.15</code> (default)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (default)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (default)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (default)</td>
  </tr>
  <tr>
    <td>Other languages are supported by using Docker actions.</td>
  </tr>
</table>
{: caption="Table 1. Supported runtimes" caption-side="top"}

Runtimes are updated regularly. These updates include security fixes and minor version updates (bug fixes) to the runtime packages. Minor version updates might introduce breaks in compatibility with earlier versions. Runtime updates might impact your actions.

A runtime might also be `deprecated`, which means that certain runtime components are no longer supported. A `deprecated` runtime is still available for a certain time after deprecation and your actions continue to be executed as normal. However, you cannot create or update an action with a deprecated runtime from the console. The console marks actions that use a deprecated runtime. Note that you can create and update actions with a deprecated runtime from the CLI, however, it is highly recommended not to use deprecated runtimes.

After a runtime is deprecated, it becomes `disabled`. The timeframe between `deprecated` and `disabled` is not fixed and can vary from a few months to a few years. After a runtime is `disabled`, actions that use this runtime fail with the error `400, bad request`. An action that uses a `disabled` runtime can still be read and deleted.

Migrate actions that are running on a `deprecated` or `disabled` runtime to a different runtime version by updating it. To update the runtime, see [changing action runtime](/docs/openwhisk?topic=openwhisk-actions#actions_update).

These runtimes (kinds) are deprecated:
<ul>
  <li><code>go:1.11</code> (deprecated)</li>
  <li><code>nodejs:10</code> (deprecated)</li>
  <li><code>nodejs:8</code> (deprecated)</li>
</ul>

These runtimes (kinds) are disabled:
<ul>
  <li><code>ballerina:0.990</code> (disabled)</li>
  <li><code>nodejs</code> (disabled)</li>
  <li><code>nodejs:6</code> (disabled)</li>
  <li><code>php:7.1</code> (disabled)</li>
  <li><code>php:7.2</code> (disabled)</li>
  <li><code>python</code> (disabled)</li>
  <li><code>python:2</code> (disabled)</li>
  <li><code>swift</code> (disabled)</li>
  <li><code>swift:3</code> (disabled)</li>
  <li><code>swift:3.1.1</code> (disabled)</li>
  <li><code>swift:4.1</code> (disabled)</li>
</ul>

For more information about supported and disabled runtimes, see [Available images and runtimes per region](#runtimes_per_region). 

## JavaScript runtimes
{: #openwhisk_ref_javascript_environments}

By default, all Node.js actions are executed in a version 12 environment.
{: note}

Node.js version 10 is deprecated and will soon be removed. To continue running your actions, you must update any Node.js version 10 actions to a higher runtime version. For more information, see the [Node.js release schedule](https://github.com/nodejs/Release){: external}.
{: deprecated}

Node.js version 8 is deprecated and will soon be removed. To continue running your actions, you must update any Node.js version 8 actions to a higher runtime version. For more information, see the [Node.js release schedule](https://github.com/nodejs/Release){: external}.
{: deprecated}

| Kind | Node.js version | Description | Changelog |
| --- | --- | --- | --- |
| `nodejs:12` | [12.x](https://nodejs.org/docs/latest-v12.x/api/){: external} | By default, all Node.js actions are executed in a version 12 environment. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs12/CHANGELOG.md){: external}. |
| `nodejs:10` | [10.x](https://nodejs.org/docs/latest-v10.x/api/){: external} | Deprecated | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}. |
| `nodejs:8` | [8.x](https://nodejs.org/docs/latest-v8.x/api/){: external} | Deprecated | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}. |

### Migrating from Node.js 10 to Node.js 12
{: #migrate_javascript_environments_1012}

| Package | Details |
| --- | --- | 
| `ibm-watson` | The current `ibm-watson` SDK package in the `nodejs:12` runtime is at version v6.x. <br/>Major version changes can introduce incompatibilities, which might cause your action to fail. If your action uses an earlier version of this package, consider migrating to the current version. For more information about migrating to `ibm-watson v6.x`, see [MIGRATION-V6.md](https://github.com/watson-developer-cloud/node-sdk/blob/HEAD/MIGRATION-V6.md){: external}. <br/> If migrating is not possible, consider packaging the older version of `ibm-watson` package with your action as described in [Packaging JavaScript code as NPM files](/docs/openwhisk?topic=openwhisk-prep#prep_js_npm). |
| `ibmiotf` | The `ibmiotf` package is renamed to `@wiotp/sdk`.  For more information, see [IBM Watson IoT Platform JavaScript SDK](https://www.npmjs.com/package/@wiotp/sdk){: external}. |
| `request` | The `request` package is deprecated and therefore not available in this runtime. You can consider `axios`, `bent`, `got`, or `needle` as an alternative. |

For more information about migrating to `Node.js:12`, see [(Details on GitHub)](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs12/CHANGELOG.md){: external}.


### Migrating from Node.js 8 to Node.js 10
{: #migrate_javascript_environments_810}

| Package | Details |
| --- | --- |
| `cloudant` | The `cloudant` NPM package is not available in `nodejs:10`. The package is deprecated. You need to use the official NPM package [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} v3.0.0 when you import the Node.js module (that is, `require('@cloudant/cloudant')`) also [v3.x only returns Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}. |
| `cradle` | The `cradle`  NPM package is not available in `nodejs:10`. |
| `watson-developer-cloud` | The `watson-developer-cloud` NPM package available in `nodejs:10` is version 4.x. This version includes support for Promises. A list of the changes that are made is documented. Note that this package is deprecated and is no longer supported. This package will not receive updates and will be removed in the future. Move your action to the `ibm-watson` package.
| `ibm-watson` | The `ibm-watson` package available in `nodejs:10` is version 4.x. This package is the successor of the `watson-developer-cloud` package. Upgrade your action code to use this new package, since the former will not receive updates anymore. This package includes support for promises. |
| `ibmiotf` | The package `ibmiotf` has been renamed by the maintainers to `@wiotp/sdk`. Make sure to update your action code to the new package. See [`@wiotp/sdk`](https://www.npmjs.com/package/@wiotp/sdk){: external} for all changes. The package `ibmiotf` will not receive further updates and will be removed from this runtime in the future. |

For more information about migrating to `Node.js:10`, see [(Details on GitHub)](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}.


### Node.js packages
{: #javascript_packages}

| Node.js 12 packages | Description |
|:-----------------|:-----------------|
| [`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant) | This library is the official Cloudant library for Node.js. |
| [`@ibm-functions/iam-token-manager`](https://www.npmjs.com/package/@ibm-functions/iam-token-manager) | This package is an IAM access token manager library for Node.js. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail) | Provides email support via the SendGrid API. |
| [`@wiotp/sdk`](https://www.npmjs.com/package/@wiotp/sdk) | IBM Watson IoT Platform JavaScript SDK. |
| [`amqplib`](https://www.npmjs.com/package/amqplib) | A library for making AMQP 0-9-1 clients for Node.js. |
| [`apn`](https://www.npmjs.com/package/apn) | A Node.js module for interfacing with the Apple Push Notification service. |
| [`async`](https://www.npmjs.com/package/async) | Provides functions for working with asynchronous functions. |
| [`axios`](https://www.npmjs.com/package/axios) | Promise-based HTTP client for the browser and Node.js. |
| [`bent`](https://www.npmjs.com/package/bent) | Functional HTTP client for Node.js with async/await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser) | Parse incoming request bodies in a middleware before your handlers, available under the req.body property. |
| [`btoa`](https://www.npmjs.com/package/btoa) | A port of the browser's `btoa` function. |
| [`bufferutil`](https://www.npmjs.com/package/bufferutil) | `bufferutil` is what makes ws fast. |
| [`canvas`](https://www.npmjs.com/package/canvas) | A Cairo-backed Canvas implementation for Node.js. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver) | DataStax Node.js Driver for Apache Cassandra. |
| [`commander`](https://www.npmjs.com/package/commander) | The complete solution for Node.js command-line interfaces. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator) | Address translator from Compose UI or API for Scylla databases. |
| [`consul`](https://www.npmjs.com/package/consul) | A client for Consul, involving service discovery and configuration. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser) | Parse Cookie header and populate req.cookies with an object keyed by the cookie names.|
| [`core-js`](https://www.npmjs.com/package/core-js) | Modular standard library for JavaScript.|
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) | The official low-level `Elasticsearch` client for Node.js.|
| [`errorhandler`](https://www.npmjs.com/package/errorhandler) | Development-only error handler middleware.|
| [`etcd3`](https://www.npmjs.com/package/etcd3) | A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API.|
| [`express`](https://www.npmjs.com/package/express) | A Fast, unopinionated, minimalist web framework for node. |
| [`express-session`](https://www.npmjs.com/package/express-session) | A server-side session data storing module. |
| [`formidable`](https://www.npmjs.com/package/formidable) | A Node.js module for parsing form data, especially file uploads. |
| [`glob`](https://www.npmjs.com/package/glob) | Match files by using the patterns that the shell uses, like stars and stuff. |
| [`gm`](https://www.npmjs.com/package/gm) | GraphicsMagick and ImageMagick for Node. |
| [`got`](https://www.npmjs.com/package/got) | Human-friendly and powerful HTTP request library for Node.js. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk) | {{site.data.keyword.cos_full}} SDK for Node.js. |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db) | An asynchronous/synchronous interface for Node.js to IBM DB2 and IBM Informix. |
| [`ibm-watson`](https://www.npmjs.com/package/ibm-watson) | A Node.js client library to use the Watson APIs. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite) | Pure JS character encoding conversion. |
| [`jest`](https://www.npmjs.com/package/jest) | Delightful JavaScript Testing. |
| [`jsdom`](https://www.npmjs.com/package/jsdom) | `jsdom` is a pure-JavaScript implementation of many web standards, notably the `WHATWG` DOM and HTML Standards. |
| [`jsforce`](https://www.npmjs.com/package/jsforce) | Salesforce API Library for JavaScript applications. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken) | An implementation of JSON Web Tokens. |
| [`lodash`](https://www.npmjs.com/package/lodash) | The `Lodash` library that is exported as Node.js modules. |
| [`log4js`](https://www.npmjs.com/package/log4js) | This package is a conversion of the `log4js` framework to work with node. |
| [`marked`](https://www.npmjs.com/package/marked) | A full-featured markdown parser and compiler, which is written in JavaScript. Built for speed. |
| [`merge`](https://www.npmjs.com/package/merge) | Merge multiple objects into one, optionally creating a new cloned object. |
| [`moment`](https://www.npmjs.com/package/moment) | A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates. |
| [`mongodb`](https://www.npmjs.com/package/mongodb) | The official MongoDB driver for Node.js. |
| [`mustache`](https://www.npmjs.com/package/mustache) | mustache.js is an implementation of the mustache template system in JavaScript. |
| [`mysql`](https://www.npmjs.com/package/mysql) | This package is a Node.js driver for `mysql`. |
| [`nano`](https://www.npmjs.com/package/nano) | Minimalistic `couchdb` driver for Node.js. |
| [`needle`](https://www.npmjs.com/package/needle) | The leanest and most handsome HTTP client in the `Nodelands`. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer) | Send emails from Node.js – easy as cake! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server) | Complete, compliant, and well-tested module for implementing an OAuth2 Server/Provider with express in Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk) | JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp) | Turn a path string such as `/user/:name` into a regular expression, which can then be used to match against URL paths. |
| [`pg`](https://www.npmjs.com/package/pg) | Non-blocking PostgreSQL client for Node.js. Pure JavaScript and optional native `libpq` bindings. |
| [`process`](https://www.npmjs.com/package/process) | `require('process');`, just like any other module. |
| [`pug`](https://www.npmjs.com/package/pug) | Implements the Pug templating language. |
| [`redis`](https://www.npmjs.com/package/redis) | This package is a complete and feature rich Redis client for Node.js. |
| [`request-promise`](https://www.npmjs.com/package/request-promise) | The simplified HTTP request client 'request' with Promise support. Powered by Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf) | The UNIX command rm -rf for node. |
| [`semver`](https://www.npmjs.com/package/semver) | Semantic Versioning for Node.js. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error) | Serialize an error into a plain object. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon) | Node.js middleware for serving a favicon. |
| [`socket.io`](https://www.npmjs.com/package/socket.io) | `Socket.io` enables real-time bidirectional event-based communication. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client) | Realtime application framework for `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent) | `SuperAgent` is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools) | Package that provides various tools for integrating and interacting with Swagger. |
| [`tmp`](https://www.npmjs.com/package/tmp) | A simple temporary file and directory creator for Node.js. |
| [`ts-jest`](https://www.npmjs.com/package/ts-jest) | A TypeScript preprocessor with source map support for Jest that enables you use Jest to test projects written in TypeScript. |
| [`twilio`](https://www.npmjs.com/package/twilio) | A wrapper for the Twilio API, related to voice, video, and messaging. |
| [`underscore`](https://www.npmjs.com/package/underscore) | Underscore.js is a utility-belt library for JavaScript that supports the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern) | Parse URLs for path parameters more easily than from using a regex string matcher. |
| [`utf-8-validate`](https://www.npmjs.com/package/utf-8-validate) | Check whether a buffer contains valid `UTF-8` encoded text. |
| [`uuid`](https://www.npmjs.com/package/uuid) | Simple, fast generation of `RFC4122 UUIDS`. |
| [`validator`](https://www.npmjs.com/package/validator) | A library of string validators and sanitizers. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services) | Parse and return service credentials from `VCAP_SERVICES` environment variable that IBM Cloud provides. |
| [`when`](https://www.npmjs.com/package/when) | `when.js` is a rock solid, battle-tested, and Promises/A+, and when() implementation, including a complete ES6 Promise shim. |
| [`winston`](https://www.npmjs.com/package/winston) | A multi-transport async logging library for Node.js. "CHILL WINSTON! ... I put it in the logs." |
| [`ws`](https://www.npmjs.com/package/ws) | `ws` is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation. |
| [`xlsx`](https://www.npmjs.com/package/xlsx) | Parser and writer for various spreadsheet formats. |
| [`xml2js`](https://www.npmjs.com/package/xml2js) | Simple XML to JavaScript object converter. It supports bidirectional conversion. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest) | `node-XMLHttpRequest` is a wrapper for the built-in HTTP client to emulate the browser `XMLHttpRequest` object. |
| [`yauzl`](https://www.npmjs.com/package/yauzl) | Yet another extract library for node. For zipping. |
| [`yazl`](https://www.npmjs.com/package/yauzl) | Yet another extract library for node. For zipping. |
{: caption="Table 1. Node.js 12 packages." caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 12 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

| Node.js 10 packages | Description |
|:-----------------|:-----------------|
| [`@ibm-functions/iam-token-manager`](https://www.npmjs.com/package/@ibm-functions/iam-token-manager) | This package is an IAM access token manager library for Node.js. |
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | A library for making Advanced Message Queuing Protocol 0-9-1 clients for Node.js. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | A Node.js module for interfacing with the Apple Push Notification service. |
| [`async`](https://www.npmjs.com/package/async){: external} | Provides functions for working with asynchronous functions. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Functional HTTP client for Node.js with async and await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Parse incoming request bodies in a middleware before your handlers, available under the req.body property. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | A port of the browser's `btoa` function. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | DataStax Node.js Driver for Apache Cassandra. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/@cloudant/cloudant){: external} | The official Cloudant library for Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | The complete solution for Node.js command-line interfaces. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Address translator from Compose UI or API for Scylla databases. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | A client for Consul, involving service discovery and configuration. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Parse Cookie header and populate req.cookies with an object that is keyed by the cookie names. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | The official low-level `Elasticsearch` client for Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Development-only error handler middleware. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | A Node.js module for parsing form data, especially file uploads. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Match files by using the patterns that the shell uses, like stars and stuff. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick and ImageMagick for Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | An asynchronous/synchronous interface for Node.js to IBM DB2 and IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | The Node.js client that is used for simplifying the interaction with the IBM Watson Internet of Things Platform. |
| [`ibm-watson`](https://www.npmjs.com/package/ibm-watson){: external} | Node.js client library to use the Watson APIs. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Pure JS character encoding conversion. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` is a pure-JavaScript implementation of many web standards, notably the Web Hypertext Application Technology Working Group DOM and HTML Standards. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Salesforce API Library for JavaScript applications. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | An implementation of JSON Web Tokens. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | The `lodash` library is exported as Node.js modules. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | A full-featured markdown parser and compiler that is written in JavaScript. Built for speed. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Merge multiple objects into one, optionally creating a new cloned object.
| [`moment`](https://www.npmjs.com/package/moment){: external} | A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | The official MongoDB driver for Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | A Node.js driver for MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | Mustache.js is an implementation of the mustache template system in JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Minimalistic CouchDB driver for Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Send emails from Node.js – easy as cake! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Complete, compliant, and tested module for implementing an OAuth2 Server/Provider with express in Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Turn a path string such as `/user/:name` into a regular expression, which can then be used to match against URL paths. |
| [`pg`](https://www.npmjs.com/package/pg){: external} | Non-blocking PostgreSQL client for Node.js. Pure JavaScript and optional native `libpq` bindings. |
| [`process`](https://www.npmjs.com/package/process){: external} | `require('process')`; just like any other module. |
| [`pug`](https://www.npmjs.com/package/pug){: external} | Implements the Pug templating language. |
| [`redis`](https://www.npmjs.com/package/redis){: external} | A complete and feature rich Redis client for Node.js. |
| [`request`](https://www.npmjs.com/package/request){: external} | Make HTTP calls. |
| [`request-promise`](https://www.npmjs.com/package/request-promise){: external} | The simplified HTTP request client 'request' with Promise support. Powered by Bluebird. |
| [`rimraf`](https://www.npmjs.com/package/rimraf){: external} | The UNIX command rm -rf for node. |
| [`semver`](https://www.npmjs.com/package/semver){: external} | Semantic Versioning for nodeJS. |
| [`@sendgrid/mail`](https://www.npmjs.com/package/@sendgrid/mail){: external} | Provides email support via the SendGrid API. |
| [`serialize-error`](https://www.npmjs.com/package/serialize-error){: external} | Serialize an error into a plain object. |
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Node.js middleware for serving a favicon.
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` enables real-time bidirectional event-based communication. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Realtime application framework for `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Package that provides various tools for integrating and interacting with Swagger. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | A wrapper for the Twilio API, related to voice, video, and messaging. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js is a utility-belt library for JavaScript that supports the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Parse URLs for path parameters more easily than from a regex string matcher. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Simple, fast generation of `RFC4122 UUIDS`. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | A library of string validators and sanitizers. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js is a rock solid, battle-tested `Promises/A+` and `when()` implementation, including a complete ES6 Promise shim. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | A multi-transport async logging library for Node.js. "CHILL WINSTON! ... I put it in the logs." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation. |
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} | Parser and writer for various spreadsheet formats. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Simple XML to JavaScript object converter. It supports bidirectional conversion. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | `node-XMLHttpRequest` is a wrapper for the built-in HTTP client to emulate the browser `XMLHttpRequest` object. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Another extraction library for node. |
{: caption="Table 2. Node.js 10 packages." caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 10 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}



For more information about Node.js 12 packages, see [(Details on GitHub)](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs12/package.json){: external}.

For more information about Node.js 10 packages, see [(Details on GitHub)](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/package.json){: external}.

## Python runtimes
{: #openwhisk_ref_python_environments}

By default, all Python actions are executed in a Python version 3.7 environment.
{: note}

| Kind | Python version | Description | Changelog |
| --- | --- | --- | --- |
| &nbsp; | 2.7 | Python 2.7 reached `end of support` on 2020/01/01.<br/>See [the Active Python Releases](https://www.python.org/downloads/){: external}. | &nbsp; |
| `python:3.6` | [3.6.x](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external} | Python 3 actions are executed with Python 3.6.x. To use this runtime, specify the CLI parameter `--kind python:3.6` when you create or update an action. The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}. |
| `python:3.7` | [3.7.x](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external} | By default, all Python actions are executed in a Python version 3.7.x environment (Debian Stretch based) unless you specify the `--kind` flag when you create or update an action. To explicitly use this runtime, specify the CLI parameter `--kind python:3.7` when you create or update an action. The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.7 standard libraries. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}. |


### Python packages
{: #python_packages}

Ensure that your action uses only the packages that are mentioned in the following table.<br/>
While other Python packages might be part of the runtime, they are included only as indirect dependencies of the other listed packages. These unlisted packages are candidates to be removed as soon as they are not required by the referring package.
{: note}

| Python 3.7 packages |
|:-----------------|
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `cloudant` |
| `elasticsearch` |
| `etcd3` |
| `flask` |
| `gevent` |
| `httplib2` |
| `ibm-cos-sdk` |
| `ibm_db` |
| `ibmcloudsql` |
| `kafka_python` |
| `lxml` |
| `numpy` |
| `pandas` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `PyJWT` |
| `pymongo` |
| `python-dateutil` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `scrapy` |
| `simplejson` |
| `tornado` |
| `twisted` |
| `virtualenv` |
| `watson-developer-cloud` |
{: caption="Table 1. Python 3.7 packages" caption-side="top"}
{: #python-1}
{: tab-title="Python 3.7 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Python 3.6 packages |
|:-----------------|
| `cassandra-driver` |
| `cloudant` |
| `elasticsearch` |
| `beautifulsoup4` |
| `flask` |
| `gevent` |
| `httplib2` |
| `ibm-cos-sdk` |
| `ibm_db` |
| `ibmcloudsql` |
| `kafka_python` |
| `lxml` |
| `numpy` |
| `pandas` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `pymongo` |
| `python-dateutil` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `scrapy` |
| `simplejson` |
| `twisted` |
| `virtualenv` |
| `watson-developer-cloud` |
{: caption="Table 2. Python 3.6 packages" caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

For more information about Python 3.7 packages, see [(Details on GitHub)](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/requirements.txt){: external}.

For more information about Python 3.6 packages, see [(Details on GitHub)](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/requirements.txt){: external}.

## Swift runtime
{: #swift-actions}

By default, all Swift actions are executed in a version 4.2 environment.
{: note}

Swift 4.x action runtimes don't embed any packages, follow the instructions for [packaged Swift actions](/docs/openwhisk?topic=openwhisk-prep#prep_swift42_single) to include dependencies by using a Package.swift.

Swift 4.2 actions can use the following packages when you use a single Swift source file:
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


**<ph class="ignoreSpelling">SwiftyJSON</ph> with a single source action file**

If you have an action that is not compiled, and uses the `SwiftyJSON` package, you need to pre-compile your action, and specify the version of `SwiftyJSON` you want to use for `swift:4.2` kind action.


## PHP runtime
{: #openwhisk_ref_php}

By default, all PHP actions are executed in a version 7.3 environment.
{: note}

The following PHP extensions are available in addition to the standard ones:

- `bcmath`
- `curl`
- `gd`
- `intl`
- `mbstring`
- `mysqli`
- `pdo_mysql`
- `pdo_pgsql`
- `pdo_sqlite`
- `soap`
- `zip`

## Docker runtime
{: #openwhisk_ref_docker}

Docker actions run a user-supplied executable in a Docker container. You can include any compilation steps or dependencies by modifying your `Dockerfile`. You can specify a Docker image to use with your action code by specifying the `--docker` flag when you run the [**`action create`**](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create) command. When you create an action that uses a Docker image, your app runs in the public Docker image that you specify during action creation.

{{site.data.keyword.openwhisk_short}} provides base images on Docker hub. You can use these images as-is when you create actions, or you can use them as a base image when you write your `Dockerfile`. You can see a list of the available runtimes images in the [Available images and runtimes](#runtimes_available) section.

{{site.data.keyword.openwhisk_short}} actions can be created by using public images on from Docker Hub.
{: note}

### Docker skeleton
{: #docker_skeleton}

You can use the `openwhisk/dockerskeleton` image to deploy your action. You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. By using this argument, you can create and deploy an executable that runs inside the standard Docker action SDK.

For more information, see [Creating actions from binaries](/docs/openwhisk?topic=openwhisk-actions#actions_create_binaries).

## More runtime support
{: #more_runtime_support}

**Go**

By default, all Go actions are executed in a version 1.15 environment. Go Version 1.11 is deprecated.

When migrating from Go:1.11 to Go:1.15:
- Go:1.15 uses [Go Modules](https://golang.org/ref/mod){: external}.
- File containing the entry function (e.g `main.go` `Main`) needs be placed in root, no `"main"` directory is supported.

For more information about the `go` runtimes, see [(Details on GitHub)](https://github.com/apache/openwhisk-runtime-go/blob/master/README.md){: external}.

For more information about writing `go` actions, see [Preparing Go apps](/docs/openwhisk?topic=openwhisk-prep#prep_go).

**Java**

By default, all Java actions are executed in a version 8 environment.

For more information about the `java:8` runtime, see [(Details on GitHub)](https://github.com/apache/openwhisk-runtime-java/blob/master/core/java8/CHANGELOG.md){: external}.

**Ruby**

By default, all Ruby actions are executed in a version 2.5 environment.

For more information about the `ruby:2.5` runtime, see [(Details on GitHub)](https://github.com/apache/openwhisk-runtime-ruby/blob/master/core/ruby2.5Action/CHANGELOG.md){: external}.

**.NET Core**

By default, all .NET Core actions are executed in a version 2.2 environment.

For more information about the `dotnet:2.2` runtime, see [(Details on GitHub)](https://github.com/apache/openwhisk-runtime-dotnet/blob/master/core/dotnet2.2/CHANGELOG.md){: external}.


## Available images and runtimes per region
{: #runtimes_per_region}

To view the available runtimes for IBM Cloud Functions in each region. The following links return a JSON response. The `runtimes` section of the response contains the set of available runtimes. The `image` section contains the name of the runtime image on [Docker Hub](https://hub.docker.com/){: external} and the tag that is used.

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}
  - [`jp-tok`](https://jp-tok.functions.cloud.ibm.com/){: external}
  - [`au-syd`](https://au-syd.functions.cloud.ibm.com){: external}

The following examples point to the images `ibmfunctions/action-nodejs-v12` and `openwhisk/java8action` on [hub.docker.com](https://hub.docker.com/){: external}.
The tags can be version numbers such as `1.1.1` or the short form of a Git commit hash, like `a231007`.

Example image fields.

  ```
  image:   "ibmfunctions/action-nodejs-v12:1.1.1"
  ```
  ```
  image:   "openwhisk/java8action:a231007"
  ```

This JSON response shows the supported and the disabled runtimes.
Disabled runtimes are marked with `deprecated=true`.
Deprecated runtimes are not shown in this response.
The deprecated flag that is returned in the response does not correspond to the deprecated runtimes as mentioned in [Available images and runtimes](#runtimes_available).
Deprecated runtimes are not marked as such in this response
until the runtime is finally disabled.
Deprecated runtimes are only marked as such in the console and in this documentation ([Available images and runtimes](#runtimes_available)).

When you troubleshoot a failing action (400, bad request) to identify whether a disabled runtime is used,
check for `deprecated=true` in the query response.
Actions that use a disabled runtime can be only read or deleted.
Execution of such an action is not possible.

To update the runtime of this action to a supported one, see [changing action runtime](/docs/openwhisk?topic=openwhisk-actions#actions_update).

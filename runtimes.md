---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: runtimes, support, functions

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


# Runtimes
{: #runtimes}
Your apps can be coded and executed in programming languages such as Javascript or Python. Many runtimes are available by default with {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

View the available runtimes for IBM Cloud Functions in each region. The following links return a JSON response. The `runtimes` section of the response contains the set of available runtimes. The `image` section contains the name of the runtime image on [Docker Hub](https://hub.docker.com/){: external} and the tag that is used.

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}


The following examples point to the images `ibmfunctions/action-nodejs-v10` and `openwhisk/nodejs8action`.
The tags can be version numbers like `1.9.0` or the short form of a git commit hash, like `b99d71e`.

Example image fields.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Runtimes are updated regularly. These updates include security fixes and minor version updates to the packages inside the runtimes. Minor version updates might introduce backward compatibility breaks. Runtime updates might impact your actions. You must migrate actions that are running a runtime to a newer version by updating it.

Apps that run on deprecated runtimes cannot be completed successfully until the runtime is updated to a supported one. When troubleshooting a failing action to identify whether a runtime is deprecated, check for `deprecated=true` in the query response. To update the runtime, see [changing action runtime](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

These runtimes are deprecated:
<ul>
  <li><code>nodejs:6</code> (deprecated)</li>
  <li><code>php:7.1</code> (deprecated)</li>
  <li><code>php:7.2</code> (deprecated)</li>
  <li><code>swift:3</code> (deprecated)</li>
  <li><code>swift:3.1.1</code> (deprecated)</li>
  <li><code>swift:4.1</code> (deprecated)</li>
  <li><code>ballerina:0.990</code> (deprecated)</li>
</ul>



## JavaScript runtimes
{: #openwhisk_ref_javascript_environments}

By default, all Node.js actions are executed in a version 10 environment.
{: note}

JavaScript actions can be executed in Node.js version 8 or 10. Node.js version 8 is in maintenance mode and is available until December 2019. See the [Node.js release schedule](https://github.com/nodejs/Release){: external}. 
{: deprecated}

| Runtime | Description | Changelog |
| --- | --- | --- |
| [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external} | By default, all Node.js actions are executed in a version 10 environment. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}. |
| [8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external} | The Node.js version 8.15.0 is used if the `--kind` flag is explicitly specified with a value of `nodejs:8` when creating or updating an action. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}. |


### Migrating from nodeJS 8 to nodeJS 10

| Package | Details |
| --- | --- |
| `ibm_db` | The `ibm_db` NPM package is not available in `nodejs:10`. The `ibm_db` package doesn't support Node.js 10. You can track progress in [this issue](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541){: external}. |
| `cloudant` | The `cloudant` NPM package is not available in `nodejs:10`. The package is deprecated. You need to use the official NPM package [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} v3.0.0 when importing the Node.js module (that is, `require('@cloudant/cloudant')`) also [v3.x only returns Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}. |
| `cradle` | The `cradle`  NPM package is not available in `nodejs:10`. |
| `log4js` | The `log4js`  NPM package is not available in `nodejs:10`. You can track [this issue](https://github.com/log4js-node/log4js-node/issues/805){: external}. |
| `watson-developer-cloud` | The `watson-developer-cloud` NPM package is not available in `nodejs:10`. You can track progress on the new version in [this issue](https://github.com/watson-developer-cloud/node-sdk/issues/780){: external}. |


### Node.js packages

| Node.js 10.15 packages | Description |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | A library for making Advanced Message Queuing Protocol 0-9-1 clients for Node.JS. |
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
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | The official low-level ElasticSearch client for Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Development-only error handler middleware. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | A Node.js module for parsing form data, especially file uploads. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Match files by using the patterns that the shell uses, like stars and stuff. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick and ImageMagick for Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | An asynchronous/synchronous interface for Node.js to IBM DB2 and IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | The Node.js client that is used for simplifying the interaction with the IBM Watson Internet of Things Platform. |
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
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Send e-mails from Node.js – easy as cake! |
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
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Parse URLs for path parameters more easily than from using a regex string matcher. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Simple, fast generation of RFC4122 UUIDS. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | A library of string validators and sanitizers. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js is a rock solid, battle-tested `Promises/A+` and `when()` implementation, including a complete ES6 Promise shim. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | A multi-transport async logging library for Node.js. "CHILL WINSTON! ... I put it in the logs." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation. |
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} | Parser and writer for various spreadsheet formats. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Simple XML to JavaScript object converter. It supports bi-directional conversion. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Another extraction library for node. |
{: caption="Table 1. Node.js 10.15 packages." caption-side="top"}
{: #javascript-1}
{: tab-title="Node.js 10.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}

| Node.js 8.15 packages | Description |
|:-----------------|:-----------------|
| [`amqplib`](https://www.npmjs.com/package/amqplib){: external} | A library for making Advanced Message Queuing Protocol 0-9-1 clients for Node.JS. |
| [`apn`](https://www.npmjs.com/package/apn){: external} | A Node.js module for interfacing with the Apple Push Notification service.
| [`async`](https://www.npmjs.com/package/async){: external} | Provides functions for working with asynchronous functions. |
| [`bent`](https://www.npmjs.com/package/bent){: external} | Functional HTTP client for Node.js with async-await. |
| [`bodyparser`](https://www.npmjs.com/package/body-parser){: external} | Parse incoming request bodies in a middleware before your handlers, available under the req.body property. |
| [`btoa`](https://www.npmjs.com/package/btoa){: external} | A port of the browser's `btoa` function. |
| [`cassandra-driver`](https://www.npmjs.com/package/cassandra-driver){: external} | DataStax Node.js Driver for Apache Cassandra. |
| [`cloudant`](https://www.npmjs.com/package/cloudant){: external} | The official Cloudant library for Node.js. |
| [`@cloudant/cloudant`](https://www.npmjs.com/package/cloudant){: external} | The official Cloudant library for Node.js. |
| [`commander`](https://www.npmjs.com/package/commander){: external} | The complete solution for Node.js command-line interfaces. |
| [`composeaddresstranslator`](https://www.npmjs.com/package/composeaddresstranslator){: external} | Address translator from Compose UI or API for Scylla databases. |
| [`consul`](https://www.npmjs.com/package/consul){: external} | A client for Consul, involving service discovery and configuration. |
| [`cookie-parser`](https://www.npmjs.com/package/cookie-parser){: external} | Parse Cookie header and populate req.cookies with an object keyed by the cookie names. |
| [`elasticsearch`](https://www.npmjs.com/package/elasticsearch){: external} | The official low-level ElasticSearch client for Node.js. |
| [`errorhandler`](https://www.npmjs.com/package/errorhandler){: external} | Development-only error handler middleware. |
| [`etcd3`](https://www.npmjs.com/package/etcd3){: external} | A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API. |
| [`formidable`](https://www.npmjs.com/package/formidable){: external} | A Node.js module for parsing form data, especially file uploads. |
| [`glob`](https://www.npmjs.com/package/glob){: external} | Match files by using the patterns that the shell uses, like stars and stuff. |
| [`gm`](https://www.npmjs.com/package/gm){: external} | GraphicsMagick and ImageMagick for Node. |
| [`ibm-cos-sdk`](https://www.npmjs.com/package/ibm-cos-sdk){: external} | {{site.data.keyword.cos_full}} SDK for Node.js. |
| [`ibm_db`](https://www.npmjs.com/package/ibm_db){: external} | An asynchronous-synchronous interface for Node.js to IBM DB2 and IBM Informix. |
| [`ibmiotf`](https://www.npmjs.com/package/ibmiotf){: external} | The Node.js client is used for simplifying the interaction with the IBM Watson Internet of Things Platform. |
| [`iconv-lite`](https://www.npmjs.com/package/iconv-lite){: external} | Pure JS character encoding conversion. |
| [`jsdom`](https://www.npmjs.com/package/jsdom){: external} | `jsdom` is a pure-JavaScript implementation of many web standards, notably the Web Hypertext Application Technology Working Group DOM and HTML Standards. |
| [`jsforce`](https://www.npmjs.com/package/jsforce){: external} | Salesforce API Library for JavaScript applications. |
| [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken){: external} | An implementation of JSON Web Tokens. |
| [`lodash`](https://www.npmjs.com/package/lodash){: external} | The `lodash` library that is exported as Node.js modules. |
| [`log4js`](https://www.npmjs.com/package/log4js){: external} | A conversion of the log4js framework to work with Node. |
| [`marked`](https://www.npmjs.com/package/marked){: external} | A full-featured markdown parser and compiler that is written in JavaScript. Built for speed. |
| [`merge`](https://www.npmjs.com/package/merge){: external} | Merge multiple objects into one, optionally creating a new cloned object. |
| [`moment`](https://www.npmjs.com/package/moment){: external} | A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates. |
| [`mongodb`](https://www.npmjs.com/package/mongodb){: external} | The official MongoDB driver for Node.js. |
| [`mysql`](https://www.npmjs.com/package/mysql){: external} | A Node.js driver for MySQL. |
| [`mustache`](https://www.npmjs.com/package/mustache){: external} | mustache.js is an implementation of the mustache template system in JavaScript. |
| [`nano`](https://www.npmjs.com/package/nano){: external} | Minimalistic CouchDB driver for Node.js. |
| [`nodemailer`](https://www.npmjs.com/package/nodemailer){: external} | Send e-mails from Node.js – easy as cake! |
| [`oauth2-server`](https://www.npmjs.com/package/oauth2-server){: external} | Complete, compliant, and tested module for implementing an OAuth2 Server-Provider with express in Node.js. |
| [`openwhisk`](https://www.npmjs.com/package/openwhisk){: external} | JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs. |
| [`path-to-regex`](https://www.npmjs.com/package/path-to-regexp){: external} | Turn a path string such as `/user/:name` into a regular expression, which can then be used to match against URL paths.
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
| [`serve-favicon`](https://www.npmjs.com/package/serve-favicon){: external} | Node.js middleware for serving a favicon. |
| [`socket.io`](https://www.npmjs.com/package/socket.io){: external} | `socket.io` enables real-time bidirectional event-based communication. |
| [`socket.io-client`](https://www.npmjs.com/package/socket.io-client){: external} | Realtime application framework for `socket.io`. |
| [`superagent`](https://www.npmjs.com/package/superagent){: external} | `superagent` is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features. |
| [`swagger-tools`](https://www.npmjs.com/package/swagger-tools){: external} | Package that provides various tools for integrating and interacting with Swagger. |
| [`twilio`](https://www.npmjs.com/package/twilio){: external} | A wrapper for the Twilio API, related to voice, video, and messaging. |
| [`underscore`](https://www.npmjs.com/package/underscore){: external} | Underscore.js is a utility-belt library for JavaScript that supports the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects. |
| [`url-pattern`](https://www.npmjs.com/package/url-pattern){: external} | Parse URLs for path parameters more easily than from using a regex string matcher. |
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Simple, fast generation of RFC4122 UUIDS. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | A library of string validators and sanitizers. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides. |
| [`watson-developer-cloud`](https://www.npmjs.com/package/watson-developer-cloud){: external} | Node.js client library to use the Watson Developer Cloud services, a collection of APIs that use cognitive computing to solve complex problems. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js is a rock solid, battle-tested `Promises/A+` and `when()` implementation, including a complete ES6 Promise shim. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | A multi-transport async logging library for Node.js. "CHILL WINSTON! ... I put it in the logs." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Simple XML to JavaScript object converter. It supports bi-directional conversion. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | `node-XMLHttpRequest` is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Another extraction library for node. |
{: caption="Table 2. Node.js 8.15 packages." caption-side="top"}
{: #javascript-2}
{: tab-title="Node.js 8.15 packages"}
{: tab-group="node"}
{: class="simple-tab-table"}


## Python runtimes
{: #openwhisk_ref_python_environments}

By default, all Python actions are executed in a version 2.7.15 environment.
{: note}

| Python version | Description | Changelog |
| --- | --- | --- |
| 2.7.15 | By default, all Python actions are executed in a version 2.7.15 environment unless you specify the `--kind` flag when you create or update an action. When creating python actions using `virtualenv`, use the docker image `openwhisk/python2action`. The following packages are available for use by Python 2 actions, in addition to the Python 2.7 standard library. | [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md){: external}. |
| [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external} | Python 3 actions are executed with Python 3.6.x. To use this runtime, specify the CLI parameter `--kind python:3.6` when you create or update an action. The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}. |
| [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external} | Python 3.7 actions (Debian Stretch based) actions are executed with Python 3.7.x. To use this runtime, specify the CLI parameter `--kind python:3.7` when you create or update an action. The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.7 standard libraries. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}. |


### Python packages

| Python 2.7.15 packages | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `enum34` |
| `Flask` |
| `functools32` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `idna` |
| `incremental` |
| `ipaddress` |
| `itsdangerous` |
| `Jinja2` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `parsel` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pyOpenSSL` |
| `python-dateutil` |
| `queuelib` |
| `requests` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `Twisted` |
| `urllib3` |
| `virtualenv=` |
| `w3lib` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Table 1. Python 2.7.15 packages." caption-side="top"}
{: #python-1}
{: tab-title="Python 2.7.15 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Python 3.6.8 packages | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `autobahn` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six=` |
| `soupsieve` |
| `tornado` |
| `Twisted` |
| `txaio` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Table 2. Python 3.6.8 packages." caption-side="top"}
{: #python-2}
{: tab-title="Python 3.6.8 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}

| Python 3.7.2 packages | 
|:-----------------|
| `asn1crypto` |
| `attrs` |
| `Automat` |
| `beautifulsoup4` |
| `botocore` |
| `cassandra-driver` |
| `certifi` |
| `cffi` |
| `chardet` |
| `Click` |
| `cloudant` |
| `constantly` |
| `cryptography` |
| `cssselect` |
| `docutils` |
| `elasticsearch` |
| `etcd3` |
| `Flask` |
| `gevent` |
| `greenlet` |
| `grpcio` |
| `httplib2` |
| `hyperlink` |
| `ibm-cos-sdk` |
| `ibm-cos-sdk-core` |
| `ibm-cos-sdk-s3transfer` |
| `ibm-db` |
| `ibmcloudsql` |
| `idna` |
| `incremental` |
| `itsdangerous` |
| `Jinja2` |
| `jmespath` |
| `kafka-python` |
| `lxml` |
| `MarkupSafe` |
| `numpy` |
| `pandas` |
| `parsel` |
| `pika` |
| `Pillow` |
| `protobuf` |
| `psycopg2` |
| `pyarrow` |
| `pyasn1` |
| `pyasn1-modules` |
| `pycparser` |
| `PyDispatcher` |
| `PyHamcrest` |
| `pymongo` |
| `pyOpenSSL` |
| `python-dateutil` |
| `pytz` |
| `queuelib` |
| `redis` |
| `requests` |
| `scikit-learn` |
| `scipy` |
| `Scrapy` |
| `service-identity` |
| `simplejson` |
| `six` |
| `soupsieve` |
| `tenacity` |
| `tornado` |
| `Twisted` |
| `urllib3` |
| `virtualenv` |
| `w3lib` |
| `watson-developer-cloud` |
| `websocket-client` |
| `Werkzeug` |
| `zope.interface` |
{: caption="Table 3. Python 3.7.2 packages." caption-side="top"}
{: #python-3}
{: tab-title="Python 3.7.2 packages"}
{: tab-group="python"}
{: class="simple-tab-table"}


## Swift runtime
{: #swift-actions}

By default, all Swift actions are executed in a version 4.2 environment.
{: note}

Swift 4.x action runtimes don't embed any packages, follow the instructions for [packaged swift actions](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) to include dependencies by using a Package.swift.

Swift 4.2 actions can use the following packages when you are using a single Swift source file:
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### <ph class="ignoreSpelling">SwiftyJSON</ph> using a single source action file
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

Docker actions run a user-supplied executable in a Docker container. The executable runs in a Docker image based on [python:3.6-alpine](https://hub.docker.com/_/python){: external}, so the executable must be compatible with this distribution.

The Docker skeleton is a convenient way to build OpenWhisk compatible Docker images. You can install the skeleton with the `ibmcloud fn sdk install docker` CLI plug-in command.

The main executable program must be located in `/action/exec` inside the container. The executable receives the input arguments from a single command-line argument string, which can be deserialized as a `JSON` object. It must return a result by using `stdout` as a single-line string of serialized `JSON`.

You can include any compilation steps or dependencies by modifying the `Dockerfile` included in the `dockerSkeleton`.

## More runtime support

| Runtime |
| --- | --- | 
| By default, all Go actions are executed in a version 1.11 environment. |
{: caption="Table 1. Go." caption-side="top"}
{: #runtimes-1}
{: tab-title="Go"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Runtime |
| --- | --- | 
| By default, all Java actions are executed in a version 8 environment. |
{: caption="Table 2. Java." caption-side="top"}
{: #runtimes-2}
{: tab-title="Java"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Runtime |
| --- |
| By default, all Ruby actions are executed in a version 2.5 environment. |
{: caption="Table 3. Ruby." caption-side="top"}
{: #runtimes-3}
{: tab-title="Ruby"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}

| Runtime |
| --- |
| By default, all .NET Core actions are executed in a version 2.2 environment. |
{: caption="Table 4. .NET Core." caption-side="top"}
{: #runtimes-4}
{: tab-title=".NET"}
{: tab-group="runtimes"}
{: class="simple-tab-table"}





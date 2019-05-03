---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-03"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Runtimes
{: #runtimes}
Your apps can be coded and executed in programming languages such as Javascript or Python. Many runtimes are available by default with {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

View the available runtimes for IBM Cloud Functions in each region. The following links return a JSON response.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

The `runtimes` section of the response contains the set of available runtimes.

The `image` section contains the name of the runtime image on [Docker Hub](https://hub.docker.com/) and the tag that is used.


The following examples point to the images `ibmfunctions/action-nodejs-v10` and `openwhisk/nodejs6action`.
The tags can be version numbers like `1.9.0` or the short form of a git commit hash, like `b99d71e`.

Example image fields.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

Runtimes are updated on a regular basis. These updates include security fixes and minor version updates to the packages inside the runtimes. Minor version updates might introduce backward compatibility breaks. Runtime updates might impact your actions. You must migrate actions that are running a runtime to a newer version by updating it.

Apps that run on deprecated runtimes cannot be completed successfully until the runtime is updated to a supported one. When troubleshooting a failing action, to identify whether a runtime is deprecated, check for `deprecated=true` in the query response. To update the runtime, see [changing action runtime](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)



## JavaScript runtimes
{: #openwhisk_ref_javascript_environments}

JavaScript actions can be executed in Node.js version 8 or 10. 

Node.js version 6 is the default version, but is deprecated as of 6 December 2018. To continue using a JavaScript action, update to Node.js version 8 or 10.
{: deprecated}



### Node.js version 10 environment with IBM SDKs
{: #openwhisk_ref_javascript_environments_10}
The Node.js version 10 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:10` when creating or updating an action.

#### Migrating from `nodejs:8` to `nodejs:10`
- The `ibm_db` npm package is not available in `nodejs:10`. The `ibm_db` package doesn't support Node.js 10. You can track progress in this issue [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- The `cloudant` npm package is not available in `nodejs:10`, the package is deprecated, you need to use the official npm package [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 when importing the nodejs module (i.e `require('@cloudant/cloudant')`) also [v3.x only returns Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- The `cradle` npm package is not available in `nodejs:10`.
- The `log4js` npm package is not available in `nodejs:10`. You can track the issue at [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- The `watson-developer-cloud` npm package is not available in `nodejs:10`. You can track progress on the new version in this  issue [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Detailed information about the nodejs version 10 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

#### Node.js 10.15 packages

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 NPM Packages:
   - [amqplib v0.5.3](https://www.npmjs.com/package/amqplib) - A library for making AMQP 0-9-1 clients for Node.JS.
   - [apn v2.2.0](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
   - [async v2.6.1](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
   - [bent v1.2.0](https://www.npmjs.com/package/bent) - Functional HTTP client for Node.js with async and await.
   - [bodyparser v1.18.3](https://www.npmjs.com/package/body-parser) - Parse incoming request bodies in a middleware before your handlers, available under the req.body property.
   - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
   - [cassandra-driver v4.0.0](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
   - [@cloudant/cloudant v3.0.2](https://www.npmjs.com/package/@cloudant/cloudant) - This is the official Cloudant library for Node.js.
   - [commander v2.19.0](https://www.npmjs.com/package/commander) - The complete solution for node.js command-line interfaces.
   - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Address translator from Compose UI or API for Scylla databases.
   - [consul v0.34.1](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
   - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object keyed by the cookie names.
   - [elasticsearch v15.3.0](https://www.npmjs.com/package/elasticsearch) - The official low-level Elasticsearch client for Node.js.
   - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
   - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API.
   - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - A Node.js module for parsing form data, especially file uploads.
   - [glob v7.1.3](https://www.npmjs.com/package/glob) - Match files using the patterns the shell uses, like stars and stuff.
   - [gm v1.23.1](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
   - [ibm-cos-sdk v1.4.1](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db v2.5.0](https://www.npmjs.com/package/ibm_db) - An asynchronous/synchronous interface for node.js to IBM DB2 and IBM Informix.
   - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - The node.js client is used for simplifying the interaction with the IBM Watson Internet of Things Platform.
   - [iconv-lite v0.4.24](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
   - [jsdom v13.1.0](https://www.npmjs.com/package/jsdom) - jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG DOM and HTML Standards.
   - [jsforce v1.9.1](https://www.npmjs.com/package/jsforce)Salesforce API Library for JavaScript applications.
   - [jsonwebtoken v8.4.0](https://www.npmjs.com/package/jsonwebtoken) - An implementation of JSON Web Tokens.
   - [lodash v4.17.11](https://www.npmjs.com/package/lodash) - The Lodash library exported as Node.js modules.
   - [marked v0.6.0](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, written in JavaScript. Built for speed.
   - [merge v1.2.1](https://www.npmjs.com/package/merge) - Merge multiple objects into one, optionally creating a new cloned object.
   - [moment v2.23.0](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
   - [mongodb v3.1.12](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
   - [mysql v2.16.0](https://www.npmjs.com/package/mysql) - This is a node.js driver for mysql.
   - [mustache v3.0.1](https://www.npmjs.com/package/mustache) - mustache.js is an implementation of the mustache template system in JavaScript.
   - [nano v7.1.1](https://www.npmjs.com/package/nano) - minimalistic couchdb driver for Node.js.
   - [nodemailer v5.1.1](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
   - [oauth2-server v3.0.1](https://www.npmjs.com/package/oauth2-server) - Complete, compliant and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
   - [openwhisk v3.18.0](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
   - [path-to-regex v3.0.0](https://www.npmjs.com/package/path-to-regexp) - Turn a path string such as /user/:name into a regular expression which can then be used to match against URL paths.
   - [pg v7.8.0](https://www.npmjs.com/package/pg) - Non-blocking PostgreSQL client for node.js. Pure JavaScript and optional native libpq bindings.
   - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'); just like any other module.
   - [pug v2.0.3](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
   - [redis v2.8.0](https://www.npmjs.com/package/redis) - This is a complete and feature rich Redis client for Node.js.
   - [request v2.88.0](https://www.npmjs.com/package/request) - Request is designed to be the simplest way possible to make HTTP calls.
   - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
   - [rimraf v2.6.3](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
   - [semver v5.6.0](https://www.npmjs.com/package/semver) - Semantic Versioning for Nodejs
   - [@sendgrid/mail v6.3.1](https://www.npmjs.com/package/@sendgrid/mail) - Provides email support via the SendGrid API.
   - [serialize-error v3.0.0](https://www.npmjs.com/package/serialize-error) - Serialize an error into a plain object.
   - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
   - [socket.io v2.2.0](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
   - [socket.io-client v2.2.0](https://www.npmjs.com/package/socket.io-client) - Realtime application framework for socket.io.
   - [superagent v4.1.0](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
   - [swagger-tools v0.10.4](https://www.npmjs.com/package/swagger-tools) - Package that provides various tools for integrating and interacting with Swagger.
   - [twilio v3.27.1](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
   - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that provides support for the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
   - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - Parse URLs for path parameters more easily than from using a regex string matcher.
   - [uuid v3.3.2](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
   - [validator v10.11.0](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
   - [vcap_services v0.6.0](https://www.npmjs.com/package/vcap_services)Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides.
   - [when v3.7.8](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
   - [winston v3.1.0](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
   - [ws v6.1.2](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
   - [xlsx v0.14.1](https://www.npmjs.com/package/xlsx) - Parser and writer for various spreadsheet formats.
   - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
   - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
   - [yauzl v2.10.0](https://www.npmjs.com/package/yauzl) - Another extraction library for node.

### Node.js version 8 environment with IBM SDKs
{: #openwhisk_ref_javascript_environments_8}
The Node.js version 8 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:8` when creating or updating an action.

Node.js version 8 is in maintenance mode and is available until December 2019. See the [Node.js release schedule](https://github.com/nodejs/Release).
{: deprecated}

#### Node.js 8.15 packages

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib v0.5.3](https://www.npmjs.com/package/amqplib) - A library for making AMQP 0-9-1 clients for Node.JS.
   - [apn v2.2.0](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
   - [async v2.6.1](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
   - [bent v1.2.0](https://www.npmjs.com/package/bent) - Functional HTTP client for Node.js w/ async/await.
   - [bodyparser v1.18.3](https://www.npmjs.com/package/body-parser) - Parse incoming request bodies in a middleware before your handlers, available under the req.body property.
   - [btoa v1.2.1](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
   - [cassandra-driver v3.6.0](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
   - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
   - [@cloudant/cloudant v2.4.1](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
   - [commander v2.19.0](https://www.npmjs.com/package/commander) - The complete solution for node.js command-line interfaces.
   - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - Address translator from Compose UI or API for Scylla databases.
   - [consul v0.34.1](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
   - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object keyed by the cookie names.
   - [elasticsearch v15.3.0](https://www.npmjs.com/package/elasticsearch) - The official low-level Elasticsearch client for Node.js.
   - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
   - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3) - A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API.
   - [formidable v1.2.1](https://www.npmjs.com/package/formidable) - A Node.js module for parsing form data, especially file uploads.
   - [glob v7.1.3](https://www.npmjs.com/package/glob) - Match files using the patterns the shell uses, like stars and stuff.
   - [gm v1.23.1](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
   - [ibm-cos-sdk v1.4.1](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db v2.5.0](https://www.npmjs.com/package/ibm_db) - An asynchronous/synchronous interface for node.js to IBM DB2 and IBM Informix.
   - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf) - The node.js client is used for simplifying the interaction with the IBM Watson Internet of Things Platform.
   - [iconv-lite v0.4.24](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
   - [jsdom v13.1.0](https://www.npmjs.com/package/jsdom) - jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG DOM and HTML Standards.
   - [jsforce v1.9.1](https://www.npmjs.com/package/jsforce)Salesforce API Library for JavaScript applications.
   - [jsonwebtoken v8.4.0](https://www.npmjs.com/package/jsonwebtoken) - An implementation of JSON Web Tokens.
   - [lodash v4.17.11](https://www.npmjs.com/package/lodash) - The Lodash library exported as Node.js modules.
   - [log4js v3.0.6](https://www.npmjs.com/package/log4js) - This is a conversion of the log4js framework to work with Node.
   - [marked v0.6.0](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, written in JavaScript. Built for speed.
   - [merge v1.2.1](https://www.npmjs.com/package/merge) - Merge multiple objects into one, optionally creating a new cloned object.
   - [moment v2.23.0](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
   - [mongodb v3.1.12](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
   - [mysql v2.16.0](https://www.npmjs.com/package/mysql) - This is a node.js driver for mysql.
   - [mustache v3.0.1](https://www.npmjs.com/package/mustache) - mustache.js is an implementation of the mustache template system in JavaScript.
   - [nano v7.1.1](https://www.npmjs.com/package/nano) - minimalistic couchdb driver for Node.js.
   - [nodemailer v4.7.0](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
   - [oauth2-server v3.0.1](https://www.npmjs.com/package/oauth2-server) - Complete, compliant and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
   - [openwhisk v3.18.0](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
   - [path-to-regex v2.4.0](https://www.npmjs.com/package/path-to-regexp) - Turn a path string such as /user/:name into a regular expression which can then be used to match against URL paths.
   - [pg v7.8.0](https://www.npmjs.com/package/pg) - Non-blocking PostgreSQL client for node.js. Pure JavaScript and optional native libpq bindings.
   - [process v0.11.10](https://www.npmjs.com/package/process) - require('process'); just like any other module.
   - [pug v2.0.3](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
   - [redis v2.8.0](https://www.npmjs.com/package/redis) - This is a complete and feature rich Redis client for Node.js.
   - [request v2.88.0](https://www.npmjs.com/package/request) - Request is designed to be the simplest way possible to make HTTP calls.
   - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
   - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
   - [semver v5.6.0](https://www.npmjs.com/package/semver) - Semantic Versioning for Nodejs
   - [@sendgrid/mail v6.3.1](https://www.npmjs.com/package/@sendgrid/mail) - Provides email support via the SendGrid API.
   - [serialize-error v3.0.0](https://www.npmjs.com/package/serialize-error) - Serialize an error into a plain object.
   - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
   - [socket.io v2.2.0](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
   - [socket.io-client v2.2.0](https://www.npmjs.com/package/socket.io-client) - Realtime application framework for socket.io.
   - [superagent v4.1.0](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
   - [swagger-tools v0.10.4](https://www.npmjs.com/package/swagger-tools) - Package that provides various tools for integrating and interacting with Swagger.
   - [twilio v3.27.1](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
   - [underscore v1.9.1](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that provides support for the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
   - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern) - Parse URLs for path parameters more easily than from using a regex string matcher.
   - [uuid v3.3.2](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
   - [validator v10.11.0](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
   - [vcap_services v0.6.0](https://www.npmjs.com/package/vcap_services)Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides.
   - [watson-developer-cloud v3.16.0](https://www.npmjs.com/package/watson-developer-cloud) - Node.js client library to use the Watson Developer Cloud services, a collection of APIs that use cognitive computing to solve complex problems.
   - [when v3.7.8](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
   - [winston v3.1.0](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
   - [ws v6.1.2](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
   - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
   - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
   - [yauzl v2.10.0](https://www.npmjs.com/package/yauzl) - Another extraction library for node.

Detailed information about the Node.js version 8 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).

### Node.js version 6 environment (deprecated)
{: #openwhisk_ref_javascript_environments_6}
Node.js version 6 is the default version, but is deprecated. To continue using a JavaScript action, update to Node.js version 8 or 10.
{: deprecated}

#### Node.js 6.16 packages

 NodeJS version:
   - [6.16.0](https://nodejs.org/en/blog/release/v6.16.0/)

 The following packages are available to be used in the Node.js 6 environment:
 - [apn v2.1.2](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
 - [async v2.1.4](https://www.npmjs.com/package/async) - Provides asynchronous function capabilities.
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
 - [yauzl v2.7.0](https://www.npmjs.com/package/yauzl) - Another extraction library for node.

Detailed information about the nodejs version 6 runtime environment can be found in the [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md).

## Python runtimes
{: #openwhisk_ref_python_environments}

You can choose from two different runtime versions for Python actions.


### Python 3.7 actions (Debian Stretch based)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 actions are executed with Python 3.7.x. To use this runtime, specify the CLI parameter `--kind python:3.7` when you create or update an action.

The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.7 standard libraries.

#### Python 3.7.2 packages

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Python packages:
 - asn1crypto==0.24.0
 - attrs==18.2.0
 - Automat==0.7.0
 - beautifulsoup4==4.7.1
 - botocore==1.12.80
 - cassandra-driver==3.16.0
 - certifi==2018.11.29
 - cffi==1.11.5
 - chardet==3.0.4
 - Click==7.0
 - cloudant==2.10.2
 - constantly==15.1.0
 - cryptography==2.4.2
 - cssselect==1.0.3
 - docutils==0.14
 - elasticsearch==6.3.1
 - etcd3==0.8.1
 - Flask==1.0.2
 - gevent==1.4.0
 - greenlet==0.4.15
 - grpcio==1.18.0
 - httplib2==0.12.0
 - hyperlink==18.0.0
 - ibm-cos-sdk==2.4.2
 - ibm-cos-sdk-core==2.4.2
 - ibm-cos-sdk-s3transfer==2.4.2
 - ibm-db==2.0.9
 - ibmcloudsql==0.2.23
 - idna==2.7
 - incremental==17.5.0
 - itsdangerous==1.1.0
 - Jinja2==2.10
 - jmespath==0.9.3
 - kafka-python==1.4.4
 - lxml==4.3.0
 - MarkupSafe==1.1.0
 - numpy==1.16.0
 - pandas==0.23.4
 - parsel==1.5.1
 - pika==0.12.0
 - Pillow==5.4.1
 - protobuf==3.6.1
 - psycopg2==2.7.6.1
 - pyarrow==0.11.1
 - pyasn1==0.4.5
 - pyasn1-modules==0.2.3
 - pycparser==2.19
 - PyDispatcher==2.0.5
 - PyHamcrest==1.9.0
 - pymongo==3.7.2
 - pyOpenSSL==18.0.0
 - python-dateutil==2.7.5
 - pytz==2018.9
 - queuelib==1.5.0
 - redis==3.0.1
 - requests==2.21.0
 - scikit-learn==0.20.2
 - scipy==1.2.0
 - Scrapy==1.5.1
 - service-identity==18.1.0
 - simplejson==3.16.0
 - six==1.12.0
 - soupsieve==1.7.1
 - tenacity==5.0.2
 - tornado==4.5.2
 - Twisted==18.9.0
 - urllib3==1.23
 - virtualenv==16.2.0
 - w3lib==1.20.0
 - watson-developer-cloud==2.5.4
 - websocket-client==0.48.0
 - Werkzeug==0.14.1
 - zope.interface==4.6.0

Detailed information about the Python 3.7 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Python 3.6.8 actions (Debian Jessie based)
{: #openwhisk_ref_python_environments_3.6}

Python 3 actions are executed with Python 3.6.x. To use this runtime, specify the CLI parameter `--kind python:3.6` when you create or update an action.

The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries.

#### Python 3.6.8 packages

Python version:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Python packages:
 - asn1crypto==0.24.0
 - attrs==18.2.0
 - autobahn==19.1.1
 - Automat==0.7.0
 - beautifulsoup4==4.7.1
 - botocore==1.12.80
 - cassandra-driver==3.16.0
 - certifi==2018.11.29
 - cffi==1.11.5
 - chardet==3.0.4
 - Click==7.0
 - cloudant==2.10.2
 - constantly==15.1.0
 - cryptography==2.4.2
 - cssselect==1.0.3
 - docutils==0.14
 - elasticsearch==5.5.3
 - Flask==1.0.2
 - gevent==1.4.0
 - greenlet==0.4.15
 - httplib2==0.12.0
 - hyperlink==18.0.0
 - ibm-cos-sdk==2.4.2
 - ibm-cos-sdk-core==2.4.2
 - ibm-cos-sdk-s3transfer==2.4.2
 - ibm-db==2.0.9
 - ibmcloudsql==0.2.23
 - idna==2.8
 - incremental==17.5.0
 - itsdangerous==1.1.0
 - Jinja2==2.10
 - jmespath==0.9.3
 - kafka-python==1.4.4
 - lxml==4.3.0
 - MarkupSafe==1.1.0
 - numpy==1.16.0
 - pandas==0.23.4
 - parsel==1.5.1
 - pika==0.12.0
 - Pillow==5.4.1
 - psycopg2==2.7.6.1
 - pyarrow==0.11.1
 - pyasn1==0.4.5
 - pyasn1-modules==0.2.3
 - pycparser==2.19
 - PyDispatcher==2.0.5
 - PyHamcrest==1.9.0
 - pymongo==3.7.2
 - pyOpenSSL==18.0.0
 - python-dateutil==2.7.5
 - pytz==2018.9
 - queuelib==1.5.0
 - redis==2.10.6
 - requests==2.21.0
 - scikit-learn==0.20.2
 - scipy==1.2.0
 - Scrapy==1.5.1
 - service-identity==18.1.0
 - simplejson==3.16.0
 - six==1.12.0
 - soupsieve==1.7.1
 - tornado==4.5.2
 - Twisted==18.9.0
 - txaio==18.8.1
 - urllib3==1.24.1
 - virtualenv==16.2.0
 - w3lib==1.20.0
 - watson-developer-cloud==1.7.1
 - Werkzeug==0.14.1
 - zope.interface==4.6.0

Detailed information about the Python 3.6 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Python 2 actions

Python 2 actions are executed with Python 2.7.15 unless you specify the `--kind` flag when you create or update an action. To explicitly select this runtime, use `--kind python:2`.

When creating python actions using virtualenv, use the docker image `openwhisk/python2action`.
The following packages are available for use by Python 2 actions, in addition to the Python 2.7 standard library.

#### Python 2 packages

 - asn1crypto==0.24.0
 - attrs==18.2.0
 - Automat==0.7.0
 - beautifulsoup4==4.6.3
 - certifi==2018.8.24
 - cffi==1.11.5
 - chardet==3.0.4
 - Click==7.0
 - constantly==15.1.0
 - cryptography==2.3.1
 - cssselect==1.0.3
 - enum34==1.1.6
 - Flask==1.0.2
 - functools32==3.2.3.post2
 - gevent==1.3.6
 - greenlet==0.4.15
 - httplib2==0.11.3
 - hyperlink==18.0.0
 - idna==2.7
 - incremental==17.5.0
 - ipaddress==1.0.22
 - itsdangerous==0.24
 - Jinja2==2.10
 - kafka-python==1.4.3
 - lxml==4.2.5
 - MarkupSafe==1.0
 - parsel==1.5.0
 - pyasn1==0.4.4
 - pyasn1-modules==0.2.2
 - pycparser==2.19
 - PyDispatcher==2.0.5
 - PyHamcrest==1.9.0
 - pyOpenSSL==18.0.0
 - python-dateutil==2.7.3
 - queuelib==1.5.0
 - requests==2.19.1
 - Scrapy==1.5.1
 - service-identity==17.0.0
 - simplejson==3.16.0
 - six==1.11.0
 - Twisted==18.7.0
 - urllib3==1.23
 - virtualenv==16.0.0
 - w3lib==1.19.0
 - Werkzeug==0.14.1
 - zope.interface==4.5.0

Detailed information about the Python 2 runtime environment can be found in the [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).

## Swift runtimes
{: #swift-actions}

Swift 3.1.1 and 4.1 runtimes are deprecated and are available until 28 February 2019.
Start any new actions or migrate any existing actions to Swift 4.2 runtime using the kind `swift:4.2` and new compile process.
{: tip}

### Swift 3
Swift 3 actions are executed with Swift 3.1.1 `--kind swift:3.1.1`. Always specify kind `swift:3.1.1` as previous versions of Swift are unsupported.

You must migrate all Swift actions to use kind `swift:3.1.1`. As a best practice, always provide the specific kind when you create or update actions.
{: tip}

Swift 3.1.1 actions can use the following packages when using a single Swift source file:
- KituraNet version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 actions can be executed using Swift 4.1 or 4.2 using `--kind swift:4.1` or `--kind swift:4.2` respectively.
The default `--kind swift:default` is Swift 4.2.

Swift 4.x action runtimes don't embed any packages, follow the instructions for [packaged swift actions](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable) to include dependencies using a Package.swift.

Swift 4.1 actions can use the following packages when using single Swift source file:
- Watson Developer Cloud SDK version 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

Swift 4.2 actions can use the following packages when using single Swift source file:
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Migrating Swift 3.1.1 to Swift 4.1

#### SwiftyJSON using a single source action file
If you have a `swift:3.1.1` action that is not compiled, and uses the **SwiftyJSON** package, you need to pre-compile your action, and specify the version of SwiftyJSON you want to use for `swift:4.2` kind action. Take into account that starting with Swift 4.1, there are improvements to managing JSON data.

## PHP runtimes
{: #openwhisk_ref_php}

PHP 7.1 and 7.2 are deprecated as of 11 January 2019. To continue using a PHP action, update to PHP 7.3.
{: deprecated}

PHP actions are executed with PHP 7.3.0. To use this runtime, specify the CLI parameter `--kind php:7.3` when you create or update an action. This behavior is the default when you create an action with a file that has a `.php` extension.

The PHP 7.1 and 7.2 runtime are deprecated. Migrate all actions to PHP 7.3 for better latency and faster runs end to end.

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

## Docker runtimes
{: #openwhisk_ref_docker}

Docker actions run a user-supplied binary in a Docker container. The binary runs in a Docker image based on [python:3.6-alpine](https://hub.docker.com/r/library/python), so the binary must be compatible with this distribution.

The Docker skeleton is a convenient way to build OpenWhisk compatible Docker images. You can install the skeleton with the `ibmcloud fn sdk install docker` CLI plug-in command.

The main binary program must be located in `/action/exec` inside the container. The executable receives the input arguments from a single command-line argument string, which can be deserialized as a `JSON` object. It must return a result by using `stdout` as a single-line string of serialized `JSON`.

You can include any compilation steps or dependencies by modifying the `Dockerfile` included in the `dockerSkeleton`.

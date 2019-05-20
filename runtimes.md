---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
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

View the available runtimes for IBM Cloud Functions in each region. The following links return a JSON response.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

The `runtimes` section of the response contains the set of available runtimes.

The `image` section contains the name of the runtime image on [Docker Hub](https://hub.docker.com/) and the tag that is used.


The following examples point to the images `ibmfunctions/action-nodejs-v10` and `openwhisk/nodejs8action`.
The tags can be version numbers like `1.9.0` or the short form of a git commit hash, like `b99d71e`.

Example image fields.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Runtimes are updated on a regular basis. These updates include security fixes and minor version updates to the packages inside the runtimes. Minor version updates might introduce backward compatibility breaks. Runtime updates might impact your actions. You must migrate actions that are running a runtime to a newer version by updating it.

Apps that run on deprecated runtimes cannot be completed successfully until the runtime is updated to a supported one. When troubleshooting a failing action, to identify whether a runtime is deprecated, check for `deprecated=true` in the query response. To update the runtime, see [changing action runtime](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

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

JavaScript actions can be executed in Node.js version 8 or 10. By default, all Node.js actions are executed in a version 10 environment.



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
   - [amqplib](https://www.npmjs.com/package/amqplib) - A library for making AMQP 0-9-1 clients for Node.JS.
   - [apn](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
   - [async](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
   - [bent](https://www.npmjs.com/package/bent) - Functional HTTP client for Node.js with async and await.
   - [bodyparser](https://www.npmjs.com/package/body-parser) - Parse incoming request bodies in a middleware before your handlers, available under the req.body property.
   - [btoa](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) - This is the official Cloudant library for Node.js.
   - [commander](https://www.npmjs.com/package/commander) - The complete solution for node.js command-line interfaces.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Address translator from Compose UI or API for Scylla databases.
   - [consul](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object keyed by the cookie names.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - The official low-level Elasticsearch client for Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
   - [etcd3](https://www.npmjs.com/package/etcd3) - A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API.
   - [formidable](https://www.npmjs.com/package/formidable) - A Node.js module for parsing form data, especially file uploads.
   - [glob](https://www.npmjs.com/package/glob) - Match files using the patterns the shell uses, like stars and stuff.
   - [gm](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - An asynchronous/synchronous interface for node.js to IBM DB2 and IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - The node.js client is used for simplifying the interaction with the IBM Watson Internet of Things Platform.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG DOM and HTML Standards.
   - [jsforce](https://www.npmjs.com/package/jsforce)Salesforce API Library for JavaScript applications.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - An implementation of JSON Web Tokens.
   - [lodash](https://www.npmjs.com/package/lodash) - The Lodash library exported as Node.js modules.
   - [marked](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, written in JavaScript. Built for speed.
   - [merge](https://www.npmjs.com/package/merge) - Merge multiple objects into one, optionally creating a new cloned object.
   - [moment](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
   - [mongodb](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
   - [mysql](https://www.npmjs.com/package/mysql) - This is a node.js driver for mysql.
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js is an implementation of the mustache template system in JavaScript.
   - [nano](https://www.npmjs.com/package/nano) - minimalistic couchdb driver for Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Complete, compliant and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - Turn a path string such as /user/:name into a regular expression which can then be used to match against URL paths.
   - [pg](https://www.npmjs.com/package/pg) - Non-blocking PostgreSQL client for node.js. Pure JavaScript and optional native libpq bindings.
   - [process](https://www.npmjs.com/package/process) - require('process'); just like any other module.
   - [pug](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
   - [redis](https://www.npmjs.com/package/redis) - This is a complete and feature rich Redis client for Node.js.
   - [request](https://www.npmjs.com/package/request) - Request is designed to be the simplest way possible to make HTTP calls.
   - [request-promise](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
   - [semver](https://www.npmjs.com/package/semver) - Semantic Versioning for Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - Provides email support via the SendGrid API.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - Serialize an error into a plain object.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Realtime application framework for socket.io.
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Package that provides various tools for integrating and interacting with Swagger.
   - [twilio](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that provides support for the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - Parse URLs for path parameters more easily than from using a regex string matcher.
   - [uuid](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
   - [validator](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
   - [vcap_services](https://www.npmjs.com/package/vcap_services)Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides.
   - [when](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
   - [winston](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
   - [ws](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
   - [xlsx](https://www.npmjs.com/package/xlsx) - Parser and writer for various spreadsheet formats.
   - [xml2js](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
   - [yauzl](https://www.npmjs.com/package/yauzl) - Another extraction library for node.

### Node.js version 8 environment with IBM SDKs
{: #openwhisk_ref_javascript_environments_8}
The Node.js version 8 environment is used if the `--kind` flag is explicitly specified with a value of `nodejs:8` when creating or updating an action.

Node.js version 8 is in maintenance mode and is available until December 2019. See the [Node.js release schedule](https://github.com/nodejs/Release).
{: deprecated}

#### Node.js 8.15 packages

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib) - A library for making AMQP 0-9-1 clients for Node.JS.
   - [apn](https://www.npmjs.com/package/apn) - A Node.js module for interfacing with the Apple Push Notification service.
   - [async](https://www.npmjs.com/package/async) - Provides functions for working with asynchronous functions.
   - [bent](https://www.npmjs.com/package/bent) - Functional HTTP client for Node.js w/ async/await.
   - [bodyparser](https://www.npmjs.com/package/body-parser) - Parse incoming request bodies in a middleware before your handlers, available under the req.body property.
   - [btoa](https://www.npmjs.com/package/btoa) - A port of the browser's btoa function.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver) - DataStax Node.js Driver for Apache Cassandra.
   - [cloudant](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant) - This is the official Cloudant library for Node.js.
   - [commander](https://www.npmjs.com/package/commander) - The complete solution for node.js command-line interfaces.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator) - Address translator from Compose UI or API for Scylla databases.
   - [consul](https://www.npmjs.com/package/consul) - A client for Consul, involving service discovery and configuration.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser) - Parse Cookie header and populate req.cookies with an object keyed by the cookie names.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch) - The official low-level Elasticsearch client for Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler) - Development-only error handler middleware.
   - [etcd3](https://www.npmjs.com/package/etcd3) - A high-quality, production-ready client for the Protocol Buffer-based etcdv3 API.
   - [formidable](https://www.npmjs.com/package/formidable) - A Node.js module for parsing form data, especially file uploads.
   - [glob](https://www.npmjs.com/package/glob) - Match files using the patterns the shell uses, like stars and stuff.
   - [gm](https://www.npmjs.com/package/gm) - GraphicsMagick and ImageMagick for Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - {{site.data.keyword.cos_full}} SDK for Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db) - An asynchronous/synchronous interface for node.js to IBM DB2 and IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf) - The node.js client is used for simplifying the interaction with the IBM Watson Internet of Things Platform.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite) - Pure JS character encoding conversion
   - [jsdom](https://www.npmjs.com/package/jsdom) - jsdom is a pure-JavaScript implementation of many web standards, notably the WHATWG DOM and HTML Standards.
   - [jsforce](https://www.npmjs.com/package/jsforce)Salesforce API Library for JavaScript applications.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - An implementation of JSON Web Tokens.
   - [lodash](https://www.npmjs.com/package/lodash) - The Lodash library exported as Node.js modules.
   - [log4js](https://www.npmjs.com/package/log4js) - This is a conversion of the log4js framework to work with Node.
   - [marked](https://www.npmjs.com/package/marked) - A full-featured markdown parser and compiler, written in JavaScript. Built for speed.
   - [merge](https://www.npmjs.com/package/merge) - Merge multiple objects into one, optionally creating a new cloned object.
   - [moment](https://www.npmjs.com/package/moment) - A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.
   - [mongodb](https://www.npmjs.com/package/mongodb) - The official MongoDB driver for Node.js.
   - [mysql](https://www.npmjs.com/package/mysql) - This is a node.js driver for mysql.
   - [mustache](https://www.npmjs.com/package/mustache) - mustache.js is an implementation of the mustache template system in JavaScript.
   - [nano](https://www.npmjs.com/package/nano) - minimalistic couchdb driver for Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer) - Send e-mails from Node.js – easy as cake!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server) - Complete, compliant and well tested module for implementing an OAuth2 Server/Provider with express in Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk) - JavaScript client library for the OpenWhisk platform. Provides a wrapper around the OpenWhisk APIs.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp) - Turn a path string such as /user/:name into a regular expression which can then be used to match against URL paths.
   - [pg](https://www.npmjs.com/package/pg) - Non-blocking PostgreSQL client for node.js. Pure JavaScript and optional native libpq bindings.
   - [process](https://www.npmjs.com/package/process) - require('process'); just like any other module.
   - [pug](https://www.npmjs.com/package/pug) - Implements the Pug templating language.
   - [redis](https://www.npmjs.com/package/redis) - This is a complete and feature rich Redis client for Node.js.
   - [request](https://www.npmjs.com/package/request) - Request is designed to be the simplest way possible to make HTTP calls.
   - [request-promise](https://www.npmjs.com/package/request-promise) - The simplified HTTP request client 'request' with Promise support. Powered by Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf) - The UNIX command rm -rf for node.
   - [semver](https://www.npmjs.com/package/semver) - Semantic Versioning for Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail) - Provides email support via the SendGrid API.
   - [serialize-error](https://www.npmjs.com/package/serialize-error) - Serialize an error into a plain object.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon) - Node.js middleware for serving a favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io) - Socket.IO enables real-time bidirectional event-based communication.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client) - Realtime application framework for socket.io.
   - [superagent](https://www.npmjs.com/package/superagent) - SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools) - Package that provides various tools for integrating and interacting with Swagger.
   - [twilio](https://www.npmjs.com/package/twilio) - A wrapper for the Twilio API, related to voice, video, and messaging.
   - [underscore](https://www.npmjs.com/package/underscore) - Underscore.js is a utility-belt library for JavaScript that provides support for the usual functional suspects (each, map, reduce, filter...) without extending any core JavaScript objects.
   - [url-pattern](https://www.npmjs.com/package/url-pattern) - Parse URLs for path parameters more easily than from using a regex string matcher.
   - [uuid](https://www.npmjs.com/package/uuid) - Simple, fast generation of RFC4122 UUIDS.
   - [validator](https://www.npmjs.com/package/validator) - A library of string validators and sanitizers.
   - [vcap_services](https://www.npmjs.com/package/vcap_services)Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides.
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud) - Node.js client library to use the Watson Developer Cloud services, a collection of APIs that use cognitive computing to solve complex problems.
   - [when](https://www.npmjs.com/package/when) - When.js is a rock solid, battle-tested Promises/A+ and when() implementation, including a complete ES6 Promise shim.
   - [winston](https://www.npmjs.com/package/winston) - A multi-transport async logging library for node.js. "CHILL WINSTON! ... I put it in the logs."
   - [ws](https://www.npmjs.com/package/ws) - ws is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation.
   - [xml2js](https://www.npmjs.com/package/xml2js) - Simple XML to JavaScript object converter. It supports bi-directional conversion.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest) - node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object.
   - [yauzl](https://www.npmjs.com/package/yauzl) - Another extraction library for node.

Detailed information about the Node.js version 8 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).


## Python runtimes
{: #openwhisk_ref_python_environments}

You can choose from two different runtime versions for Python actions. By default, all Python actions are executed in a version 2 environment.


### Python 3.7 actions (Debian Stretch based)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 actions are executed with Python 3.7.x. To use this runtime, specify the CLI parameter `--kind python:3.7` when you create or update an action.

The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.7 standard libraries.

#### Python 3.7.2 packages

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Python packages:
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Detailed information about the Python 3.7 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Python 3.6.8 actions (Debian Jessie based)
{: #openwhisk_ref_python_environments_3.6}

Python 3 actions are executed with Python 3.6.x. To use this runtime, specify the CLI parameter `--kind python:3.6` when you create or update an action.

The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries.

#### Python 3.6.8 packages

Python version:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Python packages:
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Detailed information about the Python 3.6 runtime environment can be found in the [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Python 2 actions

Python 2 actions are executed with Python 2.7.15 unless you specify the `--kind` flag when you create or update an action.

When creating python actions using virtualenv, use the docker image `openwhisk/python2action`.
The following packages are available for use by Python 2 actions, in addition to the Python 2.7 standard library.

#### Python 2 packages

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Detailed information about the Python 2 runtime environment can be found in the [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).


## Swift runtime
{: #swift-actions}

By default, all Swift actions are executed in a version 4.2 environment.

Swift 4.x action runtimes don't embed any packages, follow the instructions for [packaged swift actions](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) to include dependencies using a Package.swift.

Swift 4.2 actions can use the following packages when using single Swift source file:
- Watson Developer Cloud SDK version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### SwiftyJSON using a single source action file
If you have an action that is not compiled, and uses the **SwiftyJSON** package, you need to pre-compile your action, and specify the version of SwiftyJSON you want to use for `swift:4.2` kind action.


## PHP runtime
{: #openwhisk_ref_php}

By default, all PHP actions are executed in a version 7.3 environment.

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

## Docker runtime
{: #openwhisk_ref_docker}

Docker actions run a user-supplied binary in a Docker container. The binary runs in a Docker image based on [python:3.6-alpine](https://hub.docker.com/r/library/python), so the binary must be compatible with this distribution.

The Docker skeleton is a convenient way to build OpenWhisk compatible Docker images. You can install the skeleton with the `ibmcloud fn sdk install docker` CLI plug-in command.

The main binary program must be located in `/action/exec` inside the container. The executable receives the input arguments from a single command-line argument string, which can be deserialized as a `JSON` object. It must return a result by using `stdout` as a single-line string of serialized `JSON`.

You can include any compilation steps or dependencies by modifying the `Dockerfile` included in the `dockerSkeleton`.



## Go runtime
{: #runtimes_go}

By default, all Go actions are executed in a version 1.11 environment.



## Java runtime
{: #runtimes_java}

By default, all Java actions are executed in a version 8 environment.



## Ruby runtime
{: #runtimes_ruby}

By default, all Ruby actions are executed in a version 2.5 environment.



## .NET Core runtime
{: #runtimes_dotnet}

By default, all .NET Core actions are executed in a version 2.2 environment.

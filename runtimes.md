---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-30"

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

Your apps can be coded and executed in programming languages such as JavaScript or Python. Many runtimes are available by default with {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Available images and runtimes
{: #runtimes_available}

View the available runtimes for IBM Cloud Functions in each region. The following links return a JSON response. The `runtimes` section of the response contains the set of available runtimes. The `image` section contains the name of the runtime image on [Docker Hub](https://hub.docker.com/){: external} and the tag that is used.

  - [`us-south`](https://us-south.functions.cloud.ibm.com/){: external}
  - [`us-east`](https://us-east.functions.cloud.ibm.com/){: external}
  - [`eu-gb`](https://eu-gb.functions.cloud.ibm.com/){: external}
  - [`eu-de`](https://eu-de.functions.cloud.ibm.com/){: external}
  - [`jp-tok`](https://jp-tok.functions.cloud.ibm.com/){: external}

The following examples point to the images `ibmfunctions/action-nodejs-v10` and `openwhisk/nodejs8action`.
The tags can be version numbers such as `1.9.0` or the short form of a Git commit hash, like `b99d71e`.

Example image fields.

  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Runtimes are updated regularly. These updates include security fixes and minor version updates to the packages inside the runtimes. Minor version updates might introduce breaks in compatibility with earlier versions. Runtime updates might impact your actions. You must migrate actions that are running a runtime to a newer version by updating it.

Apps that run on deprecated runtimes cannot be completed successfully until the runtime is updated to a supported one. When troubleshooting a failing action to identify whether a runtime is deprecated, check for `deprecated=true` in the query response. To update the runtime, see [changing action runtime](/docs/openwhisk?topic=cloud-functions-actions#actions_update).

These runtimes are deprecated:
<ul>
  <li><code>nodejs:8</code> (deprecated)</li>
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

Node.js version 8 is deprecated and will soon be removed. To continue running your actions, you must update any Node.js version 8 actions to a higher runtime version. For more information, see the [Node.js release schedule](https://github.com/nodejs/Release){: external}.
{: deprecated}

| Runtime | Description | Changelog |
| --- | --- | --- | 
| [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/){: external} | By default, all Node.js actions are executed in a version 10 environment. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md){: external}. |
| [8.15.0](https://nodejs.org/en/blog/release/v8.15.0/){: external} | Deprecated | [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md){: external}. |


### Migrating from nodeJS 8 to nodeJS 10

| Package | Details |
| --- | --- |
| `cloudant` | The `cloudant` NPM package is not available in `nodejs:10`. The package is deprecated. You need to use the official NPM package [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant){: external} v3.0.0 when importing the Node.js module (that is, `require('@cloudant/cloudant')`) also [v3.x only returns Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x){: external}. |
| `cradle` | The `cradle`  NPM package is not available in `nodejs:10`. |
| `watson-developer-cloud` | The `watson-developer-cloud` NPM package available in nodejs:10 is version 4.x. This version includes support for Promises. A list of the changes that are made is documented. Note that this package is deprecated and is no longer supported. This package will not receive updates and will be removed in the future. Move your action to the `ibm-watson` package.
| `ibm-watson` | The `ibm-watson` package available in `nodejs:10` is version 4.x. This package is the successor of the `watson-developer-cloud` package. Upgrade your action code to use this new package, since the former will not receive updates anymore. This package includes support for promises. |
| `ibmiotf` | The package `ibmiotf` has been renamed by the maintainers to @wiotp/sdk. Make sure to update your action code to the new package. See [@wiotp/sdk](https://www.npmjs.com/package/@wiotp/sdk){: external} for all changes. The package `ibmiotf` will not receive further updates and will be removed from this runtime in the future. |


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
| [`ibmwatson`](https://www.npmjs.com/package/ibm-watson){: external} | Node.js client library to use the Watson APIs. |
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
| [`uuid`](https://www.npmjs.com/package/uuid){: external} | Simple, fast generation of RFC4122 UUIDS. |
| [`validator`](https://www.npmjs.com/package/validator){: external} | A library of string validators and sanitizers. |
| [`vcap_services`](https://www.npmjs.com/package/vcap_services){: external} | Parse and return service credentials from VCAP_SERVICES environment variable that IBM Cloud provides. |
| [`when`](https://www.npmjs.com/package/when){: external} | When.js is a rock solid, battle-tested `Promises/A+` and `when()` implementation, including a complete ES6 Promise shim. |
| [`winston`](https://www.npmjs.com/package/winston){: external} | A multi-transport async logging library for Node.js. "CHILL WINSTON! ... I put it in the logs." |
| [`ws`](https://www.npmjs.com/package/ws){: external} | `ws` is a simple to use, blazing fast, and thoroughly tested WebSocket client and server implementation. |
| [`xlsx`](https://www.npmjs.com/package/xlsx){: external} | Parser and writer for various spreadsheet formats. |
| [`xml2js`](https://www.npmjs.com/package/xml2js){: external} | Simple XML to JavaScript object converter. It supports bidirectional conversion. |
| [`xmlhttprequest`](https://www.npmjs.com/package/xmlhttprequest){: external} | node-XMLHttpRequest is a wrapper for the built-in http client to emulate the browser XMLHttpRequest object. |
| [`yauzl`](https://www.npmjs.com/package/yauzl){: external} | Another extraction library for node. |





## Python runtimes
{: #openwhisk_ref_python_environments}

By default, all Python actions are executed in a Python version 3.7 environment.
{: note}

| Kind | Python version | Description | Changelog |
| --- | --- | --- | --- |
| &nbsp; | 2.7 | Python 2.7 reached `end of support` on 2020/01/01.<br/>See [the Active Python Releases](https://www.python.org/downloads/){: external}. | &nbsp; |
| python:3.6 | [3.6.x](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile){: external} | Python 3 actions are executed with Python 3.6.x. To use this runtime, specify the CLI parameter `--kind python:3.6` when you create or update an action. The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.6 standard libraries. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md){: external}. |
| python:3.7 | [3.7.x](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile){: external} | By default, all Python actions are executed in a Python version 3.7.x environment (Debian Stretch based) unless you specify the --kind flag when you create or update an action. To explicitly use this runtime, specify the CLI parameter `--kind python:3.7` when you create or update an action. The runtime contains SDK packages for IBM Cloud services available for use by Python actions, in addition to the Python 3.7 standard libraries. | [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md){: external}. |


### Python packages

Ensure that your action uses only the packages mentioned in the following table.<br/>
While other Python packages might be part of the runtime, they are included only as indirect dependencies of the other listed packages. These unlisted packages are candidates to be removed as soon as they are not required by the refering package.
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

For more information about Python 3.7 packages, see [(Details on Github)](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/requirements.txt){: external}.

For more information about Python 3.6 packages, see [(Details on Github)](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/requirements.txt){: external}.

## Swift runtime
{: #swift-actions}

By default, all Swift actions are executed in a version 4.2 environment.
{: note}

Swift 4.x action runtimes don't embed any packages, follow the instructions for [packaged Swift actions](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) to include dependencies by using a Package.swift.

Swift 4.2 actions can use the following packages when you use a single Swift source file:
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

Docker actions run a user-supplied executable in a Docker container. You can include any compilation steps or dependencies by modifying your `Dockerfile`. You can specify a Docker image to use with your action code by specifying the `--docker` flag when you run the [`action create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create) command. When you create an action that uses a Docker image, your app runs in the public Docker image that you specify during action creation.

{{site.data.keyword.openwhisk_short}} provides base images on Docker hub. You can use these images as-is when creating actions, or you can use them as a base image when writing your `Dockerfile`. You can see a list of the available runtimes images in the [Available images and runtimes](#runtimes_available) section.

{{site.data.keyword.openwhisk_short}} actions can be created by using public images on from Docker Hub.
{: note}



For more information about creating actions with Docker images, see [Preparing apps in Docker images](/docs/openwhisk?topic=cloud-functions-prep#prep_docker).


## More runtime support

**Go**

By default, all Go actions are executed in a version 1.11 environment. 

**Java**

By default, all Java actions are executed in a version 8 environment.

**Ruby**

By default, all Ruby actions are executed in a version 2.5 environment.

**.NET Core**

By default, all .NET Core actions are executed in a version 2.2 environment.
